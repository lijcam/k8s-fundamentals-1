# Kubernetes API Objects

Following on from our previous lab, we will redeploy the Pacman application using Kubernetes objects. Kubernetes objects permit our application configuration to be described as code. This means we can deploy our application to new clusters simply and also sets the conditions to develop patterns and templates for your team or organisation.

## 1. Preparation

If you haven’t already, set up your virtual console and clone this code repository.

    cd /root

    git clone https://github.com/lijcam/k8s-fundamentals-1.git
    cd k8s-fundamentals-1/Lab\ 2
 
Followed by connecting to your OpenShift cluster as the developer and create a new project.

    oc login -u developer
    oc new-project lab2

## 2. Deploy the database

Create the mongo Deployment, and Service.

    oc apply -f mongo-deployment.yml
    oc apply -f mongo-service.yml

## 3. Deploy the Pacman application

Use the existing Pacman Kubernetes objects to deploy the Pacman app.

    oc apply -f pacman-deployment.yml
    oc apply -f pacman-service.yml
    oc apply -f pacman-route.yml

Once the Pacman application is up and running, we can find its route by running:

    oc get routes

