# DevOps-Task-3-Integrating-Jenkins-with-Kubernetes

**About the project-** The key points of the project are : 
1- Creation of a container image that has Jenkins installed using Dockerfile. 
2- On launching the image, Jenkins should auto start. 
3- Creation of the following tasks in that Jenkins :-

Production : This task will auto download the code from github whenever any new code is pushed.

Deployment : A suitable Kubernetes pod will be launched automatically & the code will be auto deployed. Ex- If the code is in php, the program will auto detect and launch a php pod and deploy the code. The pod will be auto exposed & the data will be persistent as a dynamic PVC will be attached to ensure that no data is lost in case of container crash.

Testing : The Deployed code will be automatically tested and if the page isn't working, an email will be automatically sent to the devdeloper. The pod will be redeployed after the developer has corrected the code.

Monitoring : Since we are using a Replica Set, hence we need not worry about pod monitoring. If the pod goes down, the Replica Set  will automatically relaunch it based on the labels.

Step - 1: I have created a container image that has Jenkins installed using Dockerfile.

The commands in Dockerfile are as follows--

I have used Centos latest version. We need to install sudo & wget because they will be furthur needed in the installation of Jenkins. Then we have installed Jenkins & the suitable jdk version. Since systemctl doesn't work in Centos, so we would require to use sudo service jenkins start command. But service command isn't avvailable in Centos latest image. So, we have installed /sbin/service We have also installed git because we'll need it later. The third last line is to give powers to jenkins to perform operations inside the container. The, we have started the Jenkins by sudo service jenkins start but we also need to start /bin/bash otherwise the container will close as soon as jenkins starts because there will be no tasks left. Hence, we also start the bash in same coomand.

Note : The bash has to be started in the same command because if there are more than one commands in the Dockerfile, then only the last command runs.

After that, we have exposed Port 8080 because Jenkins runs on port 8080.

![](/images/df.png)

Build an image from this Dockerfile using docker build -t NAME:TAG /location of Dockerfile/

Run a container from your newly built image & expose it to any available port using PAT.

Ex- docker run -it -p 2301:8080 --name jenpro jentest:v5

Step - 2: Now, we move on to our Jenkins tasks.

TASK 1 : Production In this task, the code will be auto downloaded from Github. For this, I have used trigger method.


 

