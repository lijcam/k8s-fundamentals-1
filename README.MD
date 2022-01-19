# Kubernetes Fundamentals 

Following on from our previous lab, we will redeploy the pac man using Kubernetes objects. Kubernetes objects permit our application configuration to be described as code, expressed in `yaml` notation. 

In this repository, you will find Kubernetes objects that describe the Pacman application, in this lab will extend this application to include a Persistent Volume Claim for the database.

## Preparation
If you haven’t already, clone this code repository.

    git clone https://github.com/lijcam/k8s-fundamentals-1.git \
        && cd k8s-fundamentals-1
 
Followed by connecting to your OpenShift cluster as the developer and create a new project.

    oc login -u developer
    oc new-project lab2

## Creating the Persistent Volume Claim
The Persistent Volume Claim object already exists. Create the claim and OpenShift will allocate a Persistent Volume to our project.

    oc apply -f mongo-pvc.yml

We can view the status of the claim in the console in the Administrator view: Storage > PersistentVolumeClaims 

## Modify the database deployment
Modify our Pod Template for the database to include the volume, and a volumeMount for the container. [An example can be found in offical documentation](https://docs.openshift.com/container-platform/4.9/storage/understanding-persistent-storage.html#pvc-claims-as-volumes_understanding-persistent-storage) but make sure you set the following parameters correctly.

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
