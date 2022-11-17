Once you have ensured the [Automated CNDA Pipeline](https://childbrainlab.github.io/tutorials/cnda_wt/) is set up, and you're pulling data successfully onto Moochie, you're ready to begin the steps to processing the fMRI data. 

## Data Management
Before getting into the full on pre-processing algorithms, there are some steps we take to ensure that data are managed properly, and transferred to the CHPC server.

### Updating Moochie's Sourcedata
`sourcedata` is a folder within a BIDS repository that stores the raw, unaltered MRI data in DICOM format. On Moochie, this folder is located in `/data/perlman/moochie/analysis/STUDY_NAME/MRI_data_clean/sourcedata`. This data can be automatically organized here via an insertion of a command to your `crontab` file.
Edit your `crontab` file with the following command:

    crontab -e
    
And insert the following contents:

```
# at 1 AM every Saturday, duplicate the NP1166 CNDA folder's contents (with some renaming) to the BIDS directory on Moochie
0 1 * * 6 bash /data/perlman/moochie/github/LCBDtools/scripts/MRI/bids/update_sourcedata.sh CARE
```

### Syncing the Sourcedata Folder to CHPC
We will also want this folder to be sync'd to the `sourcedata` folder on CHPC, where all of the subsequent processing will take hold. This too can be accomplished via a single-line insertion to your `crontab` file. 
    
Edit your `crontab` file with the following command:

    crontab -e
    
And insert the following contents:

```
# at 1 AM every Sunday, use 'rsync' to synchronize the data with the sourcedata folder on CHPC
0 1 * * 7 bash /data/perlman/moochie/github/LCBDtools/scripts/MRI/transfer_CAREMRI_to_chpc.sh >> ~/transfer_subs_`data + '%m_%d_%Y'`.txt
```

The formatted data will now synchronize to CHPC every Sunday morning. 

### BIDS Formatting
You'll first want a list of subjects who have yet to be formatted in the BIDS dataset. I.e., they have a session in `/sourcedata`, but that session isn't BIDS-formatted. 

There is a script for generating a list of un-BIDS'd subjects. To navigate to it, run this command:
    
    cd /home/claytons/LCBDtools/scripts/MRI/bids
    
And run the following command:

    python3 get_bids_participants.py --help

This will print information about the required and optional arguments for the script. To run,

    python3 get_bids_participants.py --data_folder /scratch/claytons/MRI_data_clean

The script for submitting [BIDSkit](https://github.com/jmtyszka/bidskit) jobs on CHPC is located in `/home/claytons/LCBDtools/scripts/MRI/sbatch/bidskit_sbatch.sh`.

To use the script, navigate to the sbatch directory with the following command:

    cd /home/claytons/LCBDtools/scripts/MRI/sbatch
    
And submit a BIDSKIT job with the following command, substituting <SUBJECT_ID> with the intended subject number:

    sbatch bidskit_sbatch.sh /scratch/claytons/MRI_data_clean/ <SUBJECT_ID>
    
Or, alternatively, if you'd like to loop over all of the participants you generated in the previous step, you could use a bash loop, as follows:

    for sub in `cat ~/bids_list.txt`; do sbatch bidskit_sbatch.sh /scratch/claytons/MRI_data_clean/ $sub; done
    
This will submit a job to CHPC. You can view activate jobs with the following command:

    squeue -u USERID
    
## Preprocessing
Once all of the data management steps are completed, you should have a BIDS-formatted dataset. A subject's data will be converted to NIFTI from DICOM, and stored in the BIDS directory under `sub-<SUBJECT_NUMBER>`, with subfolders for `anat`, `func`, `dwi`, and any other modalities collected. A subject with complete BIDS data is ready to be preprocessed with [FMRIPREP](https://fmriprep.org/en/stable/).

### Generate List of Subjects for FMRIPREP
Only subjects who have valid functional scans need to be run through FMRIPREP. To generate the list, navigate to the script directory:

    cd /home/claytons/LCBDtools/scripts/MRI/fmriprep
    
And run the following command:

    python3 get_fmriprep_participants.py --help

This will print information about the required and optional arguments for the script. To run,

    python3 get_fmriprep_participants.py --data_folder /scratch/claytons/MRI_data_clean
    
This will generate a list of subject names that need to be run for FMRIPREP, saved to `~/participant_list.txt`. 

### FMRIPREP
Running FMRIPREP is very similar to running the BIDS conversion script. Navigate to the script directory with the following command:

    cd /home/claytons/LCBDtools/scripts/MRI/sbatch
    
And submit FMRIPREP jobs with the following, substituting <SUBJECT_NUMBER> with a valid subject ID:

    sbatch singularity_fmriprep_sbatch.sh /scratch/claytons/MRI_data_clean/ <SUBJECT_NUMBER>
    
Or, alternatively, if you'd like to loop over all of the participants you generated in the previous step, you could use a bash loop, as follows:

    for sub in `cat ~/participant_list.txt`; do sbatch singularity_fmriprep_sbatch.sh /scratch/claytons/MRI_data_clean/ $sub; done
    
## Post-Processing

The post-processing steps have been bundled into a single sbatch script, found in `LCBDtools/scripts/MRI/sbatch`, which accomplishes several tasks in one fell swoop.

- exporting regressors from the FMRIPREP confounds.txt file
- placing cropped versions of CARE movie-related regressors in the subjects' functional directories
- making motion spike regressors via the FSL tool, `fsl_motion_outliers`
- smoothing the functional data with a 7mm gaussian kernel, and exporting in unarchived NIFTI format

### Post-Processing Batch
To run the post-processing step, navigate to the sbatch directory:
    
    cd /home/claytons/LCBDtools/scripts/MRI/sbatch

And run the following command:

    sbatch post_processing_sbatch.sh /scratch/claytons/MRI_data_clean/
    
This will start quite a few jobs. The first steps are taken care of on a single node, but the smoothing and unarchiving will each occur on its own node. So, in reality, you're submitting as many jobs as there are subjects who need to be smoothed. This is noteworthy due to the 40 jobs / user cap on CHPC, meaning you may need to execute this command several times before all of the subjects have been thoroughly processed.

### Evaluating Functional Runs for Quality
Now that FMRIPREP is complete, and we've run the post-processing script, we should evaluate the confounds file in the FMRIPREP output to ensure that subjects are not moving excessively. Similarly to the script which evaluates the subjects who need to be run through FMRIPREP, there is a Python script which can generate a list of valid subjects for us. First, navigate to the FSL directory:

    cd /home/claytons/LCBDtools/scripts/MRI/fsl

Then, execute the script's help text with the following command:

    python3 get_fsl_subs.py --help
    
Once you have read over the required and optional arguments, you can submit the evaluation with the following:

    python3 get_fsl_subs.py --data_folder /scratch/claytons/MRI_data_clean
    
A list of subjects who pass the framewise-displacement motion criteria will be printed out to `~/fsl_subs.txt`. These are subjects who should be used in analysis moving forward. However, you should also evaluate the individual quality of the registration and T1 images using the `.html` output in the fmriprep derivatives folder, located at `/scratch/claytons/MRI_data_clean/derivatives/fmriprep`. Each subject has its own `.html` file, which can be opened in a web browser and inspected. It details information about the relevant confounds, displays images on the registration and field map interpretations, and will inform you of dropout and other MR artifacts. Any subjects with highly abnormal data shown here should also be excluded from analysis. 
