# Implementation details

To solve the problem I've deployed a node.js webserver which sits behind the ingress (proxy).
The webserver is only accessible inside the cluster, and the is reached through the ingress which can be accessed from outside the cluster.

## Image

To build and push the docker image I have ran the following

```
cd images/app
docker build -t hlanganani/practical:latest .
docker push hlanganani/practical:latest
```


## Deployment

To deploy the application, and the service  you can run the following (in the default namespace)

```
cd ../manifests
kubectl apply -f app-deployment.yaml
kubectl apply -f app-service.yaml 
```


## Ingress

Before we deploy an ingress, we deploy an ingress controller, (this accounts for the proxy pod as required)
Since i'm running a microk8s cluster, this is done by enablig the ingress add-on as below (note the pod is deployed in the ingress namespace).

```
microk8s.enable ingress

```
Now, that we have both our pods we can deploy an ingress as follows.

```
kubectl apply -f app-ingress.yaml
```

## Connect

Now we can connect to the application, by running

```
curl -kL http://127.0.0.1/practical
```


## Monitoring