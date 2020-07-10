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

**TASK 1 : Production** 
In this task, the code will be auto downloaded from Github. For this, I have used trigger method.

![](/images/prod1.png)

![](/images/prod2.png)

The trigger would work and I have provided the link along with the authentication token to my local git. As soon as any code is pushed, Git hook would work & run this task automatically. In the execute shell section, The webpages are being downloaded to this folder inside the container.

![](/images/prod3.png)


**TASK 2 : Transfer**
In this task, the web pages downloaded inside the container are being transferred to the base Redhat using scp. I have already authorized a ssh key from my container to Redhat.


 ![](/images/transfer.png)
 
 
 
 
**TASK 3 : Deployment**
In this task, the file type is checked by checking the extension. Like if the pages are build in html, the extension of the file would be .html, then the code would detect and launch an httpd server to deploy the pages. If the pages are build in php, the extension of the file would be .php, then a php supporting container would be deployed. I have checked just 2 types - html & php but you can add more depending upon your requirements. The suitable pod will auto launch and the code will be auto deployed.

For running the kubectl through your redhat, make sure that your minikube is running, all the necessary keys & certificates are transferred in Redhat & the config file is successfully built. A demo config file to setup kubectl is as follows :


    apiVersion: v1
    kind: Config

    clusters:
    - cluster:
        server: https://192.168.99.101:8443
        certificate-authority: /root/ca.crt
      name: apun_ka_cluster

    contexts:
    - context:
        cluster: apun_ka_cluster
        user: sparsh

    users:
    - name: sparsh
      user:
        client-key: /root/client.key
        client-certificate: /root/client.crt
        
After creating this file, save it in the .kube folder. Now, we can access the kubernetes via redhat.


![](/images/deploy.png)


**TASK 4 : Testing**
In this task, the deployed code would be tested and if there is any error, an email will be automatically sent to the mentioned emails. For this, I have used the status method, i.e. , whenever we access any web page using curl and linux, the status is 200 if the page is working, otherwise not. So, I have segregated the status and used it in my concept. The exit 1 would deliberately fail the task.


![](/images/test1.png)

![](/images/test2.png)

![](/images/test3.png)

If your email isn't working, you need to go to Jenkins configuration and do the following setup. 

![](/images/test4.png)

If you are still facing any errors in email, go to your jenkins container & run ---

![](/images/test5.png)

In that file, in the _**JENKINS_JAVA_OPTIONS**_, make the following changes :

![](/images/test6.png)

After that, run sudo service jenkins restart cmd in your jenkins container.

If you still face error in sending email, & the popup says authentication error, you need to go to your google account & disable the two step authentication & turn on the less secure app access option below it. After this, the email would be working absolutely fine.


**Conclusion -** Since, we have launched the pods using Kubernetes Deployment, we need not worry about the monitoring. If the pods will crash, the replica set working behind the deployment will relaunch the pods.

You can open the IP in your browser & see the website.

That's all for now !!
Any suggestions are highly appreciated.


