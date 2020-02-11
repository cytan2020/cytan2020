# Lab 2.2 - ReplicaSet  

In this lab, you will need to:
* Create replicaset imperatively 
* View and inspect the replicaset that are currently in the namespace 
* Scale the replicaset up and down 
* Delete the replicaset once it is no longer in use
 
 
## Prerequisites  

Complete Lab 2.1 before attempting this exercise. You will need to understand the concept of pods, replicaset and yaml. 

## Create ReplicaSet 


1. Copy the following into an empty yaml file: 

```
apiVersion: v1
kind: ReplicaSet
metadata:
  name: replicaset-1
spec:
  replicas: 2
  selector:
    matchLabels:
      env: dev
  template:
    metadata:
      labels:
        env: dev
    spec:
      containers:
      - name: nginx
        image: nginx 
```

Save the file as replicaset1.yaml and run ```kubectl create -f replicaset1.yaml```. You should see the following message: ```error: unable to recognize "replicaset-definition-1.yaml": no matches for kind "ReplicaSet" in version "v1"```.

2.  Note that the apiVersion for ReplicaSet is not the same as the apiVersion for Pods. Edit the yaml file and change the version to ```apps/v1```. Run ```kubectl create -f replicaset1.yaml``` again. You should see the following message: ```replicaset.apps/replicaset-1 created```. 

If the yaml file is unable to execute, an error message will be displayed. Common errors include identation issues, wrong use of apiVersion and incorrect labels. 


## View ReplicaSet
1. To view the replicaset that you have created in the current namespace, run the command ```kubectl get replicaset```. You should see the following: 
 
<img width="447" alt="Screenshot 2020-02-11 at 3 48 39 PM" src="https://user-images.githubusercontent.com/60460833/74218848-09461b80-4ce6-11ea-857e-0f19f5802f9d.png">
   

This command gives a high level overview of the replicasets that are currently running in the namespace. It provides the name of the replicaset, the number of ‘desired’ number of pods, the 'current' number of pods, the 'ready' of pods, as well as the age of the replicaset. We set the number of replicas to 2, so our replicaset will create 2 pods. 

2. To get more information on a particular replicaset, run the command ```kubectl describe replicaset```. You should see the following: 

<img width="880" alt="Screenshot 2020-02-11 at 4 24 47 PM" src="https://user-images.githubusercontent.com/60460833/74220627-0a2d7c00-4ceb-11ea-8fda-7675a92e4e64.png">

From the event section at the bottom, you will be able to see that 2 pods have been created successfully. 

3. Run ```kubectl get pods```. You should see 2 pods. Delete 1 of the pods and run ```kubectl get pods``` again. You should see that the replicaset will automatically maintain 2 pods even if you delete 1 of them.
 
## Scale Pods 

There are 2 ways of scaling pods. The first way is to edit the yaml file, while the second is to edit it via the command line. 

1. Run ```kubectl get replicaset```. There are currently 2 pods. Run ```kubectl scale rs replicaset-1 --replicas=5```. You should see the following message: ```replicaset.apps/replicaset-1 scaled ```. Run ```kubectl get replicaset``` again. The number of pods should be 5 now. 

2. Run ```kubectl edit rs replicaset-1```. Edit the number of replicas to 3. Save and exit. You should see the message: ```replicaset.apps/replicaset-1 edited```. Run ```kubectl get replicaset``` again. The number of pods should be 3 now. 
 
## Delete ReplicaSet

1. Next, let's delete the replicaset. Run the command ```kubectl delete rs replicaset-1```. You should see the message: ```replicaset.apps "replicaset-1" deleted```.
 

Run ```kubectl get rs```. You should not see any replicaset. 


# Congratulations! You have completed the Kubernetes ReplicaSet exercise!
