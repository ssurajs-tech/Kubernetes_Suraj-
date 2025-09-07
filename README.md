# Kubernetes_Suraj-

What is kubernetes :- 
-------------------
	- Kubernetes is an open-source container management tool which automate container deployment, Container scaling and Load balancing.
	- It schedules, runs and manages isolated containers which are running on virtual/physical/cloud machine.
	- All top cloud providers support Kubernetes.

History -
-------
	- Google developed an internal system called Brog (later named as Omega) to deploy & manage thousands of google application and services on their cluster.
	- In 2014, Google introduced Kubernetes an open source platform written in Golang language & later donated to CNCF.

Online Platform for k8s -
------------------------
	- Kubernetes playground
	- Play with K8s
	- Play with Kubernetes classroom

Cloud based Kubernetes services -
--------------------------------
	GKE - Google Kubernetes services
	AKS - Azure Kubernetes services
	Amazon EKS - Elastic Kubernetes services

Kubernetes Installation Tool -
-----------------------------
	- Minikube
	- Kubeadm
	- Kind

Problems with the scaling up the containers -
------------------------------------------------
	- Container can not communicate with each other
	- Autoscaling and load balancing was not possible
	- Containers had to be managed carefully.

Features of Kubernetes - 
----------------------
	1. Orchestration (Clustering of any number of containers on different network)
	2. Auto-scaling {Vertical(Scale up and down) and Horizontal (scale in and out) Scaling}
	3. Load-balancing
	4. Platform Independent (Cloud/Physical/virtual)
	5. Fault tolerance (Node/POD failure)
	6. Health Monitoring of Containers
	7. Batch execution (One time, Sequential, Parallel)
	8. Rollback (going to get back to previous version)


                        Differences between Kubernetes and docker swarm
                        -----------------------------------------------
						
	FEATURES                                    KUBERNETES                                       DOCKER SWARM
-------------------------------------------------------------------------------------------------------------------------------------------------------
	1. Installation and Cluster configuration   Complicated and time consuming                   Fast and easy
												
	2. Support                                  K8s can work with almost all containers          Work with docker only
												type like rocket, Docker, Containers.
	
	3. GUI                                      Available                                        Not Available
												
	4. Data Volume                              Only shared within containers in same pod        Can be shared with any other container
												
	5. Logging and Monitoring                   Inbuilt tool present for monitoring              Used 3rd party tool like Splunk.
----------------------------------------------------------------------------------------------------------------------------------------------------------

	- Apache Marathon - It is a container management tool. which has low market share. however, It is very important tool.
	
	
Architecture of Kubernetes -
------------------------------

Working with Kubernetes -
	- We create a manifest (.yml)
	- Apply this to cluster to bring it to desired state
	- Pod runs on node, which is controlled by master
	
Role of control plane/Master-node -
	➢ Kubernetes cluster contains containers running on bare metal/ vm instances, Cloud instances/ all mix.
	➢ Kubernetes designates one or more of these as master and all others as workers.
	➢ The Master is now going to run set of k8s processes. 
	➢ These processes will resume smooth functioning of cluster. 
	➢ These processes are called “control plane”.
	➢ Can be multi-master for high availability.
	➢ Master runs control plane to run cluster smoothly.

Components of Control Plane (Master Node):
-------------------------------------
	1. Kube-API server
	2. Etcd
	3. Kube-scheduler
	4. Controller manager

1. Kube-API server (for all communications):
	➢ This api-server interacts directly with user (i.e we apply .yml or json manifest to kube-apiserver).
	➢ This kube-apiserver is meant to scale automatically as per load.
	➢ Kube api-server is front-end of control-plane.

2. Etcd:
	➢ It stores metadata and status of cluster.
	➢ Etcd is consistent and high available store (key-value store).
	➢ Single Source of truth for cluster state (info about state of cluster).
		Etcd has following features: -
			a. Fully replicated: the entire state is available on every node in the cluster.
			b. Secure: implements automatic TLS with optional client-certificate authentication.
			c. Fast: benchmarked at 10,000 writes per second.
		
3. Kube scheduler:
	➢ When users make request for the creation and management of PODs, Kube-scheduler is going to take action on these requests.
	➢ Handles POD creation and management.
	➢ Kube scheduler match/assign any node to create and run PODs.
	➢ A scheduler watches for newly created PODs that have no node assigned.For every POD that the scheduler discovers, 
	   the scheduler becomes responsible for finding best node for that POD to run on.

4. Controller Manager:
	➢ Make sure that actual state of cluster matches the desired state.
	➢ Two possible choices for controller manager:
				a. If k8s on cloud, then it will be cloud-controller-manager.
				b. If k8s on non-cloud then it will be kube-controller-manager
	
				Components on master that runs controller:-
		a. Node controller   : for checking with the cloud providers to determine of a node has been detected in the cloud after id stops responding.
		b. Route controller  : responsible for setting up network, routes on your cloud.
		c. Service controller: responsible for load balancers on your cloud against services of type load balancers.
		d. Volume controller : for creating, attaching and mounting volumes and interacting with the cloud provider to orchestrate volume.
	
           Worker Nodes 
           -------------
Node is going to run 3 important pieces of software/process. These 3 components collectively called NODE.
    1. Kublet
    2. Container engine
    3. Kubeproxy

1. Kublet:
	➢ Agent running on the node.
	➢ Listens to Kubernetes master.(e.g- POD creation request)
	➢ Use port 10255.
	➢ Send success/fail reports to master.

2. Container Engine/Docker:
	➢ Works with kublet.
	➢ Pulling images.
	➢ Start/stop containers.
	➢ Exposing containers on ports specified in manifest.
	
3. Kube-proxy:
	➢ Assign IP to each POD.
	➢ It is required to assign IP address to PODs (dymanic).
	➢ Kube-proxy runs on each node and this make sure that each POD will get its own unique IP address.


POD:-
-----
	➢ Smallest unit in Kubernetes.
	➢ POD is a group of one or more containers that are deployed together on the same host.
	➢ A cluster is a group of nodes.
	➢ A cluster has at least one worker node and master node.
	➢ In Kubernetes the control unit is the POD, not containers. 
	➢ It consists of one or more tightly coupled containers.
	➢ POD runs on node which is control by master.
	➢ Kubernetes only knows about PODs (does not know about individual container).
	➢ Cannot start containers without a POD.
	➢ One POD usually contains one container.

Multi container PODs:
	➢ Share access to memory space.
	➢ Connect to each other using local host <container port>.
	➢ Share access to the same volume.
	➢ Containers within POD are deployed in an all-or-nothing manner.
	➢ Entire POD is hosted on the same node (scheduler will decide about which node).
	
POD limitations:
	➢ No auto healing or auto scaling.
	➢ POD crashes.

Higher level Kubernetes objects:
	a. Replication set: auto scaling and auto healing
	b. Deployment: versioning and rollback
	c. Service:
	d. Static (non-ephemeral) IP and networking.
	e. Volume: non-ephemeral storage                

Advantages and disadvantages of Kubernetes -
----------------------------------------------

Set up Kubernetes and Master node and worker node on AWS -
----------------------------------------------------------

- login into AWS account --> Create 3 instance --> One is master and 2 worker node(ubuntu 16.04, t2 medium{4GB RAM and 2 core CPU}).
- Now using puttygen, Create private key and save it.
- Access all the instances through putty.

--> Commands common for master node and worker node

# sudo su -
# apt-get update
--> Install https package
# apt install apt-transport-https
This https is needed for intra cluster communication (Particuler from control node to individual ports)
--> Install docker on all the three instances.
# sudo apt install docker.io -y
# docker --version
# docker --info
# systemctl status docker
# sysytemctl start --now docker
--> Set up GPG key
This is required for intra communication cluster. It will be added to source key on this node i.e when k8s sends signed info to our hosts,
It is going to accepts those information beacause this open GPG key is present in the source key.
# sudo curl -s https://packages.cloud.google.com/apt... | sudo apt-key add
--> Install the kubernetes
# nano /etc/apt/sources.list.d/kubernetes.list --> open a file
deb http://apt.kubernetes.io/ kubernetes-xenial main --> Add url in the file
exit from nano ctrl+x --> caps Y --> enter
# apt-get update
# apt-get install -y kubelet kubeadm kubectl kubernetes-cni (Perform this stpes in all nodes).

--> Bootstapping the master node

To initiliase K8s cluster - 

# kubeadm init
you will get one long command satrted from "kubeadm ---". Copy this command and save in the notepad.
# mkdir -p $HOME/.kube
Copy configuration to kube directiory
# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
Provide user permission to config file ;
# chown $(id -u):$(id -g) $HOME/.kube/config
Deploy funnel node network for it's all repository path. flannel is going to be place in each node.
# sudo kubectl apply -f https://raw.githubusercontent.com/cor...
# sudo kubectl apply -f https://raw.githubusercontent.com/cor...

--> Configure Worker node

Copy long command provided by master in node now command given below.
# kubeadm join 172.31.6.165:6443 --token kl9fhu.co2n90v3rxtqllrs --discovery-token-ca-cert-hash
  sha256:b0f8003d23dbf445e0132a53d7aa1922bdef8d553d9eca06e65c928322b3e7c0

Minikube installation -
----------------------
- Install Docker
# sudo apt update && apt -y install docker.io

- Install kubectl
#  curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl &&   chmod +x ./kubectl && sudo mv ./kubectl /usr/local/bin/kubectl

- Install Minikube
#  curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

- Start Minikube
#  apt install conntrack

#  minikube status

Go to master and run this command - 
# Kubectl get nodes

Kubernetes objects -
--------------------
	- Kubernetes uses objects to represent the state of the cluster.
	- What containers application are running.
	- The Policies and how those application behaves and faults tolerance.
	- Once you create the object, the Kubernetes system constantly will work to ensure that object exist and maintains cluster desired state.
	- Every Kubernetes Object includes two nested fields that govern the object config.
		1. object specification
		2. object status.
	- The specification, which we provide, describe your desired state for the object - the characteristics that you want that object to have.
	- The status describes the actual state of the object and is supplied and updated by the Kubernetes system.
	- All object are identify by unique name and UID.
	
Basic Kubernetes objects include -
	1. POD
	2. Service
	3. Volume
	4. Namespace
	5. Replicasets
	6. Secrets
	7. ConfigMap
	8. Deployments
	9. Job
	10.Daemonset 

Relationships between pod and the objects
	- Pod Manages containers
	- Replicasets manage pods
	- Services expose pod process to outside the world.
	- ConfigMap and Secrets helps you to configure pods.

Kubernetes Objects -
	- It represents as JSON and yaml files.
	- You create those and push them to the kubernetes API with kubectl
	
State of the object -
	- Relicas (2/2)
	- Image (Tomcat and Ubuntu)
	- Name
	- Volume
	- Startup command
	- Detached (Defaults)

Kubernets Objects Management -
------------------------------
The kubectl command line tools supports servel different ways to create and manage kubernets object

	Management Technique      	Operates on                             Recommanded environment
-----------------------------------------------------------------------------------------------------
	Imperatives Commands      	Live Object                             Developement object
							
	Declarative Object Config 	Individual files(yml/json)              Production
	
Declarative is about describing what you are trying to achieve, without instructing how to do it.
Imperative, explicitly tells how it accomplishes.

Fundamentals of PODS -
---------------------
	- When pods get created, it is scheduled to run on a node in your cluster.
	- The pod remains on that node until the process is terminated, the pod object is deleted, the pod is evicted for lack of resources or node fails.
	- If a pod is scheduled to a node that fails or if the scheduling operation itself is fails, then the pod is deleted.
	- If pod dies, the pods are scheduled for a deletion after a timeout period.
	- A given pod is not rescheduled to a new node, instead it will be replaced by an identical pod, with even the same name if desired but with new UID.
	- Volume in a pod will be existed if that pod (with that UID) exists. if that pod is deleted for any reasons, Volume is also destroyed and created as new on new pod.
	- A controller can create and manage multiple pods, handling replication, roll out and providing self-healing capabilities.

Kubernetes configuration -
-------------------------
	- All in one single node installation (minikube - Proactive purpose)
	  with all-in-one, all the master and worker node components are installed on a single node. 
	  This is very useful for learining, development and testing.
	  This type should not be used in production. Minikube is one such example and we are going to explore it soon.
	- Single node etcd, single mater node and multi-worker node
	  In this setup, We have single master node, which also runs a single-node etcd instance. Multiple worker nodes are connected to the master.
	- Single node etcd, Multi-master node, Mult-worker node Installation
	  In this set up, We have multi-master nodes which work on HA mode. but we have a single node etcd instance. 
	  Multiple worker nodes are connected to the master node.

                             Minikuber installation -             
                             ----------------------

--> Go to aws account -> Launch instance -> Ubuntu 18.04 -> t2 medium (2 vCPU)
--> Now access EC2 via putty -> login as ec2-user

# sudo su -
# sudo apt-get update && apt -y install docker.io
--> Now install kubectl  (curl )
# yum install kubectl
--> Then, Install minitube (curl)
# yum install minitube
# apt install contract
# minikube status
# kubectl version

--> Now, Onwards, we will work with kubectl commands
# kubectl get nodes
# kubectl describe node ip-192.168.34.55

Lab-01] To create a container with the help of yml file.

vi pod1.yml
--------------------------------------------------------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: testpod
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo 'Hello suraj'; sleep 5 ; done"]
  restartPolicy: Never   # Defaults to always
--------------------------------------------------------------------------------------------------------------

--> Create a pod and container
# kubectl apply -f pod1.yml
--> Display pod
kuebctl get pods
--> if you want to see, where exactly pod is running
kubectl get pods -o wide
--> Display pod details
kubectl describe pod testpod
--> To see logs
kubectl logs -f pod_name
--> Delete the pod
kubectl delete pod pod-name
kubectl delete -f pod1.yml

Lab-02] To mention annotations

vi pod1.yml
---------------------------------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: testpod
  annotations:
	description: I am suraj and now logging to the pod using this and it has been created for practice 
spec:
  containers:
    - name: c00
      image: centos
      command: ["/bin/bash", "-c", "while true; do echo 'Hello suraj'; sleep 5 ; done"]
  restartpolicy: Never
----------------------------------------------------------------------------------------- 
kubectl apply -f pod1.yml
kuebctl get pod
kubectl describe pod testpod
kubectl delete pod testod
kubectl delete -f pod1.yml

Lab-03] To create multiple container

--> Multi-container pod2.yml
-----------------------------------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: pod1
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo 'Hello suraj'; sleep 5 ; done"]
    - name: c01
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo 'Hello suraj'; sleep 5 ; done"]
----------------------------------------------------------------------------------------- 
kubectl apply -f pod2.myl
kuebctl get pods
kubectl get pod pod1
kubectl describe pod pod1
kubectl log -f pod1
kubectl logs -f pod1 -c c00
kubectl logs -f pod1 -c c01
kubectl exec pod1 -it -c c01 -- hostname -f
kubectl exec pod2 -it -c c02 -- hostname -f
kubectl exec pod1 -it -c c01 -- /bin/bash
ps
ps -ef
exit
kubectl delete pod pod1
kubectl delete -f pod2.yml

Lab-04] To create a environment varible
--> Environment Variables in the kubernets

vi pod3.yml
----------------------------------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: pod
spec:
  container:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo 'Hello suraj'; sleep 5 ; done"]
	  env:
		- name: myname
		  value: suraj
----------------------------------------------------------------------------------------
kubectl apply -f pod3.yml
kubectl get pods
kubectl get pod pod
kubectl describe pod pod-name
kubectl exec pod -it -c c00 -- /bin/bash
env
echo $myname
exit
kubectl delete -f pod3.yml

Lab-05] Pod with ports
vi pod4.yml
-------------------------------------------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: pods 
spec:
  containers:
    - name: c00
      image: httpd
	  command: ["/bin/bash", "-c", "while true; do echo 'Hello suraj'; sleep 5 ; done"]
      ports:
        - containerPort: 80
-----------------------------------------------------------------------------------------------
kubectl apply -f pod4.yml
kubectl get pods
kubectl get pods -o wide
curl 172.17.3.90
kubectl delete -f pod4.yml
kubectl get pods

													Lables  -
													-------
									
- Lables are the mechanism which you use to organise kubernetes objects.
- A lable is a key-value pair without any predefined meaning that can be attached to object.
- Lables are similar to tags in AWS or git where you use a name to quick refernence.
- So you are free to choose lables as you need to refer an environment which is used for DEV/TEST or PROD, refer a product group depta and deptb.
- Multiple lables can be added to a single object. 

Lab-06] Create labels to the pods

vi pod5.yml
----------------------------------------------------------------------------------
kind: pod
apiversion: v1
metadata:
  name: pod
  lable:
    env: Dev
    class: pods
spec:
  containers:
    - name: c00
      image: ubuntu
	  command: ["/bin/bash","-c","while true;do echo 'Hello Suraj';sleep 5;done"]
----------------------------------------------------------------------------------
kubectl apply -f pod5.myl
kubectl get pods
kubectl get pods --show-labels

--> Now if you want to add a lable to an existing pod imperatively 
kubectl label pods pod-name myname=surajs
kubectl get pods --show-labels

--> Now, List the pods matching a lables
kubectl get pods -l env=Dev

--> Now, give a list, where 'Dev' is not present
kubectl get pods -l env!=Dev

--> Now, you want to delete pod using labels
kubectl delete pod -l env!=Dev

--> Display all the pods
Kubectl get pods

												Labels selectors -
												------------------
- Unlike name/UID, Lables do not provide uniquess, as in generel, we can expect many objects to carry the same label.
- Once lablels are attached to an object, we would need a filter to narrow down and these are called as label selectors.
- The API currently supports two types of selectors. Equity based and set based.
- A label selector can be made of multiple requirements which are comma-seperated.
- Equality based (=, !=)
              name: suraj
              class: nodes
              Project: Dev
- Set based: (in, notin and exists)
              env in (Prod, Dev)
              env notin (team1, team2)
- Kubernetes also supports set-based selectors, i.e., match multiple value
kubectl get pods 'env in (PROD, DEV/TEST, UAT)'
kubectl get pods -l 'env in ()'

												Node selector -
												--------------
- One usecase for selecting labels is to constrain the set of nodes onto which a pod can schedule i.e you can tell a pod to only be able to run on particuler nodes.
- Generally such constraints are unnessessary, as the scheduler will automatically do reasonable placement, but on certain cirumstances we might need it.
- We can use labels to tag nodes.
- The nodes are tagged, you can use the label selectors to specify the pods run only of the specific nodes.
- first we give label to the node then use node selector to the pod configuration.

vi nodeselectors.yml
-------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: nodelabels
  labels:
    env: development
spec:
    containers:
	   - name: c00
	     image: ubuntu
		 command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
    nodeSelector:                                         
       hardware: t2-medium
------------------------------------------------
# kubectl get nodes 
# kubectl label nodes hardware=t2-medium
# kubectl get nodes --show-lables
# kubectl apply -f node-selector.yml
# kubectl get pods -o wide 

                                                Kubernetes Concepts -
												---------------------
- Kubernetes is designed to orchestarte multiple containers and replication.
- Need for multiple containers/replications helps us with there.

Reliability		- By having multiple versions of an applicaiton, you prevent problems if one or more fails.

Load Balancing 	- Having multiple versions of a containers enables you to easily send a traffice to differnent instances 
				  to prevent overloading of a single instance or node.

Scaling			- When load does become too much for the number of existing instances, Kubernetes enables you to easily scale up the application, 
				  adding additional instances as needed.

Rolling update 	- Updates to a service by replacing pods one by one.
 
Replication controller (Object)-
--------------------------------
	- A replication controller is a object that enables you to easily create multiple pods, then make sure that number of pods always exist.
	- A pod is created using RC will be automatically replaced/Created if they does crash, failed or terminated.
	- RC is recommeded if you just want to make sure one pod is always running even after system reboot.
	- you can run the RC with one replica and RC will make sure that the pod is always running.

lab-07]
RC.yml
-----------------------------------------------------------------------------------------------------------------
kind: ReplicationController                            # This defines to check the object of Replication type
apiVersion: v1
metadata:
  name: myreplica
spec:
  replicas: 2                                          # This element defines the desired number of pods
  selector:                                            # Tells the controller which pods to watch/belongs to this RC.
    myname: surajs                                     # This must match the label
  template:                                            # Templete elements defines to launch a new pod
    metadata:
      name: pod_name                                            
    label:                                             # Selector values need to match the labels values specified in the pod template.
      myname: surajs
    spec:
      container:
        - name: c00
          image: ubuntu
          command: ["/bin/bash","-C","while true;do echo 'hello suraj'; sleep 5 ; done"]
-------------------------------------------------------------------------------------------------------------------

kubectl apply -f RC.yml
kubectl get rc
kubectl describe rc myreplica
kubectl get pods
kubectl delete -f pod pod_name
kubectl get pods
kubectl get rc
kubectl describe rc myreplica
kubectl get pods --show-lables
kubectl scale --replicas=8 rc -l myname=surajs
kubectl get rc
kubectl get pods
kubectl scale --relicas=1 rc -l myname=surajs
kubectl get rc
kubectl get pods
kubectl get pods --show-lables
kubectl delete -f myrc.yml

**************************************** Differnece between Replicatset Daemonset Deployment and statefulset *************************
https://semaphoreci.com/blog/replicaset-statefulset-daemonset-deployments
***************************************************************************************************************************************

Replica set (Object)
-------------------
	- Relica set is next generation of replication controller.
	- The replication controller only supports equily based selectors whereas the replica set works on both i.e filtering according to set of values.
	- Replicaset rather than replica controller is used by other.
	- This helps reduce traffic to one particular instance and also helps in load-balancing traffic between each of these instances.
	
vi rc.yml 
-----------

kind: ReplicaSet
apiVersion: apps/v1
metadata:
  name: myreplicaset
spec:
  replicas: 2
  selector:
    matchExpressions:
      - {key: myname, operator: In, values: [Bhupinder, Bupinder, Bhopendra]}
      - {key: env, operator: NotIn, values: [production]}
  template:
    metadata:
      labels:
        myname: Bhupinder
    spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
		  
Difference between ReplicaController and ReplicaSet -
			  
Development objects (Self-healing)-
-----------------------------------
	- Replication controller and replica set is not able to do updates and rollback of apps in the cluster.
	- A deployment object act as supervision for pods, giving you fine grained control over how and when is rolled out, updated and rolled back.
	- When using deployment object, we define the state of the app, kubernetes cluster schedules mentioned app instance and specific individuals nodes.
	- kubernets then moniotrs, if the pod goes down and pod is deleted, deployment controller replaces/deploys new pod.
	- This provides self-healing mechanism to address machine failure or maintenance.
	- A deployment provides declarative updates for pods and replicaset.

The following are typical use cases of deployments -
---------------------------------------------------
- Create deployment to rollout replicaset - 
  The replicaset creates a pods in the background and check the status of rollout if it is successeded or not.
  
- Declare the new state of the pod - 
  By updating the pod template of deployment. A new replicaset is created and the deployment manages moving the pods from
  old replicaset to the new one at controlled rate, each new replicaset updates the revision of the deployment.
  
- Rollback to the eariler deployment revision - 
  if the current state of the deployment is not stable, each rollback updates the revision of the updates.
  
- Scale up the deployment to faciliate more load.

- Pause the deployment to apply multiple fixes to it's PodTemplateSpec and then resume it to start a new rollout.

- Cleanup Old repilicaset that you do not need anymore.

- if there are problems in the deployment, kubernetes automatically roll back to the previsios version, however you can explicitly roll back to specific revision 
  as in our case to revsion 1 (Original Pod versions)
  
- you can rollback to previous verion by specifying it with --to-revision.
# kubectl rollout undo deploy/mydeployments -to-revision=2

- Note: That the name of the replica set is always fromatted as [deployment-name]-[random-string].

# kubectl get deploy
- When you inspect the deployment in the cluster, the following fields are displayed.

	NAME		: List if the deployments in the namespace
	READY		: Display how many replicas of the application are available are available to yours if follows the pattern ready/desired.
	UP-TO-DATE	: Diaplay the number of replicas that have been updated to achive the desired state.
	AVAILABLE	: Displays how many replicas of the application are availble to your user.
	AGE			: Display the amount of time that the application has been running.

Failed deployment - your deployment may get stuck trying to deploy it's newest Replicaset without ever completing. This can occur due to some of following points.
	- Insufficient quata
	- Rediness probe failure
	- Image pull errors
	- Insufficient permission
	- Limit ranges
	- Application runtime misconfiguration

vi deployment.yml
-------------------------------------------------------------------------------------------------
kind: Deployment
apiVersion: apps/v1
metadata:
  name: mydeployments
spec:
  replicas: 2
  selector:
    matchLabels:
      name: deployment
  template:
    metadata:
      labels:
        name: deployment
    spec:
      containers:
        - name: c00
          image: ubuntu
          command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5; done"]
---------------------------------------------------------------------------------------------------

kubectl apply -f deploy.yaml
kubectl get pods
kubectl describe mydeployments
kubectl get rs
kubectl get pods
kubectl delete pod pod_name
kubectl get pods
kubectl get rs
kubectl scale --replicas=1 deploy mydeployments
kubectl scale --replicas=3 deploy mydeployments
kubectl get deploy
kubectl get rs
kubectl get pods
kubectl logs -f pod_name

--> Change on existing file
kubectl apply -f deploy.yaml
kubectl get rs
kubectl get pods
kubectl logs -f pod_name
kubectl exec pod_name -- cat /etc/os-release
kubectl scale --replicas=4 deploy mydeployments
kubectl get rs
kubectl get pods
kubectl scale --replias=2 deployment mydeployments
kubectl get pods
kubectl get rs
kubectl rollout status deployment mydeployments
kubectl get pods
kubectl rollout histroy deployment mydeployments
kubectl rollout undo deploy mydeployments
kubectl get deploy
kubectl get pods
kubectl logs -f pod_name
kubectl exec pod-Name -- cat /etc/os-release

StatefulSet - 
-------------

StatefulSet is the controller that manages the deployment and scaling of a set of Stateful pods.
A stateful pod in Kubernetes is a pod that requires persistent storage and a stable network identity to maintain its state all the time, 
even during pod restarts or rescheduling.
These pods are commonly used for stateful applications such as databases or distributed file systems as these require a stable identity and 
persistent storage to maintain data consistency.

vi pod.yml
--------------------
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nginx-data
          mountPath: /var/www/html
  serviceName: nginx
  volumeClaimTemplates:
  - metadata:
      name: nginx-data
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
		  
		  
DaemonSet - 
---------
A DaemonSet ensures that a single instance of a pod is running on each node in a cluster. While the earlier controller types ensure that a specific number of 
replicas are running across the cluster, DaemonSets are intended to run exactly one pod per node. This is particularly useful for running pods as system daemons
or background processes that need to run on every node in the cluster.
Due to this, DeamonSets can be used for collecting logs, monitoring system performance, and managing network traffic across the entire cluster.

vi demonet.yml
---------------------------------------------------
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
spec:
  selector:
    matchLabels:
      app: fluentd
  template:
    metadata:
      labels:
        app: fluentd
    spec:
      containers:
      - name: fluentd
        image: fluent/fluentd:v1.7.4-1.0
        volumeMounts:
        - name: varlog
          mountPath: /var/log
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
---------------------------------------------------------

terminationGracePeriodSeconds mentions the time allowed for graceful termination

									Difference between Deployment and statefulset - 
									----------------------------------------------

	Feature						Deployment					StatefulSet
	--------------------------------------------------------------------------------------------------
	Purpose						Stateless apps				Stateful apps (databases, queues)
	Pod Identity				Random pod names			Fixed pod names (pod-0, pod-1, etc.)
	Scaling						Any order					Sequential (ensures data integrity)
	Storage						Ephemeral (unless PVC used)	Persistent (separate volume for each pod)
	Pod Replacement				New pod gets a new name		New pod retains the same name
	-----------------------------------------------------------------------------------------------------
	
--> Networking in kubernetes
    ------------------------
Kubernetes Networking addresses four cocnerns :-
	1. Containers within a pod use networking to commiunicate via loopback address(localhost:80)
	2. Cluster networking provides communication between differnent pods.
	3. The service lets you expose an application running in pods to be reacheable from outside your cluster.
	4. You can also use services to publish services only for consumption inside your cluster.

lab-01] Container to container communication on same pod happens through localhost with the containers.

vi pod1.yaml --> Create a file to create pod
---------------------------------------------------------
kind: Pod
apiVersion: v1
metadata:
  name: testpod
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash", "-c", "while true; do echo Hello-Bhupinder; sleep 5 ; done"]
    - name: c01
      image: httpd
      ports:
       - containerPort: 80
---------------------------------------------------------

# kubectl apply -f pod1.yml
# kubectl get pods
--> Go to inside a container -->
# kubectl exec testpod -it -c c00 -- /bin/bash
# apt update && apt install curl
# curl localhost:80  --> You will get replay "It works"
# exit --> logout from the container
# kubectl delete -f pod1.yml

lab-02] Now try to establish communication between two different pods within the same machine.
	1. Pod to Pod communication on the same worker node happens through Pod IP
	2. By defaults pod's ip will not be accessible outside the node.

# vi pod1.yml
------------------------------------------------------------------------------------
kind: pod
apiVersion: v1
metadata:
   name: testpod1
spec:
  containers:
    - name: c00
      image: ubuntu
      command: ["/bin/bash","-c","while true;do echo hello-surajs;sleep 5;done"]
    - name: c01
      image: nginx
      ports:
        - containerPort: 80
-------------------------------------------------------------------------------------

# vi pod2.yml
------------------------------------------------------------------------------------
kind: pod
apiVersion: v1
metadata:
   name: testpod2
spec:
  containers:
    - name: c00
      image: httpd
      ports:
        - containerPort: 80
------------------------------------------------------------------------------------

# kubectl apply -f pod1.yml
# kubectl apply -f pod2.yml
# kubectl get pods
# kubectl get pods -o wide
# curl pod1-ip:80 --> You will get replay "It works"
# curl pod2-ip:80 --> You will get replay "It works"

Services - 
--------
Problems -
	- Each pod gets its own ip address, however in a deployment, set of pod's running in one moment in time could be different from the set of pods running 
	  that application moment later.
	- This leads to problem:- if some set of pods(call them backend) provides functionality to pods (call them frontend) inside your cluster, how do the
	  forntend find out and keep tarck of which ip address connect to, so that the frontend can use the backend part of workload? --> services
	- When using RC, pods are terminated and created during scaling and replication operations.
	- When using deployment, while updating the image version, The pods are terminated and new pods take the place of the other pod.
	- Pod are very dynamic i.e., they come & go on the k8s cluster and any of the availble nodes & it would be diffcult to access the pods as 
	  the pods IP changes once it recreated.
	  
Solution - 
	- Service object is an logical bridge between pods and end users, which provides Virtual IP(VIP).
	- Service allows clients to reliabily connect to the containers which are running in the Pod by using VIP.
	- The VIP is not an actual VIP connected to netwrok interface but it's purpose is purely forward traffice to one or more pods.
	- kubeproxy is the one which keeps the mapping between the VIP and pods with the dates, which queries the API server to learn about new services in the cluster.
	- Although each pod has unique ip address, those ip's are not exposed to outside the cluster. 
	- Services helps to expose the VIP mapped to the pods and allow application to receive traffice. 
	- Lables are used to select which are the pods to be put under a service.
	- Creating a service will create an endpoint to access the pods/application in it.
	- Serivce can be exposed in differnent ways by specifying a type in the service spec	
		1. ClusterIP
		2. NodePort
		3. Loadbalancer 
		4. Headless 
	- Loadbalancers are created by cloud provider that will route external traffice to every node on the nodeport.
	- Headless Creates several endpoints that are used to produce DNS records. Each DNS record is bound to a pod.
	- By defaults, service can run only between ports 30000-32767
	- The set of pods targeted by a service is usually determined by selector. 

Types of the services  -
	1. ClusterIP
	2. NodePort
	3. Loadbalancer 
	4. Headless  
	5. Ingress Controller 
	
1.Cluster IP
-------------
	- Expose VIP only reachable within the cluster 
	- Mainly used to communicate between components of microservices.

vi httpddeploy.yaml
---------------------------------------------------------------
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
      matchLabels:
        name: deployment
   template:
     metadata:
       name: testpod1
       labels:
         name: deployment
     spec:
       containers:
         - name: c00
           image: httpd
           ports:
		   - containerPort: 80
------------------------------------------------------------------------
kubectl apply -f deployhttpd.ymlcluster
kubectl get pods
kubectl get pods -o wide
curl pod-ip

vi Service.yml
-----------------------------------------------------------------------------------------
kind: Service                         # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
	- port: 80                        # Containers port exposed
      targetPort: 80                  # Pods port
  selector:
    name: deployment                  # Apply this service to any pods which has the specific label
  type: ClusterIP                     # Specifies the service type i.e ClusterIP 
---------------------------------------------------------------------------------------------------
kubectl apply -f service.yml
kubectl get svc
curl service-cluster-ip:80
kubectl delete pod pod_name
kubectl get pods
curl service-cluster-ip:80
kubectl delete -f service.yml
kubectl delete -f pod1.yml
kubectl delete -f pod2.yml
kubectl get pods
kubectl delete -f httpdservicedeploy.yml
kubectl get po

2.NodePort
-----------
	- Make the service accessible outside the cluster. 
	- Expose the services on the same port of each selected node in the cluster using NAT. 

vi httpddeploy.yml
---------------------------------------------------------------
kind: Deployment
apiVersion: apps/v1
metadata:
   name: mydeployments
spec:
   replicas: 1
   selector:      # tells the controller which pods to watch/belong to
     matchLabels:
      name: deployment
   template:
     metadata:
       name: testpod1
       labels:
         name: deployment
     spec:
       containers:
         - name: c00
           image: httpd
           ports:
		   - containerPort: 80
------------------------------------------------------------------------
kubectl apply -f httpddeploy.yml

vi service.yml
-----------------------------------------------------------------------------------------
kind: Service                         # Defines to create Service type Object
apiVersion: v1
metadata:
  name: demoservice
spec:
  ports:
	- port: 80                        # Containers port exposed
      targetPort: 80                  # Pods port
  selector:
    name: deployment                  # Apply this service to any pods which has the specific label
  type: NodePort                     # Specifies the service type NodePort
---------------------------------------------------------------------------------------------------
kubectl applay -f service.yml
kubectl get svc
kubectl get pods
kubectl describe svc demoservice
copy ip address,ports and paste on chorme
kubectl delete -f serivce.yml

  														 Volumes
                                                         -------
- Containers are short lived in nature.
- All data stored inside the a container will be deleted if the containers crashes. however the kublet will restart it with clean state, which means, 
  that will not have any of the old data.
- To overcome the problem, kubernetes uses volumes. A volumes is essentially a directiory backed by a storage medium. The storage medium and it's 
  content are determined by volume type.
- In kubernetes, a volume is attached to a Pod and shared amoung containers of the pod.
- The volume has the same life span as the pod and it outlives the container of the pod this allows data to be preserved across container restarts..
 
Volume types  - 
- A volume type decides the proprties of the directory, Like size, content etc. 
examples - 
	- Node-local Volume types such as empty-directory and hostpath
	- File sharing Volume type such as NFS 
	- Clound provide-specific Volume types like aws elasticebackstore azuredisk.
	- Distributed file system Volume types like glusterfs and Cephfs 
	- Special Purpose Volume types like gitrepo and Secrets
	
EmptyDir - 


1. Container to container within a pod

vi emptydir.yml
-----------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: myvolemptydir
spec:
  containers:
  - name: c1
    image: centos
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:                                    # Mount definition inside the container
      - name: xchange
        mountPath: "/tmp/xchange"         
  - name: c2
    image: centos
    command: ["/bin/bash", "-c", "sleep 10000"]
    volumeMounts:
      - name: xchange
        mountPath: "/tmp/data"
  volumes:                                                  
  - name: xchange
    emptyDir: {}
---------------------------------------------
kubectl apply -f emptydir.yml
kubectl get pods
kubectl exec pod-name -it -c c1 -- /bin/bash
cd /tmp
cd xchange/
ls
echo "Kubernetes" >> text.txt
pwd
kubectl exec pod-name -it -c c2 -- /bin/bash
cd /tmp/data
ls  --> test.txt
vi test.txt --> add content in the file
kubectl exec pod-name -it -c c1 -- /bin/bash
kubectl delete -f emptydir.yml

2. hostpath to pod volume access

vi hostpath.yml
--------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: myvolhostpath
spec:
  containers:
  - image: centos
    name: testc
    command: ["/bin/bash", "-c", "sleep 15000"]
    volumeMounts:
    - mountPath: /tmp/hostpath
      name: testvolume
  volumes:
  - name: testvolume
    hostPath:
      path: /tmp/data
---------------------------------------------------------

kubectl apply -f hostpath.yml
kubectl get pods
ls /tmp/data
kubectl get pods
kubectl exec pod-name -- ls /tmp/hostpath
kubectl exec pod-name -- ls /tmp
echo "Love" > myfile
pwd
kubectl exec pod-name -- cat /tmp/hostpath/myfile
kubectl exec pod-name -c -it -- /bin/bash
cd /tmp/hostpath
echo "i am fine" >> test1.txt
exit
cd /tmp/data --> check in the node path

Persistence volume claim -

Namespace - 
---------

	- You can name a object, if many are using cluster then it would be diffcult for managing.
	- A Namespace is a group of related elements that each have unique name or identifiers.
	- Namespace is used to uniquicly identify one or more names from other similar names of different object or groups or namespace in general.
	- kubernetes namespace helps differnent project, teams and customers to share kubernetes cluster and provides
		1. A scope of every names.
		2. A mechanism to attach authorization and policy to a subsection of a cluster.
	- A kubernetes cluster instantiate a default namespace while provisioning the cluster to hold the defaults set of pod, services, deployments used by the cluster.
	- We can use resouces quota on specifying how many resouces each namespace can use.
	- Most kunernetes resouces (Pods, services, deployments, replication controller and others) are in same namespace.
	- Namespaces are intented to use on environtments with many users who speard across the teams or projects for cluster with few to ton's of users. 
	- you should not need to create or think of namespace and all.
 
Create New Namespace -
----------------------
	- Let us suppose we have shared k8s cluster for DEV and PROD use cases.
	- The DEV team would like to maintain a space in cluster where they can get a view on the list of pods, services and deployments they use to build and run.
	  their application, In this no restriction are put on who can or can not modify resources to enable agile deployment.
	- For PROD, we can enforce strict procedure on who can or cannot manipulate the set of pods, service and deployments.
	
# kubetctl get namespaces
---------------------------------
apiVersion: v1
kind: Namespace
metadata:
   name: dev
   labels:
     name: dev
----------------------------------
--> To change default namespace.

# kubectl config set-context $(kubectl config current-context) --namespace=dev

vi pod.yml
------------------------------------------------------------------------------------------
kind: Pod                             
apiVersion: v1                    
metadata:                          
  name: testpod                 
spec:                                   
  containers:                     
    - name: c00                    
      image: ubuntu             
      command: ["/bin/bash", "-c", "while true; do echo Technical Guftgu; sleep 5 ; done"]
  restartPolicy: Never
------------------------------------------------------------------------------------------

# kubectl apply -f pod.yml -n dev
# kubectl get pods
# kubectl get pods -n dev
# kubectl delete -f pod.yml --> can not delete
# kubectl delete -f pod.yml -n dev -> you have to mention namespace then it will delete pod
# kubetctl get pods -n dev
# kubectl apply -f pod.yml
- To change default namespace.
# kubectl config set-context $(kubectl config current-context) --namespace=dev
# kubectl get pods
# kubectl config view | grep namespace:
# kubectl config view
# kubectl get pods -n default
# kubectl get pods
# kubectl delete -f pod.yml .

Storage - 

Network Policies in Kubernetes
------------------------------
Kubernetes NetworkPolicies are used to control communication between pods at the network level. 
They define rules that specify how groups of pods are allowed to communicate with each other and with other network endpoints.

1. Key Concepts :- 
	Ingress rules		: Control incoming traffic to a pod.
	Egress rules		: Control outgoing traffic from a pod.
	Selectors			: 
		PodSelector			: Defines which pods the policy applies to.
		NamespaceSelector	: Limits communication to pods in a specific namespace.
		IPBlock				: Allows or denies traffic based on IP ranges.
		
2. Default Behavior
	By default, all pods can communicate with each other without any restrictions.
	Once a NetworkPolicy is applied to a pod, it denies all traffic except the ones explicitly allowed.
	
3. Creating a NetworkPolicy
	A basic example to allow ingress traffic from a specific pod in the same namespace:
	
------------------------------
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-app-traffic
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
    ports:
    - protocol: TCP
      port: 80
------------------------------------------
Explanation:
	Applies to pods labeled app=my-app.
	Only allows ingress traffic from pods labeled app=frontend.
	Only allows traffic on TCP port 80.
	
4. Denying All Traffic (Default Deny Policy)
	To deny all ingress traffic, use an empty ingress field:

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  
Explanation:
Applies to all pods in the namespace.
Denies all inbound traffic unless allowed by another policy.

5. Allowing Egress Traffic
To allow only outgoing traffic to a specific IP range:

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-egress
  namespace: default
spec:
  podSelector:
    matchLabels:
      app: my-app
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 192.168.1.0/24
    ports:
    - protocol: TCP
      port: 443
Explanation:
Pods labeled app=my-app can only send traffic to IPs in 192.168.1.0/24.
Only allows outgoing traffic on TCP port 443.

6. Common Use Cases
	Use Case							Network Policy Example
	Restrict pod communication			Use podSelector to allow traffic between specific pods.
	Isolate namespaces					Use namespaceSelector to block access between namespaces.
	Allow traffic from specific IPs		Use ipBlock to allow only certain external IPs.
	Control external egress traffic		Restrict pod access to external networks (e.g., Internet).
	
7. Important Notes
Network policies only work if your Kubernetes cluster uses a CNI (Container Network Interface) that supports them, such as:
	Calico
	Cilium
	Weave Net
	Kube-router
If no network policy is applied, all traffic is allowed.

8. How to Apply and Verify
- Apply a Network Policy
# kubectl apply -f network-policy.yaml
- Check Existing Policies
# kubectl get networkpolicy -A
- Describe a Specific Policy
# kubectl describe networkpolicy allow-app-traffic

Resource Quata -
--------------

	- A pod in kubernetes will run with on limit of resources.
	- you can specify optionallly how much CPU and Memory each container needs.
	- Scheduler decides about which nodes to place pod, Only if the node has enough memory and CPU resources available to satisfy pod memory and cpu request.
	- CPU is specified in units of cores and memory is sepcified in units of bytes.

Two types of contraintes
	1. Request
	2. Limit

	- A request is amount of resources that the system will guarantee the container and kubernetes will use this value to decide on which node to place the pod.
	- A Limit is the max amount of resources that the kubernetes will allow the container to use. 
	  In the case the request is not set for container it default to limits and if the limit is not set then it defaults to 0.
	- CPU value is specified in "millicpu " and memory in "MiB"
	- A kubernetes can be divied in to namespace if container is created in the namespace that has default CPU Limit and container does not specify it's own
	  CPU limit then container is asssigned to default cpu limit.
	- Namesapces can be assigned resources quata objects, this will limit the amount of usage allocated to the objects in the namespace.

You can Limit-
	1. Compute
	2. Memory
	3. Storage
	
- Here are the two restriction that a resouces impose on the namespace.
    1. Every containers that run in the namesapce must have it's own CPU limit.
    2. The total amount of resources(CPU/Memory) used by all containers in the namespaces must not exceed specified limit.

Lab-01] Both are defined means request and limit both are defined.

vi podresouces.yml
--------------------------------------------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: resources
spec:
  containers:
  - name: resources7
    image: centos
    command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
    resources:                                         
      requests:
        memory: "64Mi"
        cpu: "100m"
      limits:
        memory: "128Mi"
        cpu: "200m"
-----------------------------------------------------------------------------------------------
# kubectl apply -f podresouces.yml
# kubetctl get pods
# kubetctl describe pod resources
# kubectl delete -f podresources.yml

Lab-02] Limit is not defined, only request is defined. (Resource quota example)
vi resoucesquota.yaml
------------------------------------------------------------------------------------------------
apiVersion: v1
kind: ResourceQuota
metadata:
   name: myquota
spec:
  hard:
    limits.cpu: "400m"
    limits.memory: "400Mi"
    requests.cpu: "200m"
    requests.memory: "200Mi"
--------------------------------------------------------------------------------------------------
# kubectl apply -f resoucesquota.yaml
# vi testpod.yml
--------------------------------------------------------------------------------------------------
kind: Deployment
apiVersion: apps/v1
metadata:
  name: deployments
spec:
  replicas: 3
  selector:     
    matchLabels:
		objtype: deployment
  template:
    metadata:
      name: testpod8
      labels:
        objtype: deployment
    spec:
     containers:
       - name: c00
         image: ubuntu
         command: ["/bin/bash", "-c", "while true; do echo Technical-Guftgu; sleep 5 ; done"]
         resources:
            requests:
              cpu: "200mi"
-----------------------------------------------------------------------------------------------

# kubectl apply -f testpod.yml
# kubectl get deploy --> pod is not created due to lack of resources
# kubectl get pod
# kubectl get rs
# kubectl describe rs
# kubectl delete -f testpod.yml
# kubectl delete -f resourcequota.yml

Lab-03] CPU limit range

vi cpulimitrange.yml
----------------------------------------------------------------------------------------------
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
-----------------------------------------------------------------------------------------------
# kubectl apply -f cpulimitrange.yml
# kubectl apply -f testpod.yml
# kubectl describe pods

Lab-04] limit is defined but request is not defined then limit = request
vi cpulimitrange.yml
------------------------------------------------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-2
spec:
  containers:
  - name: default-cpu-demo-2-ctr
    image: nginx
    resources:
      limits:
        cpu: "1"
-----------------------------------------------------------------------------------------------

# kubectl apply -f cpulimitrange.yml
# kubectl get pod

Lab-05] Request defined and limit is not defined then request -nq limit will be 1 CPU.

vi cpulimitrange.yml
-----------------------------------------------------------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-3
spec:
  containers:
  - name: default-cpu-demo-3-ctr
    image: nginx
    resources:
      requests:
        cpu: "0.75"
-----------------------------------------------------------------------------------------------
# kubectl apply -f cpulimitrange.yml
# kubectl describe pods mypod

              Resources quota 
              ---------------

Default range of CPU and memory for a container.

CPU -
  request: 0.5
  limit: 500 mili
memory:
  reqeust: 500MB
  limit: 1G

Lab-01]
vi cpu.yml
------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-2
spec:
  containers:
  - name: default-cpu-demo-2-ctr
    image: nginx
    resources:
      limits:
        cpu: "1"
--------------------------------------

# kubectl apply -f cpu.yml
# kubectl get pods
# kubectl describe pod pod_name
# kubectl delete -f cpu.yml

Lab-02]

vi cpu.yml
---------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: default-cpu-demo-3
spec:
  containers:
  - name: default-cpu-demo-3-ctr
    image: nginx
    resources:
      requests:
        cpu: "0.75"
-------------------------------------------

# kubectl apply -f cpu.yml
# kubectl get pods
# kubectl describe pod pod_name

Lab-03]

vi memorylimitrange.yml
--------------------------------------------
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-min-max-demo-lr
spec:
  limits:
  - max:
      memory: 1Gi
  - min:
      memory: 500Mi
    type: Container
----------------------------------------------

# kubectl apply -f memorylimitrange.yml

# vi memory.yml
-----------------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: constraints-mem-demo
spec:
  containers:
  - name: constraints-mem-demo-ctr
    image: nginx
    resources:
      limits:
        memory: "800Mi"
      requests:
        memory: "600Mi"
-------------------------------------------------

# kubectl apply -f memory.yml
# kubectl get pods
# kubectl describe pod constraints-mem-demo-ctr

Horizontal pod autoscaler (HPA)-
--------------------------------
	- Kubernetes has the posibilities to automatically scale pods based on observed CPU utilization, which is horizontal pod scaling.
	- Scaling cab be done only for scalable object like controller, deployment and replicaset.
	- HPA is implemented as a kubernetes API resource and controller.
	- The HPA controller periodically adjust the number of replicas in a replication controller or deployment to match observed CPU average utilization 
	  to the target specified by user.
	- The HPA is implemented as a controlled loop with a period controlled by the controller manager's HPA sync-period flag (default value - 30 sec).
	- During each period, the controller manager queries the resource utilization against metric specified in each hpa manifest defination.
	- for pre-pod resouces mertric(like CPU), the controller fetches the metric from the resouce metrics API for each pod targeted by the HPA.
	- Then, if a target utilization value is set, the controller calculates the utilzation value as a percentage of the equivalent resouces request on the container in each pod.
	- If a traget raw-value is set, the raw metric values are used directly. The controller then takes the mean of the utilization on the raw value across all
	  targeted pods and produces a ratio used to scale the number of desired repilicas.
	- Cooldown period to wait before another downscale operation can be performed is controlled by hap-stabilization flag (Default value of 5 min)
	- Metric servers needs to be deployed in the cluster to provide metrics via the resouces metric API '--kubelet-insecure-tls'.

# kubectl autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=10

====================================================================
$ wget -O metricserver.yml https://github.com/kubernetes-sigs/me...

# vi hpa.yml (If request is not specified & limit is given, then request = limit)
-------------------------------------------------------------------------
kind: Deployment
apiVersion: apps/v1
metadata:
  name: mydeploy
spec:
  replicas: 1
  selector:
    matchLabels:
      name: deployment
    template:
     metadata:
       name: testpod8
       labels:
         name: deployment
     spec:
      containers:
        - name: c00
          image: httpd
          ports:
          - containerPort: 80
          resources:
            limits:
              cpu: 500m
            requests:
              cpu: 200m
-------------------------------------------------------------------------

$ kubectl autoscale deployment mydeploy --cpu-percent=20 --min=1 --max=10

                                                          Jobs
                                                          ----

- We have replicasets, Deamonsets, statefuleset and deployments and they all shares all comman property, they ensure that the pod are always running. 
- If pod fails, the controller restart it or reschedules it to another node to make sure the application the pods is hostng keeps running.
- Jobs does not get deleted by itself, we have to delete it.

Use cases -
			1. Take backup of DB
			2. Helm charts uses jobs
			3. Running batch process
			4. Run task at an schedule internal
			5. Log rotation
			
Example - 

# vi jobs.yml
-------------------------------------------------------------------------
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  template:
    metadata:
      name: testpod
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
      restartPolicy: Never
---------------------------------------------------------------------------
# Kubectl apply -f jobs.yml

apiVersion and kind	: Specifies that this resource is a Job.
metadata			: Contains the name of the Job.
spec				: Defines the pod template that specifies the container and the command to run.
restartPolicy		: Ensures the pod is restarted on failure.
backoffLimit		: Limits the number of retries before considering the Job as failed.

Parallism -

jobs.yml
----------------------------------------------------------------------------
apiVersion: batch/v1
kind: Job
metadata:
  name: testjob
spec:
  parallelism: 5                           # Runs for pods in parallel
  activeDeadlineSeconds: 10                # Timesout after 30 sec
  template:
    metadata:
      name: testpod
    spec:
      containers:
      - name: counter
        image: centos:7
        command: ["bin/bash", "-c", "echo Technical-Guftgu; sleep 20"]
      restartPolicy: Never
----------------------------------------------------------------------------
# kubectl apply -f jobs.yml

      Cron Job Pattern
      ----------------
- If you have multiple nodes hosting the application for HA, which nodes handles cron ?
- What happens if multiple identical cron jobs run simultaneously.

jobs.yml
------------------------------------------------------------------------------
apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: bhupi
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - image: ubuntu
            name: bhupi
            command: ["/bin/bash", "-c", "echo Technical-Guftgu; sleep 5"]
          restartPolicy: Never
-------------------------------------------------------------------------------

Real-Life Use Case: Automating Database Backups
-----------------------------------------------
automating the backup of a MongoDB database running in a Kubernetes cluster

vi mongodb-deployment.yaml


                                          Init Containerns (Initializing)
                                          -------------------------------
										  
1. Init containers are specilised containers that run before application in a pod.
2. Init containers always run to completion.
3. If a pods init containers fails, kuberbetes repeatedly restarts the pod until init containers succeed.
4. Init containers do not support readiness problem.

Use Cases -
			1- Seeding a database.
			2- Deploying the application launch until the dependencies are ready.
			3- Clone a git repositoy in a volume
			4- Generates configuration file dynamically.

vi init.yml
----------------------------------------------------------------------------------------------------
appVersion: v1
kind: pod
metadata: Pod
  name: initcontainers 
spec:
  initContainers:
  - name: c1
    image: centos
    command: ["/bin/bash","-c","echo 'Hello Suraj' >> /tmp/xchange/testfile;sleep 30"]
    volumeMounts:
              - name: xchange
                mountPath: "/tmp/xchange"
  containers:
  - name: c2
    image: centos
    command: ["/bin/bash","-c","while true; do echo `cat /tmp/data/testfile`;sleep 5;done"]
    volumeMounts:
                - name: xchange
                  mountPoints: "/tmp/data"
  volumes:
  - name: xchange
    empttdir: {}
-------------------------------------------------------------------------------------------------------
# kubectl apply -f init.yml
# kubectl get pod
# watch kubectl get pods

Service Mesh - Istio (Getaway)

							POD Lifscycle :- 
							-------------

" Pending --> Running --> succeed --> failed --> Completed --> Unknown "

The phase of pod is simple, high level-summary, of where the pod is in it's lifecycle.

Pending -
	- The pod has been accepted by the k8s system, but it is not running.
	- One or more of the container images are still downloading.
	- The pod can not be scheduled due to contraints.
	
Running -
	- The pod has been bound to a node.
	- All the containers have been created.
	- Atleast one container is still running or is in process of starting of restarting.

Succeded -
	- All containers in the pod have been terminated successfully and will not be restarted.

failed - 
	- All containers in the pod have been terminated and kubernetes has failed at least one container in failure.
	- The container either existed with non-zero status or was terminated by system.

Unknown -
	- State of the pod could not be obtained.
	- Typically due to an error in the network or commiunicating with the host of the pod.

Completed -
	- The pod has been running to completion as there is nothing to keep it running.

							POD Condition -
							---------------

- A pod has a PodStatus, which has an array of PodConditions through which pod has or has not been passed.
- Using "kubectl describe pod pod_name". you can get condition of the pod.

Types -
		PodSeheduled - The pod has been scheduled to the node.
		Ready 		 - The pod is able to serve request and will be added to the load balancing pods of all matching service.
		Initialized  - All init container have been started successfully.
		Unscheduled  - The kubernetes can't schedule the pod right now e.g., Due to lacking of resouces or other constraints.

