# Hello World App with Go

## Deployment Option

* helm
* kubectl

## Deployment Features

* `multi-stage` builds in dockerfile
* using `alpine` docker image for security and lightweight.
* custom `fast` storageclass which will allow you to increase the disk size any time with help of storageclass varibale : `allowVolumeExpansion: true`
* The hello-world application use `initcontainer` to donwload the data to `/data` folder
* Using `podAffinity` for increse the performance (read and write to mongodb) 
* Using `podAntiAffinity` for High availability incase if we have cluster
* `readinessProbe` wait till the service up before accepting the request
* `livenessProbe` ensure the service availability
* using `persistentVolumeClaim` for MongoDB and Hello wold Application


**Note The `storageclass` Configuration is not mandatory to run this services, but I conifgured to showcase how we can achieve high throughput / io operation and capability to increase the disk size anytime even in production system**


## Local Deployment (docker-compose deployment)

for Local testing purpose, `docker-compose and docker` can be used.

```
cd Docker-Deployment/
docker-compose up -d
```


## Kubernetes Deployment

* Create Namespaces
```
kubectl apply -f namespace.yaml
```

*  Configure `fast` storageclass with `allowVolumeExpansion: true` (for an example I choose AWS and Azure as Cloud Providers)

**AWS Cloud**
```
kubectl apply -f storageclass/AWS-storageclass.yaml
```

**Azure Cloud**
```
kubectl apply -f storageclass/Azure-storageclass.yaml
```

* Create Persistent Volume Claim and Deploy `MongoDB` on namespace `mongo`
```
kubectl apply -f mongo-pvc.yaml
kubectl apply -f mongo-deployment.yaml
kubectl apply -f mongo-service.yaml
```

* Create Persistent Volume Claim and Deploy `Hello-World` App on namespace `hello-world`
```
kubectl apply -f hello-world-pvc.yaml
kubectl apply -f hello-world-deployment.yaml
kubectl apply -f hello-world-service.yaml
```



