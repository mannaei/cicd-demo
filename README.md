# CI/CD demo

### 1. AWS CodeCommit (Source Code Stage)

  ***Initialize your Git repository***
  
  ***A.*** Download [Git](https://git-scm.com/downloads)

  ***B.*** Download this repository as zip and extract it.
  
  ***C.*** From inside the extracted folder, run the below:
  
    git init
    git add . 
    git commit -m "first commit"
  
  ***Get the Git credentials for AWS CodeCommit***
  
  ***D.*** Create an IAM user
  
  ***E.*** Attach with it AWSCodeCommitPowerUser AWS managed policy
  
  ***F.*** Clink on ***Security Credentials** tab --> under ***HTTPS Git credentials for AWS CodeCommit** click on ***Generate credentials***
  
  ***G.*** Download the csv to save these credentials
 
   ***Create your CodeCommit repository***
   
  ***H.*** Go to AWs CodeCommit service --> Create repository --> enter a repository name and click ***Create***
  
  ***I.*** Click on Clone URL --> Clone HTTPS
  
  ***J.*** Back to the command line, enter the below:
  
      git push <The link you copied> master
 
  ***K.*** Refresh your CodeCommit page and you shoud see the code files there
  
  
