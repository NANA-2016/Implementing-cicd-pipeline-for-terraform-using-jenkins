# Implementing-cicd-pipeline-for-terraform-using-jenkins
Implementing-cicd-pipeline-for-terraform-using-jenkins

Create an AWS instance and run it on the terminal to create a jenkins server and create a port which where Jenkins will be able to run.

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/fd1c2df7-a7e1-4833-9f6e-5c4fa1350cc7)

Create a  FOLDER "Terraform-with-cicd" and in the folder create a file "docker file "

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/e670e181-087a-4240-af65-c733e272cc64)

 Docker file  content .

  # Use the official Jenkins base image
 FROM jenkins/jenkins:lts

 # Switch to the root user to install additional packages
 USER root

 # Install necessary tools and dependencies (e.g., Git, unzip, wget, software-properties-common)
 RUN apt-get update && apt-get install -y \
     git \
     unzip \
     wget \
     software-properties-common \
     && rm -rf /var/lib/apt/lists/*

 # Install Terraform
 RUN apt-get update && apt-get install -y gnupg software-properties-common wget \
     && wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | tee /usr/share/keyrings/hashicorp-archive-keyring.gpg \
     && gpg --no-default-keyring --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg --fingerprint \
     && echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list \
     && apt-get update && apt-get install -y terraform \
     && rm -rf /var/lib/apt/lists/*

 # Set the working directory
 WORKDIR /app

 # Print Terraform version to verify installation
 RUN terraform --version

 # Switch back to the Jenkins user
 USER jenkins



![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/27f68ad2-3cd7-474e-83c6-a476ba0d6b0a)

Create a docker image 

 docker build -t jenkins-server 

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/429debfd-97d9-4875-b6d4-91a6c8270868)
 
Create a jenkins image and run it into a docker container 

docker run -d -p 8080:8080 --name jenkins-server jenkins-server 

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/eec18c6c-0913-4660-bc62-a3eed59ad61b)

check if the container has been created succesffully by running the command 

sudo docker ps 

![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/47132b78-e8ba-47d0-a8f7-c489f9a065c2)

Now go to the browser run the ourJenkins server .

 You access the jenkinds file  from the terraform-with-cicd  folder with command "sudo docker exec -it jenkins bash" or 

docker exec -it  <container id or container name gotten after you run "docker ps command">  /bin/bash then run 

"jenkins@<container id>:/app$ pwd /apps" to get into apps file 

 This i s where  you will run "cat /var/jenkins_home/secrets/initialAdminPassword" to get jenkins password
 
![image](https://github.com/NANA-2016/Implementing-cicd-pipeline-for-terraform-using-jenkins/assets/141503408/1ee56f98-860f-4600-9f55-f8a357ce99e5)



 



