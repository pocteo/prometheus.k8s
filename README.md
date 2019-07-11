# prometheus.k8s
###The Goal
The purpose of this task is to deploy Prometheus on a Kubernetes cluster and to achieve this task's goal we should master k8s concepts...
<br>
###### So what is Kubernetes in a simple way ?
It's a container orchestration technology that manages clustered environment.
<br>
###### What is a container ? 
It's an isolated environment in which you can run your application.. 
###Steps before 
Before jumping to details about k8s let us give a hit about a tool that we need to continue our scenario which is **docker**
<br>
###### So what is docker ?
It's a tool designed to manage containers by providing the compatibility ..as simple as that
 <br>
 ### Demo and steps
 Before we start with k8s you should install docker because it's required for this demo you can check here : [https://docs.docker.com/install/linux/docker-ce/ubuntu/]
###### 1- Setting up the environment :
Since we are working on a learning environment we need to set up a **Kubernetes** **cluster** on our local machine.Actually we will use **Minikube** to do this.
To install Minikube : 


```
first of all you need to install a VM such as VirtualBox <br>
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
&& chmod +x minikube 
sudo install minikube /usr/local/bin
minikube start
```

We also need Kubernetes command-line tool which is kubectl that allows you run commands against Kubernetes clusters. -To install it :
```
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectlcurl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
&& chmod +x minikube
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
kubectl version
```
###### 2- Create the deployment yaml file of Prometheus

 We should know that Kubernetes uses yaml files as inputs to create it's object that's why we need a yaml deployment file and it's structure should look like this : <br>
 ```
 apiVersion: apps/v1
 kind: Deployment
 metadata:
   labels:
     app: prometheus
   name: prometheus
 spec:
   replicas: 1
   selector:
     matchLabels:
       app: prometheus
   template:
     metadata:
       labels:
         app: prometheus
     spec:
       containers:
       - image: prom/prometheus:v2.11.0
         name: prometheus
 ```
 After preparing the deployment yaml file you sould tape those commands :
 ```
 kubectl apply -f prometheus-dep.yaml
  ```
  See if deployment has bees created :
  ```
    kubctl get deployment
   ```
   You can also see that the pod ans replicaSet has been created too :
   ```
       kubctl get pods
       kubectl get replicaset
   ```
   All good know.
   ###### 3- Create the Service yaml file 
   As we mentioned before the service object allows us to access to the container's port via node port and see the pod that is running from web browser for example ..to achieve that we need to configure service yaml file as this one : 
   
   ```
    apiVersion: v1
    kind: Service
    metadata:
      name: prometheus-service
    spec:
      type: NodePort
      ports: 
        - targetPort: 9090
          port: 9090
          nodePort: 30008	
      selector:
         app: prometheus	
   ```
   After preparing the Service yaml file you sould tape this command :
   ```
       kubectl apply -f prometheus-service.yaml
   ```
   you can check by :
   ```
    kubectl get services
   ```
   Now it's time to check from the browser before that you should get the minikube ip adress by taping :
   ```
    minikube ip 
   ```
   go to browser and search : $(minikube ip):30008 <br> 

