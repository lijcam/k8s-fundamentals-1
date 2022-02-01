# Kubernetes Fundamentals 

The challenge lab, builds on what we learned from the Kubernetes API objects lab. The goal of this exercise is to mount storage into our mongo pod to persist data across pod redeployments. 

## 1. Preparation

If you haven’t already, set up your virtual console and clone this code repository.

    mkdir projects
    cd projects

    git clone https://github.com/lijcam/k8s-fundamentals-1.git
    cd k8s-fundamentals-1/Lab\ 3
 
Followed by connecting to your OpenShift cluster as the developer and create a new project.

    oc login -u developer
    oc new-project lab3

## 2. Creating the persistent volume claim

Persistent volumes have been pre-created in the playground environment. Use the `oc` command to allocate a Persistent Volume to our project.

    oc apply -f mongo-pvc.yml

We can view the status of the claim in the console in the Administrator view: Storage > PersistentVolumeClaims 

## 3. Modify the database deployment

Modify our Pod Template for the database to include the volume, and a volumeMount for the container. [An example can be found in offical documentation](https://docs.openshift.com/container-platform/4.9/storage/understanding-persistent-storage.html#pvc-claims-as-volumes_understanding-persistent-storage) but make sure you set the following parameters correctly.

The file we need to modify is `Lab 3/mongo-deployment.yml`

1. In the container, volumeMounts section, the `mountPath` must be `/data/db/`
2. In the volume section, `claimName` must match the name of the Persistent Volume Claim.

## Deploy the database

Create the mongo Deployment, and Service.

    oc apply -f mongo-deployment.yml
    oc apply -f mongo-service.yml

## Deploy the Pacman application

Use the existing Pacman Kubernetes objects to deploy the Pacman app.

    oc apply -f pacman-deployment.yml
    oc apply -f pacman-service.yml
    oc apply -f pacman-route.yml
