# Webhosting on AWS S3

## Create VUE or React app

- this guides provides instructions for VUE and React projects. Other projects can be used, but you will need to determine how to edit the `buildspec.yml` file in the next step to work with your porject
- [Create React Project](https://reactjs.org/docs/create-a-new-react-app.html)
- [Create Vue Project](https://cli.vuejs.org/guide/creating-a-project.html)

## Changes to your Repo

- a buildspec.yml must be added to your repo
- download this file and add it at the root directory
- [buildspec.yml · GitHub](https://gist.github.com/aaronjenkins/197e46cb58dd77c2ba4a817139f4d856)
- be sure to cun comment the base-directory line that is appropraite for your app
  - 'dist' for VUE
  - 'build' for React
- dont forget to check in this new file to your repo
- [Build specification reference for CodeBuild - AWS CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
  ![](WebHostingOnS3ScreenShots/buildspecymllocation.png)

## Setting up your S3 Bucket

1. create the bucket \
   ![](WebHostingOnS3ScreenShots/s3bucketcreation.png)
2. set up as website

- go to Properties > Static Webhosting > 'use this bucket to host a website' & enter index.html\
   ![](WebHostingOnS3ScreenShots/enablestaticwebhosting1.png)
   ![](WebHostingOnS3ScreenShots/enablestaticwebhosting2.png)
- Rememebr the generated URL, it will be how to access the website later

3. Edit Permissions

- Go to Permissions > Bucket Policy and paste in the content of this
- [s3 public bucket policy · GitHub](https://gist.github.com/aaronjenkins/a4d0791944ad0e83c7658fc9c1a88514)
- be sure to change 'yourbucketnamehere' to that name of the bucket you created
![](WebHostingOnS3ScreenShots/s3bucketpolicy.png)

## Pipeline: Create the Pipeline

1. Pipeline Name

- the name of your app
- Service Role: New Service Role
- Role Name: leave default
- Allow AWS Code Pipeline to create a service role so it can be used with this new pipeline : true
- Advanced Settings:
  - Artifact Store: Default
  - Encryption Key: default AWS Managed key
![](WebHostingOnS3ScreenShots/pipelinesetup-name.png)

2. Source

- choose Github and link your github account so you can access your repos easily
- repository: your app repo
- Branch: master
- Detection Options: GitHub Webhooks
![](WebHostingOnS3ScreenShots/pipelinesetup-source1.png)
![](WebHostingOnS3ScreenShots/pipelinesetup-source2.png)

3. Create the Build

- Choose AWS Code Build
- Click the create project button
- Give it a name you will remember\
![](WebHostingOnS3ScreenShots/pipelinesetup-buildstage1.png)
- Choose the following under Environment section
  - Managed Image
  - Operating System: Amazon Linux 2
  - Runtime: Standard
  - Image: aws/codebuild/amazonlinux2-x86_64-standard:2.0
  - Image Version: Always use the latest
  - Environment Type: Linux
  - Service Role: New Service Role
  - Role Name: (leave as default)
- Build Spec - choose 'use buildspec file'\
![](WebHostingOnS3ScreenShots/pipelinesetup-buildstage2.png)

4. Deploy

- Provider: Amazon S3
- Region: US East (Ohio) -or- closest to you
- Bucket: public bucket you created earlier
- Deployment path: leave blank
- Extract file before deploy: true
![](WebHostingOnS3ScreenShots/pipelinesetup-deploystage.png)

5. Review & Create
- Click create pipeline at the bottom of step 5 and that should be it, go to the url you got from step 2 of setting up yout bucket
