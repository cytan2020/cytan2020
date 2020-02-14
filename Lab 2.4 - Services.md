
# Lab 2.4 - Services  

In this lab, you will need to:
* Create ClusterIP services imperatively and declaratively
* View and inspect the services that are currently in the namespace 
* Adding pods to a particular service
* Delete service
  
## Prerequisites  

Complete Lab 2.1, Lab 2.2 & Lab 2.3 before attempting this exercise. You will need to understand the concept of pods, replicaset, deployments, services and yaml. 

## Create ClusterIP Services Imperatively
 
There are 2 ways of creating clusterip services. The first way is to create it via the yaml file imperatively, while the second is to create it via the command line declaratively.   

1. Copy the following into an empty yaml file called ```my-service.yaml```: 

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: myApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```
      
Run ```kubectl create -f my-service.yaml```. You should see the message: ```service/my-service created```. 

2. Run ```kubectl get svc``` to check if the service is created successfully. You should see the following: 

<img width="717" alt="Screenshot 2020-02-14 at 3 53 13 PM" src="https://user-images.githubusercontent.com/60460833/74513449-8cb17800-4f45-11ea-9b98-31d204ed77f8.png">

By default, Kubernetes will create a service for its own internal usage. We can ignore if for this lab.
 
3. Run ```kubectl get ep```. The endpoint indicates the resources that are currently bind to the service. The endpoint for our service should be <none> because we have not assgined any resource to the service yet.  
 
 <img width="378" alt="Screenshot 2020-02-14 at 3 53 37 PM" src="https://user-images.githubusercontent.com/60460833/74513274-2a587780-4f45-11ea-982d-f54d3b860de9.png">
 
3. Copy the following into an empty yaml file called ```pod-1.yaml```: 

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
  labels:
    app: myApp
spec:
  containers:
  - name: nginx
    image: nginx 
    ports: 
     - containerPort: 80 
       protocol: TCP
```
    
Run ```kubectl create -f pod-1.yaml``` to create the pod.

4. Run ```kubectl get pod -o wide```. This gives extra information about the pod, and in this case we want to take a look at the IP address. 

5. Run ```kubectl get ep```. You should see that the endpoint of the pod now reflects the IP of the pod. 

6. Run ```curl (IP of your pod):80```. You should see the following: 
 
<img width="712" alt="Screenshot 2020-02-14 at 4 14 06 PM" src="https://user-images.githubusercontent.com/60460833/74513340-5247db00-4f45-11ea-8f85-2f89f8d00b03.png">

 
This command displays the content of the pod. 

7. Run kubectl get svc . Run ```curl (IP of your service):80```. If the service is linked correctly, you should see the same content as step 7. The pods and service are linked together via the concept of labels and selectors. The concept of labels and selectors is very important in Kubernetes.

## Create ClusterIP Services Declaratively

1. Run ```kubectl run pod-service2 --image=nginx --restart=Never --port=80 --expose --name=my-service-2```. This command creates both a pod and a service.  

2. Run ```kubectl get svc``` and ```kubectl get ep``` to verify that the pod and services have been created successfully. 

3. Run ```curl (IP address of your service)``` to verify that your pod and service is linked correctly. 



1. Copy the following into an empty yaml file: 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
   replicas: 2
  selector:
    matchLabels:
      name: nginx-pod
  template:
    metadata:
      labels:
        name: nginx-pod
    spec:
      containers:
      - name: nginx-container
        image: nginx:1.7.8
```

Save the file as deployment1.yaml and run ```kubectl create -f deployment1.yaml```. You should see the following message: ```error: error parsing dep1.yaml: error converting YAML to JSON: yaml: line 6: did not find expected key```.

2.  Note that the apiVersion for Deployment is the same as the apiVersion for ReplicaSet. Edit the yaml file and align the replicaset field with the selector field. Run ```kubectl create -f deployment1.yaml``` again. You should see the following message: ```deployment.apps/deployment-1 created```. 

If the yaml file is unable to execute, an error message will be displayed. Common errors include identation issues, wrong use of apiVersion and incorrect labels. 


## Delete Service

1. Next, let's delete the service. Run the command ```kubectl delete svc my-service```. You should see the message: ```service "my-service" deleted```. 
 

Run ```kubectl get svc```. You should no longer see the service ```my-service```. Repeat for ```pod/my-service2```. 


# Congratulations! You have completed the Kubernetes Service exercise!
