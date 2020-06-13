[![Gitpod Ready-to-Code](https://img.shields.io/badge/Gitpod-Ready--to--Code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/aws-samples/sagemaker-workshop) 

# Amazon SageMaker Workshop

The Amazon SageMaker Workshop is publicly available at https://sagemaker-workshop.com/. 

### Setup:

#### Install Hugo:
On a mac:

`brew install hugo`

On Linux:
  - Download from the releases page: https://github.com/gohugoio/hugo/releases/tag/v0.37
  - Extract and save the executable to `/usr/local/bin`

#### Download Hugo themes:
Download the Hugo Theme Learn submodule:  
`git submodule update --recursive --init`

#### Install node packages:
`npm install`

#### Run Hugo locally:
`npm run server`
or
`npm run test` to see stubbed in draft pages.

#### View Hugo locally:
Visit http://localhost:1313/ to see the site.

#### Making Edits:
As you save edits to a page, the site will live-reload to show your changes.

#### Auto Deploy:
Any commits to master will auto build and deploy in a couple of minutes. You can see the currently deployed hash at the bottom of the menu panel.

note: shift-reload may be necessary in your browser to reflect the latest changes.

## License Summary

The documentation is made available under the Creative Commons Attribution-ShareAlike 4.0 International License. See the LICENSE file.

The sample code within this documentation is made available under the MIT-0 license. See the LICENSE-SAMPLECODE file.
