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

For monitoring, we again leverage microk8s add-ons. There are two add-ons to enable, namely dashboard and prometheus. We run the below.

```
microk8s.enable dashboard
microk8s.enable prometheus
```
This will create a few deployments under the monitoring namespace. Once that has settled you can verify that grafana is up and running as follows

```
kubectl cluster-info
```
I then port-foward on the grafana service, as below.
```
kubectl port-forward services/grafana 3001:3000 -n monitoring
```
From here, I can log in to grafana on the browser with the default credentials where I can do some monitoring uby selecting sing prometheus as a data source.
