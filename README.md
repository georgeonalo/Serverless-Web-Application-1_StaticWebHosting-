# Module 1: Static Web Hosting with AWS Amplify Console

In this module you'll configure AWS Amplify Console to host the static resources for your web application. In subsequent modules you'll add dynamic functionality to these pages using JavaScript to call remote RESTful APIs built with AWS Lambda and Amazon API Gateway.

## Architecture Overview

The architecture for this module is very straightforward. All of your static web content including HTML, CSS, JavaScript, images and other files will be managed by AWS Amplify Console and served via Amazon CloudFront. Your end users will then access your site using the public website URL exposed by AWS Amplify Console. You don't need to run any web servers or use other services in order to make your site available.

![image](https://user-images.githubusercontent.com/115881685/208917172-f6b03e3e-8b8b-41d5-8c93-9edd68a1befe.png)

### Implementation Instructions

❗ Ensure you've completed the setup guide: ([https://github.com/georgeonalo/Serverless-Web-Application-0_Setup-]) before beginning the workshop.

Each of the following sections provides an implementation overview and detailed, step-by-step instructions. The overview should provide enough context for you to complete the implementation if you're already familiar with the AWS Management Console or you want to explore the services yourself without following a walkthrough.

#### Region Selection

This project step can be deployed in any AWS region that supports the following services:

AWS Amplify Console

AWS CodeCommit

You can refer to the AWS region table in the AWS documentation to see which regions have the supported services. Among the supported regions you can choose are:

North America: N. Virginia, Ohio, Oregon

Europe: Ireland, London, Frankfurt

Asia Pacific: Tokyo, Seoul, Singapore, Sydney, Mumbai

Once you've chosen a region, you should deploy all of the resources for this workshop there. Make sure you select your region from the dropdown in the upper right corner of the AWS Console before getting started.

![image](https://user-images.githubusercontent.com/115881685/208920461-ccde2ad0-04e6-49eb-9e90-861c16dc0e59.png)

##### Create the git repository

You have two options for this first step which is to either use AWS CodeCommit or GitHub to host your site's repository. The choice is yours. If you have a GitHub account feel free to use that. Otherwise CodeCommit is included in the AWS Free Tier

###### Using CodeCommit

The AWS Cloud9 development environment comes with AWS managed temporary credentials that are associated with your IAM user. You use these credentials with the AWS CLI credential helper. Enable the credential helper by running the following two commands in the terminal of your Cloud9 environment.

git config --global credential.helper '!aws codecommit credential-helper $@'

git config --global credential.UseHttpPath true

Next you need to create the repository and clone it to your Cloud9 environment:

1. Open the AWS CodeCommit console

2. Select Create Repository

3. Set the Repository name* to "wildrydes-site"

4. Select Create

5. From the Clone URL drop down, select Clone HTTPS

Now from your Cloud9 development environment:

1. From a terminal window run git clone and the HTTPS URL of the respository:

ec2-user:~/environment $ git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site

Cloning into 'wildrydes-site'...

warning: You appear to have cloned an empty repository.

ec2-user:~/environment $ 

###### Populate the git repository

Once your git repository is created and cloned locally, you'll need to pull in the files for your website and sync them up to the repository.

✅ Step-by-step directions From your Cloud9 development environment(or local environment)

1. Change directory into your repository:

cd wildrydes-site/

2. Copy the files from S3:

aws s3 cp s3://wildrydes-us-east-1/WebApplication/1_StaticWebHosting/website ./ --recursive

3. Commit the files to your git service (you might need to enter an email and user name for the commit):

$ git add .

$ git config --global user.email "<EMAIL ADDRESS>"
  
$ git config --global user.name "<USER NAME>"
  
$ git commit -m "initial checkin of website code"
  
$ git push

Username for 'https://git-codecommit.us-east-1.amazonaws.com': wildrydes-codecommit-at-xxxxxxxxx
  
Password for 'https://wildrydes-codecommit-at-xxxxxxxxx@git-codecommit.us-east-1.amazonaws.com': 
  
Counting objects: 95, done.
  
Compressing objects: 100% (94/94), done.
  
Writing objects: 100% (95/95), 9.44 MiB | 14.87 MiB/s, done.
  
Total 95 (delta 2), reused 0 (delta 0)
  
To https://git-codecommit.us-east-1.amazonaws.com/v1/repos/wildrydes-site
  
   * [new branch]      master -> master

##### Deploy the site with the AWS Amplify Console 
