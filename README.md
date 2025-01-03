# SampleMERNwithMicroservices

Step 1: Set Up the AWS Environment

1. Set Up AWS CLI and Boto3:
   - Install AWS CLI and configure it with AWS credentials.
    
   ```
   aws configure
   ```
    - Install Boto3 for Python and configure it.

   ```
   pip install boto3
   ```
   
Step 2: Prepare the MERN Application

1. Containerize the MERN Application:

   - Ensure the MERN application is containerized using Docker. Create a Dockerfile for each component (frontend and backend).

# Backend HelloService
Checkout to the code and create .env file then update PORT value there.
```
PORT= 4200
```
Created the Docker file which expose to th eport 4200, build and run docker container in yout local, you will get the output as mentioned below.
![alt text](backend_helloservice_localsetup.PNG)

# Backend Profile service 
Checkout to the code and create .env file then update PORT value and mongo url.
```
PORT=4201
MONGO_URL= 'your mongodb url ' 
```
Created the Docker file which expose to th eport 4201, build and run docker container in yout local, you will get the output as mentioned below.

Add some data in your database.
![alt text](backend_profileservice_localsetup.PNG)

# Frontend service 

Checkout to frontend application, go tto Home.js file and update the backend hello and profile service url
```
cd src/components/Home.js

# update both hello and profile service with respective port numbers
 # Hello service 
http://localhost:4200/     
 # Profile service 
http://localhost:4201/fetchUser             
```
Created the Docker file which expose to th eport 3000, build and run docker container in yout local, you will get the output as mentioned below.
![alt text](frontend_localsetup.PNG)

# Docker compose
Created docker compose file to trst all the dockerfile is up and running fine with perfect sink.
![alt text](<docker compose file.PNG>)

2. Push Docker Images to Amazon ECR:

   - Build Docker images for the frontend and backend.

   - Create an Amazon ECR repository for each image.

   - Push the Docker images to their respective ECR repositories.

Build and pushed the image to docker hub using the command mentioned below, repeated the same step for all the 3 images.
```
docker build -t frontend_service:v1.0 .
docker tag frontend_service:v1.0 flowerking21/micro_frontendservice:latest
docker push flowerking21/micro_frontendservice:latest
```
Login to AWS console, create a ECR repository and push the image to the repository.
Repeat the steps for all 3 services 
```
#Create a ECR repository 
aws ecr create-repository --repository-name micro_frontendservice --region ap-south-1
# Login to AWS
aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 515210271098.dkr.ecr.ap-south-1.amazonaws.com
# tag your image with the ECR repository image
docker tag flowerking21/micro_frontendservice:latest 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_frontendservice:latest
# push to ECR repository
docker push 515210271098.dkr.ecr.ap-south-1.amazonaws.com/micro_frontendservice:latest
```
Step 3: Version Control

1. Use AWS CodeCommit:

   - Create a CodeCommit repository.

   - Push the MERN application source code to the CodeCommit repository.

* Go To AWS code commit and create a repository, make a note you can't ssh into your account by root user so go to IAM user and create a new user with  AWSCodeCommitPowerUse access.
* After creating of the user go to HTTPS Git credentials for AWS CodeCommit then generate a new credencials and store the credencials.
![alt text](image.png) 
* Then go to AWS code commit AWS repository click on the HTTPS to copy the repository link to clone 
![alt text](image-1.png)
* Open git bash in your machine and then clone the repsoitory there, then GIT credencials manager pop up update IAM user username and password then it start clone your repository.
* Then added files which you need to upload in code commit then the process is normal one which we follows while upload in git.
```
git status
git add .
git commit -m "commit message"
git push 
```
* one done do to your AWS reposiotry and verify the changes.
![alt text](image-2.png)

Step 4: Continuous Integration with Jenkins

1. Set Up Jenkins:

   - Install Jenkins on an EC2 instance.

   - Configure Jenkins with necessary plugins.

* Launch a EC2 instance with jenkins, docker, aescli, git, kubectll etc.,
* After launching the instance Jenkins server is available in port 8080
```
http://<public_ip>:8080
```
2. Create Jenkins Jobs:

   - Create Jenkins jobs for building and pushing Docker images to ECR.

   - Trigger the Jenkins jobs whenever there's a new commit in the CodeCommit repository.

* Jenkinsfile is updated above.
