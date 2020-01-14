# Welcome to Microservices Enablement

The Microservices Enablement Training Programme comes with the Lab Guide to give you further practical experience in creating docker image, deploying the application in microservices, and also setting up the kubernete cluster with PKS.

## Prerequisites

We assume you run the lab on your Linux workstation. <br> 
Before you start the lab, you need to download the Lab Guide to your workstation.
```
cd ~/
git clone https://github.com/tkinsoon/microservices-enablement.git
cd microservices-enablement
```

## Lab 1 - Container 101

This lab walks you through creating a docker image followed by running the container on your local machine

[Click here to start your lab](./labs/01-lab1.md)


## Lab 2 - Kubernetes 101

This lab walks you through deploying the container(s) with the created image to the Kubernetes cluster

[Click here to start your lab](./labs/02-lab2.md)


## Lab 3 - PKS 101

This lab consists of **TWO** parts. Part I provides you the steps to setup your PKS on the Google Cloud Platform (GCP). Once the setup is done, you will need to create a new PKS clsuter in Part II, and deploy the container(s) to the newly created cluster (same as Lab 2)

[Click here to start your lab](./labs/03-lab3.md)
