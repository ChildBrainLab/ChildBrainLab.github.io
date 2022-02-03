# [childbrainlab.github.io](https://www.childbrainlab.github.io)

Pages and walkthroughs intended for getting new lab members set up, documenting different lab-related software, or for writing tutorials.

Everything is written in [markdown](https://www.markdownguide.org/basic-syntax/), built into a webpage via [MkDocs](https://www.mkdocs.org/) and stylized with [SquidFunk's Material Theme](https://squidfunk.github.io/mkdocs-material/).

## Testing and Deployment
Testing:
> To host a local instance, run `mkdocs serve` and follow the localhost address in your web browswer of choice. 

Deployment:
> Deployments are set up to rebuild the live website on each use of `git push` to the main GitHub branch, via [this workflow](workflows/ci.yml), so you won't need to deploy manually with mkdocs. 
> If you do need to deploy manually, or the yaml workflow is broken, you can invoke the following: `mkdocs gh-deploy --force`. 
