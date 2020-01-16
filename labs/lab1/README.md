# Lab 1 - Docker 101

In this lab, you will need to:
- Create a Dockerfile to copy the dependencies into the base image
- Build a new Docker Image with the Dockerfile
- Run the Docker container on your local workstation

### Prerequisites

You need to setup your jumpbox on GCP, if not please go to [Lab 0 - Prerequisites](./labs/lab0)

### 1. Install Docker Runtime
Setup the Docker repository:
```
sudo apt-get update
```
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

Install Docker Engine
```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

Verify that Docker Engine is installed correctly by running the **hello-world** image
```
sudo docker run hello-world
```
This command downloads a test image and runs it in a container. When the container runs, it prints an informational message and exits.

### 2. Verify the Dockerfile
```
cd ~/microservices-enablement/labs/lab1
```
You should see the **two** files under the lab1 folder: **Dockerfile** and **container101.war**. The **Dockerfile** is a text document in YAML that contains all the commands a user could call on the command line to assemble an image. The **container101.war** is WAR file with JSP and HTML pages as the dependency files to build our Docker image.
```
cat Dockerfile
```
The comments in the Dockerfile give you a better idea on what are the steps to be executed when creating a new Docker image.

In short, the new docker image relies on the base image with Tomcat pre-installed, which is available from Docker Hub as the public docker image repository, together with the WAR file with JSP and HTML pages to run as the web appplication on the Tomcat application server. The last command will only be executed during the runtime when the Docker container is created from the Docker image 

### 3. Build and run your first Docker Image
Create a new docker image namely as **docker101**
```
sudo docker build -t docker101 .
```
Once the docker image is created, it will be stored in the local repository of your jumphost, run the following command to list out the images you have in your local repository.
```
sudo docker image ls

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
docker101           latest              02300c8b4967        10 seconds ago      507MB
tomcat              latest              ed94f55483b8        11 days ago         507MB
hello-world         latest              fce289e99eb9        12 months ago       1.84kB
```
As you can see from the list, the **docker101** is created based on **tomcat** as the base image which is also downloaded from the docker hub. The **hello-world** is the image which you downloaded in the previous step to verify if the Docker Engine is properly installed.

Now, you are ready to run your Docker container
```
sudo docker run -it --rm -p 80:8080 docker101
```
Check your jumphost's External IP address via the [VM instances](https://console.cloud.google.com/compute/instances) under the GCP project given by your instructor. Copy the External IP address and paste it as the URL to your web browser.

You should see the HTML page as below:
![docker101](/images/docker101.png)

The hostname shows the External IP address of your jumphost.

Congratulations! You have completed this lab! In the next labs, you will notice the varies of the hostname from the HTML page everytime you refresh your web browser due to the multiple containers from the same Docker image running in the Kubernetes cluster, you will find out more in the next labs!

You may now press ```Ctrl + C``` command to stop running the container from your jumphost.
