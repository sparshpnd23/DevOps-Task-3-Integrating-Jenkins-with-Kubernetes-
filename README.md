# DevOps-Task-3-Integrating-Jenkins-with-Kubernetes

**About the project-** The key points of the project are : 
1- Creation of a container image that has Jenkins installed using Dockerfile. 
2- On launching the image, Jenkins should auto start. 
3- Creation of the following tasks in that Jenkins :-

Production : This task will auto download the code from #github whenever any new code is pushed.

Deployment : A suitable Kubernetes pod     tomatically & the code will be auto deployed. Ex- If the code is in php, the program will auto detect and launch a php container and deploy the code.

Testing : The Deployed code will be automatically tested and if the page isn't working, an email will be automatically sent to the devdeloper.

Monitoring : Another Jenkins task will keep on monitoring the docker container in which code is deployed and whenever the container goes down, the task will automatically deploy another container. The data won't be lost due to persistent storage.
