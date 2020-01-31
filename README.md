# Lab 2.1 - Pods  

In this lab, you will need to:
* Create pods declaratively and imperatively
* View and inspect the pods that are currently in the namespace
* Delete the pods once it is no longer in use
 
 
# Prerequisites  

You need to understand the basic concept of pods and YAML. Refer to the slides for more information
Before we begin, we need to check if kubectl is installed properly. Kubectl is a command line for controlling Kubernetes clusters. Run ‘kubectl version’ to check if kubectl is installed properly. You should see the version of the installed kubectl.

# Create Pods 

There are 2 ways to create pods in Kubernetes declaratively via a yaml manifest file, or imperatively via kubectl commands.


1.  We will walk through both ways. Let’s start with the declarative approach using YAML first. To create a pod via YAML, we need to create a YAML file first. Run the command ‘vi create-pod.yaml’. A new file ‘create-pod.yaml’ will be created.



2. The YAML file follows a specific structure. The indentation is important and the commands will not run if the indentation is wrong. Refer to the slides or the official documentation for more information on the YAML structure. Copy the following into the YAML file
` 
<apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod1
spec:
  containers:
  - name: myapp-container
    image: redis  `
