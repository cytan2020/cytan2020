
# Lab 2.4 - Services  

In this lab, you will need to:
* Create services imperatively and declaratively
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

 
3. Run ```kubectl get ep```. The endpoint indicates the resources that are currently bind to the service. The endpoint should be <none> because we have not assgined any resource to the service yet.  
 
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

 
 kubectl run nginx --image=nginx --restart=Never --port=80 --expose 
 
 kubectl get svc nginx # services
kubectl get ep # endpoints 

wget -O- IP:80 



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


## View Deployment
1. To view the deployment that you have created in the current namespace, you can run the command ```kubectl get deployment```. For now let's run ```kubectl get all``` to see all the resources created by the deployment. You should see the following: 
 
<img width="716" alt="Screenshot 2020-02-12 at 3 38 40 PM" src="https://user-images.githubusercontent.com/60460833/74313605-f9463e80-4dae-11ea-9aec-1d90d2355b55.png">

   

The "get all" command gives an overview of all the resources that are currently running in the namespace. Since deployment creates replicasets and the replicaset creates pods, we are able to see those resources in the namespace. 

2. To get more information on a particular deployment, run the command ```kubectl describe deployment```. You should see the following: 

<img width="1101" alt="Screenshot 2020-02-12 at 6 01 45 PM" src="https://user-images.githubusercontent.com/60460833/74324279-c0fc2b80-4dc1-11ea-9049-199a35be65da.png">

 
Take note of the version of nginx under the pod template section. The current image version is nginx:1.7.8. From the event section at the bottom, you will be able to see that a replicaset with 2 pods has been created successfully. 
 
## Upgrade Deployment 

There are 2 ways of upgrading deployments. The first way is to edit the yaml file, while the second is to edit it via the command line. 

1. Run ```kubectl set image deployment deployment-1 nginx-container=nginx:1.7.9```. You should see the following message: ```deployment.apps/deployment-1 image updated ```.  

2. Run ```kubectl rollout status deployment deployment-1```. You should see the following message: ```deployment "deployment-1" successfully rolled out```. The rollout command is used for checking the status of the rollout. 

3. Run ```kubectl describe deployment```. You should see the nginx version as nginx:1.7.9. 

![image](https://user-images.githubusercontent.com/60460833/74321604-82647200-4dbd-11ea-9842-417ff279304a.png)
 
4. Run ```kubectl edit deployment deployment-1```. Go to nginx-container and change the version to ```nginx:1.7.8```. 

5. Run ```kubectl describe deployment``` and verify that the version is changed back to nginx:1.7.8.

## Rollback Deployment

1. Run ```kubectl rollout undo deployment deployment-1```. You should see the message: ```deployment.apps/deployment-1 rolled back```. 

2. Run ```kubectl describe deployment``` and verify that the version is changed back to nginx:1.7.9. 
 
3. Run```kubectl set image deployment deployment-1 nginx-container=nginx:999```. Next, run ```kubectl rollout status deploy deployment-1```. You should see the message ```Waiting for deployment "deployment-1" rollout to finish: 1 out of 2 new replicas have been updated...```. Ctrl+C to terminate the command. 

4. Run ```kubectl get pods```. You should see the following: 

<img width="789" alt="Screenshot 2020-02-12 at 5 46 44 PM" src="https://user-images.githubusercontent.com/60460833/74323025-cc4e5780-4dbf-11ea-92d4-c8989fbc39dc.png">

The reason that only 1 pod encountered an error is due to the fact that the pods are upgraded 1 at a time. This is the benefit of using rolling update. If there is a problem with your update, the error will not bring your system down. The remaining pods are still able to service your existing users.
 
5. Run ```kubectl describe deployment``` and verify that the version is nginx:999. Run ```kubectl rollout history deployment deployment-1``` . You should see a list of rollout history that you have done. Run ```kubectl rollout history deploy deployment-1 --revision=2```. You should be able to see a snippet of the pod template. 

6. Run ``` kubectl rollout undo deployment deployment-1 --to-revision=2 ```. You should see the message: ```deployment.apps/deployment-1 rolled back```. Run ``` kubectl rollout status deploy deployment-1``` to verify that the deployment has been successfully rolled out.

## Delete Deployment

1. Next, let's delete the deployment. Run the command ```kubectl delete deployment deployment-1```. You should see the message: ```deployment.apps/deployment-1 deleted```. 
 

Run ```kubectl get all```. You should no longer see the pods and replicaset that are associated with the deployment. 


# Congratulations! You have completed the Kubernetes Deployment exercise!
