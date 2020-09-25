# CI/CD demo

### 1. AWS CodeCommit (Source Code Stage)

  ***Initialize your Git repository***
  
  ***1.*** Download [Git](https://git-scm.com/downloads)

  ***2.*** Download this repository as zip and extract it.
  
  ***3.*** From inside the extracted folder, run the below:
  
    git init
    git add . 
    git commit -m "first commit"
  
  ***Get the Git credentials for AWS CodeCommit***
  
  ***4.*** Create an IAM user
  
  ***5.*** Attach with it AWSCodeCommitPowerUser AWS managed policy
  
  ***6.*** Clink on ***Security Credentials** tab --> under ***HTTPS Git credentials for AWS CodeCommit** click on ***Generate credentials***
  
  ***7.*** Download the csv to save these credentials
 
   ***Create your CodeCommit repository***
   
  ***8.*** Go to AWs CodeCommit service --> Create repository --> enter a repository name and click ***Create***
  
  ***9.*** Click on Clone URL --> Clone HTTPS
  
  ***10.*** Back to the command line, enter the below:
  
      git push <The link you copied> master
 
  It will ask you for a username and password, enter the ones your downloaded on Step 7.
  
  ***11.*** Refresh your CodeCommit page and you shoud see the code files there
  
  

### 2. AWS CodeBuild (Test & Build)


  ***12.*** Create a bucket to save the build files (build artifacts) 
  
  ***13.*** Go to AWS CodeBuild Service --> build projects and click ***Create build project*** and enter the below:
  * ***Project name:*** any name you like
  * ***Source provider:*** AWS CodeCommit
  * ***Repository:*** choose the repository you create earlier in AWS CodeCommit
  * ***Branch:*** master
  * ***Environment image:*** Managed image
  * ***Operating system:*** Amazon Linux 2
  * ***Runtime(s):*** Standard
  * ***Image:*** aws/codebuild/amazonlinux2-x86_64-standard:3.0
  Under ***Artifacts***
  * ***Type:*** Amazon S3
  * ***Bucket name:*** choose the bucket you created in step 12.
  * ***Name:*** choose any name, but remember it for later. add ***.zip*** to it to be zipped
  * ***Artifacts packaging:*** Zip
  
  Click ***Create build project***
  
  ***14.*** Click ***Start build***, leave all default settings and click ***Start build***
  
  ***15.*** Click ***Phase details*** to check the status and wait until it is completed.
  
  ***16.*** Go back to your S3 bucket to confirm the files are there. If you download and extract the zip files, you should find a folder with a .war file, scripts folder, and the appsepc.yml file.
  
  
### 3. AWS CodeDeploy (DEPLOY!)

***Launch an EC2 instance***

  ***17.*** Create an EC2 role with AmazonEC2RoleforAWSCodeDeploy policy attached.
  
  ***18.*** Luanch an Amazon Linux 2 EC2 instance, ensure below are set:
  * ***Auto-assign Public IP:*** Enable
  * ***IAM role:***  choose the role create in the prvious step
  * ***Tag:*** create a tag for the instance and remember it for later
  * ***Security Group:*** ensure it have a rule to all 80  from anywhere


***A. Create Application***

  ***19.*** Go to AWS CodeDeploy --> Applications --> click ***Create application*** and enter the below:
* ***Application name:*** enter any name
* ***Compute platform:*** EC2/On-premises



***B. Create Deployment Group***

  ***20.*** Create an IAM role for CodeDeploy with a AWSCodeDeployRole AWS managed policy attached
  
  ***21.*** Click ***Create deployment group*** and enter the below:
  * ***Enter a deployment group name:*** enter any name
  * ***Enter a service role:*** choose the role you created in the previous step
  * ***Deployment type:*** In-place
  * ***Environment configuration:*** Amazon EC2 instances
  * ***Tag group 1:*** choose the key and value that you create for the instance laucnhed in step 18.
  * ***Enabled load balancing:*** uncheck this option
  
  Leave all other configrations as default and click ***Create deployment group**
  

***C. Create Deployment***

  ***22.*** Click ***Create deployment** and set the below:
  * ***Deployment group:*** chose the deployment group you just created
  * ***Revision type:*** My application is stored in Amazon S3
  * ***Revision location:*** enter the location of the CodeBuild artifacts as s3://"Bucket Name"/"Buildartifact.zip"
  * ***Revision type:*** .zip
  
  Leave all other as default and click ***Create deployment***
  
  This will start the deployment, you can click on the ***view events** to see the status. When done, go grap your EC2 public IP and put in the browser and you should see you application running. ***Congratulations !!***
  
  
  
### 4. AWS CodePipleine 

I'll leave this to you, it is super simple. Go to AWS CodePipeline, create pipeline, and then just add the three stage you created above, and Viola.
Now go change in the code and check it, it should initiate the whole stages automatically

