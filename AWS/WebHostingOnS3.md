# Webhosting on AWS S3

## Changes to your Repo
- a buildspec.yml must be added to your repo 
- download this file and add it at the root directory 
- [buildspec.yml · GitHub](https://gist.github.com/aaronjenkins/197e46cb58dd77c2ba4a817139f4d856)
- be sure to cun comment the base-directory line that is appropraite for your app 
  - 'dist' for VUE
  - 'build' for React
- dont forget to check in this new file to your repo
- [Build specification reference for CodeBuild - AWS CodeBuild](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)

## Setting up your S3 Bucket
1. create the bucket 
2. set up as website 
  - go to Properties > Static Webhosting > 'use this bucket to host a website' & enter index.html
  - Rememebr the generated URL, it will be how to access the website later
3. Edit Permissions
  - Go to Permissions > Bucket Policy and paste in the content of this 
  - [s3 public bucket policy · GitHub](https://gist.github.com/aaronjenkins/a4d0791944ad0e83c7658fc9c1a88514)
  - be sure to change 'yourbucketnamehere' to that name of the bucket you created

## Pipeline: Create the Pipeline
1. Pipeline Name
  - the name of your app 
  - Service Role: New Service Role
  - Role Name: leave default
  - Allow AWS Code Pipeline to create a service role so it can be used with this new pipeline : true
  - Advanced Settings: 
    - Artifact Store: Default
    - Encryption Key: default AWS Managed key
2. Source 
  - choose Github and link your github account so you can access your repos easily 
  - repository: your app repo 
  - Branch: master
  - Detection Options: GitHub Webhooks
3. Create the Build
  - Choose AWS Code Build
  - Click the create project button 
  - Give it a name you will remember
  - Choose the following under Environment section 
    - Managed Image
    - Operating System: Amazon Linux 2
    - Runtime: Standard
    - Image: aws/codebuild/amazonlinux2-x86_64-standard:2.0
    - Image Version: Always use the latest
    - Environment Type: Linux
    - Service Role: New Service Role
    - Role Name: (leave as default)
  - Build Spec - choose 'use buildspec file'
4. Deploy
  - Provider: Amazon S3
  - Region: US East (Ohio) -or- closest to you 
  - Bucket: public bucket you created earlier
  - Deployment path: leave blank
  - Extract file before deploy: true
5. that should be it, go to the url you got from step 2 of setting up yout bucket


