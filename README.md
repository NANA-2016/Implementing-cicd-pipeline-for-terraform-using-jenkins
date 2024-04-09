# Implementing-cicd-pipeline-for-terraform-using-jenkins
Implementing-cicd-pipeline-for-terraform-using-jenkins

Create an AWS instance and run it on the terminal to create a jenkins server and create a port which where Jenkins will be able to run.

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/fd1c2df7-a7e1-4833-9f6e-5c4fa1350cc7)

Create a  FOLDER "Terraform-with-cicd" and in the folder create a file "docker file "

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/e670e181-087a-4240-af65-c733e272cc64)

  ## Docker file  content .

  #Use the official Jenkins base image
 FROM jenkins/jenkins:lts

 #Switch to the root user to install additional packages
 USER root

 #Install necessary tools and dependencies (e.g., Git, unzip, wget, software-properties-common)
 RUN apt-get update && apt-get install -y \
     git \
     unzip \
     wget \
     software-properties-common \
     && rm -rf /var/lib/apt/lists/*

 #Install Terraform
 RUN apt-get update && apt-get install -y gnupg software-properties-common wget \
     && wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg \
     && gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint \
     && echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list \
     && apt-get update && apt-get install -y terraform \
     && rm -rf /var/lib/apt/lists/*

 #Set the working directory
 WORKDIR /app

 #Print Terraform version to verify installation
 RUN terraform --version

 #Switch back to the Jenkins user
 USER jenkins

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/0b4d0536-9ebe-4484-99c0-1e8178aa5b92)


![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/27f68ad2-3cd7-474e-83c6-a476ba0d6b0a)

 ## Create a docker image 

"docker build -t jenkins-server "

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/429debfd-97d9-4875-b6d4-91a6c8270868)
 
Create a jenkins image and run it into a docker container 

docker run -d -p 8080:8080 --name jenkins-server jenkins-server 

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/eec18c6c-0913-4660-bc62-a3eed59ad61b)

check if the container has been created succesffully by running the command 

sudo docker ps 

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/47132b78-e8ba-47d0-a8f7-c489f9a065c2)

## Running the Jenkins server on the browser.

Now go to the browser run the Jenkins server on the browser .

 You access the jenkinds file  from the terraform-with-cicd  folder with command "sudo docker exec -it jenkins bash" or 

docker exec -it  <container id or container name gotten after you run "docker ps command">  /bin/bash then run 

"jenkins@<container id>:/app$ pwd /apps" to get into apps file 

 This i s where  you will run "cat /var/jenkins_home/secrets/initialAdminPassword" to get jenkins password

 ![Screenshot 2024-03-23 221211](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/fac94745-cfcb-4efc-ae78-82c5ed87df33)


 ![Screenshot 2024-04-04 195631](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/a5147989-7902-4a1e-9bb7-d4999c651df8)

 ![Screenshot 2024-04-04 195524](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/c63d53bc-0984-4aed-80fb-1726610765c3)


 ![Screenshot 2024-04-04 195649](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/e6c95343-9635-4c96-8ffd-ebde83e57c89)

 
![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/1ee56f98-860f-4600-9f55-f8a357ce99e5)


## Creating a git repository.

"https://github.com/dareyio/terraform-aws-pipeline.git" fork to the repository  tof form a git repository you ll be working from.

![Screenshot 2024-03-25 231731](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/4ddf11e7-fe94-46ef-a237-17a6268ce276)


Check the container status with the the command "sudo docker ps -a" 

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/0e1bb859-6a15-4407-bac1-dbef8154b50f)

 Restart the docker container if it has exited with the command "docker restart <container id>

 ![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/03f6bbc0-8df3-4315-9188-54bf185b25b4)

 ## INSTALL JENKINS PLUGINS

  How to Navigate:
  Dashboard->Manage Jenkins->Plugins-> Available plugins->AWS credentials plugin->install
 Dashboard->Manage Jenkins->Plugins-> Available plugins->Git integration->install
 Dashboard->Manage Jenkins->Plugins-> Available plugins->Terraform plugin->install

 ![Screenshot 2024-03-23 224008](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/92178ed3-e16c-4875-970d-c550c2db0dd8)

                                                                                               
  After installing all the plugins then you need to restart the jenkins server on the browser  by clicking on the Jenkins 
  
  restart button.

 ## INSTALLING JENKINS CREDENTIALS

  Fist you need to create a classic token fronm git hub

   How to generate a token:

   Github account->settings->developers settings->token classic ->generate classic token(copy the token generated and keep it as you wont be able to acess it again)
   
   ![Screenshot 2024-03-23 230653](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/e28770f2-1193-4b1b-9838-7e0da72a2d56)


  ![Screenshot 2024-03-23 230724](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/5e8dbc0d-44a3-4dba-9d45-c04f6577d486)
 
![Screenshot 2024-03-23 231108](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/ac5b3eaf-9d8c-4d33-b623-c8460d318c70)![Screenshot 2024-03-23 231108](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/0a7ccffa-1846-4385-8131-f0d716acfb60)

![Screenshot 2024-03-23 231455](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/c41f3635-7de6-40d1-a9ac-bc9f51bc3cc9)

![Screenshot 2024-03-23 232159](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/33ef4fe4-dc9c-421f-8cab-df51aa2912d6)

  How to navigate

  Dashboard-> Manage jenkins-> credential->global-> configure the plugins as per your github account  using the generated 
  
  token and your git hub account name eg (NANA2016).

  ![Screenshot 2024-04-04 220811](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/78fbbd61-72b6-4f08-aadb-5800de02cd48)


  ![Screenshot 2024-03-23 232626](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/b327950c-cff1-4c32-9d36-80f13e47c82b)


  Create a second credential for AWS secret key and acess key created while creating an AMI account on AWS.

  ![Screenshot 2024-04-01 195729](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/83d50149-b134-4312-8334-8888a6d37c9d)


 ## set up jenkins multibranch Pipeline .

 Dashboard>New item>Enter the item name>multibranch pipeline .

 Give it a name  and description..branch source(github)>choose the credentials you want to use> copy your git url>apply 
 
 and save.

 Configure the forked repository perovider.tf file with the "S3 BUCKET" created on the AWS account.
 
 ## Jenkins file code 

 pipeline {
    agent any

    environment {
    TF_CLI_ARGS = '-no-color'
     }

     stages {
         stage('Checkout') {
             steps {
                 script {
                     checkout scm
                 }
             }
         }

         stage('Terraform Plan') {
             steps {
                 script {
                     withCredentials([aws(credentialsId: 'AWS_CRED', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                         sh 'terraform init'
                         sh 'terraform plan -out=tfplan'
                     }
                 }
             }
         }

         stage('Terraform Apply') {
             when {
                 expression { env.BRANCH_NAME == 'main' }
                 expression { currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause) != null }
             }
             steps {
                 script {
                      // Ask for manual confirmation before applying changes
                     input message: 'Do you want to apply changes?', ok: 'Yes'
                     withCredentials([aws(credentialsId: 'AWS_CRED', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {/                         
                        sh 'terraform init'
                         sh 'terraform aply -out=tfplan'
                     }
                 }
             }
         }
     }
 }

##  Provider.tf file code
 
![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/41e57862-e495-4f2c-9b76-d5c45806f128)


 After making the changes on the code on VS CODE Platform, run the build in Jenkins and see the console output where you can chose to either continue otr about the AKS installation.

 ## Creation of a second branch(faith)

 on vs code run the commands 

  git checkout  -b faith

  ![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/33a9c12a-b9e2-4135-ab9b-4ad436d4e064)


## Final script

 Make all the other necessary changes on the new branch and let it build on jenkins and see the result where you can choose to eithe accept or abort to be effected by the script on AWS console  ie EKS cluster configuration .

   ##### 1 .The changes to be made on the branch include  :

  #####  2 .printing echo messages before and after every execution
  
  ##### 3. In terraform apply stage correct sh 'terraform aply-out==tfplan to sh @terraform apply tfplan'

 #####  4. Intoduce terraform lint(which comes before the terraform apply stage  and terraform validate

  ##### 5. Clean up stage 
  
  ##### impelmenting error handling stage .

  
pipeline {
    agent any
 
    environment {
        TF_CLI_ARGS = '-no-color'
    }
 
    stages {
        stage('Checkout') {
            steps {
                script {
                    // checkout scm
                    checkout scmGit(branches: [[name: '*/faith']], extensions: [], userRemoteConfigs: [[credentialsId: 'Github', url: 'https://github.com/NANA-2016/terraform-aws-pipeline.git']])
                }
            }
        }
 
        stage('Terraform Lint') {
            steps {
                script {
                    echo 'Executing Terraform Lint...'
                    withCredentials([aws(credentialsId: 'AWS_CRED', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh 'terraform init -input=false -no-color'
                    sh 'terraform validate'
                    echo 'Terraform Lint executed successfully.'
                }
            }
          }
        }
 
        stage('Terraform Plan') {
            steps {
                script {
                    echo 'Executing Terraform Plan...'
                    withCredentials([aws(credentialsId: 'AWS_CRED', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                        sh 'terraform init -input=false -no-color'
                        sh 'terraform plan -input=false -no-color'
                    }
                    echo 'Terraform Plan executed successfully.'
                }
            }
        }
 
        stage('Terraform Apply') {
            when {
                // expression { env.BRANCH_NAME == 'feature-fix' }
                // expression { currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause) != null }
                expression { true }
            }
            steps {
                script {
                    try {
                        // Ask for manual confirmation before applying changes
                        input message: 'Do you want to apply changes?', ok: 'Yes'
                        echo 'Executing Terraform Apply...'
                        withCredentials([aws(credentialsId: 'AWS_CRED', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                            sh 'terraform init -input=false -no-color'
                            sh 'terraform apply tfplan -input=false -no-color'
                        }
                        echo 'Terraform Apply executed successfully.'
                    }
                    catch(Exception e) {
                        echo "An error occurred: ${e.message}"
                        currentBuild.result = 'FAILURE'
                    }
                  }
                }
            }
   
        stage('Error Handling') {
            when {
                // expression { currentBuild.result == 'SUCCESS' }
                expression { true }
            }
            steps {
                echo 'Error occurred during pipeline execution. Handling...'
                // Add error handling tasks here, if any
                echo 'Error handling completed.'
            }
        }
 
        stage('Cleanup') {
            steps {
                echo 'Performing cleanup...'
                cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
                echo 'Cleanup completed.'
            }
        }
    }
  }
 

 

  








 



