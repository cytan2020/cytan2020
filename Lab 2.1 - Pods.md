# Lab 2.1 - Pods  

In this lab, you will need to:
* Create pods declaratively and imperatively
* View and inspect the pods that are currently in the namespace
* Delete the pods once it is no longer in use
 
 
## Prerequisites  

1. You need to understand the basic concept of pods and YAML. Refer to the slides for more information.   
2. We will be using vi commands regularly when editing the YAML file. Refer to https://coderwall.com/p/adv71w/basic-vim-commands-for-getting-started if you need more information on basic vi commands. 
3. Before we begin, we need to check if kubectl is installed properly. Kubectl is a command line for controlling Kubernetes clusters. Run ‘kubectl version’ to check if kubectl is installed properly. You should see the version of the installed kubectl.

## Create Pods 

There are 2 ways to create pods in Kubernetes declaratively via a yaml manifest file, or imperatively via kubectl commands.


1.  We will walk through both ways. Let’s start with the declarative approach using YAML first. To create a pod via YAML, we need to create a YAML file first. Run the command ```vi create-pod.yaml``` . Enter ```:wq!``` to save and exit the file. A new file ```create-pod.yaml ```  will be created.


2. The YAML file follows a specific structure. The indentation is important and the commands will not run if the indentation is wrong. Refer to the slides or the official documentation for more information on the YAML structure. Copy the following into the YAML file:
```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod1
spec:
  containers:
  - name: myapp-container
    image: redis   
```

3. If you are in ```-- INSERT --```
 mode after copying the text, hit your escape key and enter ```:wq!``` into the command line. This command saves and exits the file. Hit enter to execute the command. 

4. To check if the YAML file has been saved, run the command ```cat create-pod.yaml``` to view the YAML file. You should see the following:  

<img width="266" alt="Screenshot 2020-02-11 at 2 56 48 PM" src="https://user-images.githubusercontent.com/60460833/74216419-596daf80-4cdf-11ea-8921-71e1e93c1bd2.png"> 
 



5. Run ```kubectl create -f create-pod.yaml``` to create a pod. You should see the message:```pod/myapp-pod1 created```. 
 
   The ```kubectl create -f <name of yaml>``` command executes and creates a pod object based on the specifications in the file. This is applicable for the creation of other Kubernetes objects as well.    

 
6. Next, we will create a pod via imperative command. Run the command ```kubectl run myapp-pod2 --image=redis --restart=Never```. You should see the message: ```pod/myapp-pod2 created```.

   The imperative command is a quick way to generate a pod, but more often than not it is better to have a definition file stored somewhere for accountability purposes. 
 

## View Pods
1. To view all the pods that you have created in the current namespace, run the command ```kubectl get pod```. You should see the following: 
 
 <img width="470" alt="Screenshot 2020-02-11 at 3 06 59 PM" src="https://user-images.githubusercontent.com/60460833/74216703-3394da80-4ce0-11ea-838c-b36c24ed2c09.png">
   

   This command gives a high level overview of the pods that are currently running in the namespace. It provides the name of the pod, the number of ‘ready’ containers in the pod, the status of the pod, the number of time it restarted, as well as the age of the pod

2. To get more information on a particular pod, run the command ```kubectl describe pod myapp-pod1```. You should see the following: 

<img width="1065" alt="Screenshot 2020-02-11 at 3 10 26 PM" src="https://user-images.githubusercontent.com/60460833/74216857-a69e5100-4ce0-11ea-828b-a23b36afdcdb.png">

   The ```kubectl describe pod <name of pod>``` gives in-depth information about the pod. We will not be diving into the details of the pod in this course, but if you need to find out more information about the pod you can find it here. 
 
 
## Delete Pods

1. Next, let's delete the pods. Run the command ```kubectl delete pod myapp-pod1```. You should see the message: pod "myapp-```pod1" deleted```.
 

Run ```kubectl get pod```. You should only see ```myapp-pod2``` left. 



2. Repeat the same step and delete ```myapp-pod2```. 


# Congratulations! You have completed the Kubernetes Pods exercise!
