# docker-k8s-kotlin-app
This was a learning experiment to create a simple Spring Boot 2 Kotlin app and use docker and k8s to deploy it.

## What you need
You will need Docker, minikube and kubectl installed.

## How to use
Run `mvn install` to build the project.
Start your minikube using `minikube start`. Because we use Minikube instead of pushing the Docker image to a registry, we can build the image using the same Docker host as the Minikube VM, so that the images are automatically present. To do this we need to make sure we are using the Minikube Docker daemon. Do this by running the following magic:
```
eval $(minikube docker-env)
```
Then build the image with a tag hello-node in the current folder location:
```
docker build -t hello-node:1.0 .
```

To use the yaml files run:
```
kubectl apply -f deployment-hello-node.yaml
kubectl apply -f service-hello-node.yaml
```

To see what is currently running use: `kubectl get all`

And then to open the browser use: `minikube service hello-node`

## Cleaning up
```
kubectl delete service hello-node
kubectl delete deployment hello-node
```

## New versions and rollback
Build a new version with docker. Then deploy this new version by using the following command:
```
kubectl set image deployment/hello-node hello-node=hello-node:2.0
```

For rollback we use:
```
kubectl rollout undo deployments/hello-node
```
