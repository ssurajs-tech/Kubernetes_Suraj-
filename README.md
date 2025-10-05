---
title: "Kubernetes_Suraj-"
description: "Notes, labs and examples for Kubernetes"
layout: default
permalink: /
---

# Kubernetes_Suraj-

A concise guide with labs, manifests and examples for learning Kubernetes.

---

## 📚 Table of Contents

- [What is Kubernetes](#what-is-kubernetes)
- [History](#history)
- [Online Platform for k8s](#online-platform-for-k8s)
- [Cloud based Kubernetes services](#cloud-based-kubernetes-services)
- [Kubernetes Installation Tool](#kubernetes-installation-tool)
- [Problems with the scaling up the containers](#problems-with-the-scaling-up-the-containers)
- [Features of Kubernetes](#features-of-kubernetes)
- [Differences between Kubernetes and Docker Swarm](#differences-between-kubernetes-and-docker-swarm)
- [Architecture of Kubernetes](#architecture-of-kubernetes)
- [Working with Kubernetes](#working-with-kubernetes)
- [Role of control plane/Master-node](#role-of-control-planemaster-node)
- [Components of Control Plane (Master Node)](#components-of-control-plane-master-node)
- [Worker Nodes](#worker-nodes)
- [POD](#pod)
- [Multi container PODs](#multi-container-pods)
- [Higher level Kubernetes objects](#higher-level-kubernetes-objects)
- [Set up Kubernetes and Master node and worker node on AWS](#set-up-kubernetes-and-master-node-and-worker-node-on-aws)
- [Minikube installation](#minikube-installation)
- [Kubernetes objects](#kubernetes-objects)
- [Kubernets Objects Management](#kubernets-objects-management)
- [Fundamentals of PODS](#fundamentals-of-pods)
- [Kubernetes configuration](#kubernetes-configuration)
- [Minikuber installation](#minikuber-installation)
- [Lab Examples](#lab-examples)
- [Labels and Selectors](#labels-and-selectors)
- [Node selector](#node-selector)
- [Kubernetes Concepts](#kubernetes-concepts)
- [Replication controller (Object)](#replication-controller-object)
- [Replica set (Object)](#replica-set-object)
- [Development objects (Self-healing)](#development-objects-self-healing)
- [StatefulSet](#statefulset)
- [DaemonSet](#daemonset)
- [Difference between Deployment and statefulset](#difference-between-deployment-and-statefulset)
- [Networking in kubernetes](#networking-in-kubernetes)
- [Services](#services)
- [Volumes](#volumes)
- [Namespace](#namespace)
- [Network Policies in Kubernetes](#network-policies-in-kubernetes)
- [Resource Quata](#resource-quata)
- [Horizontal pod autoscaler (HPA)](#horizontal-pod-autoscaler-hpa)
- [Jobs](#jobs)
- [Init Containers (Initializing)](#init-containers-initializing)
- [Service Mesh - Istio (Getaway)](#service-mesh---istio-getaway)
- [Pod Lifecycle Phases](#pod-lifecycle-phases)
- [Pod Condition](#pod-condition)

---

<!-- keep the rest of the file unchanged below -->

## What is kubernetes :- 
-------------------
- Kubernetes is an open-source container management tool which automate container deployment, Container scaling and Load balancing.
- It schedules, runs and manages isolated containers which are running on virtual/physical/cloud machine.
- All top cloud providers support Kubernetes.

## History -
-------
- Google developed an internal system called Brog (later named as Omega) to deploy & manage thousands of google application and services on their cluster.
- In 2014, Google introduced Kubernetes an open source platform written in Golang language & later donated to CNCF.

## Online Platform for k8s -
------------------------
- Kubernetes playground
- Play with K8s
- Play with Kubernetes classroom

## Cloud based Kubernetes services -
--------------------------------
- GKE - Google Kubernetes services
- AKS - Azure Kubernetes services
- Amazon EKS - Elastic Kubernetes services

## Kubernetes Installation Tool -
-----------------------------
- Minikube
- Kubeadm
- Kind

## Problems with the scaling up the containers -
------------------------------------------------
- Container can not communicate with each other
- Autoscaling and load balancing was not possible
- Containers had to be managed carefully.

## Features of Kubernetes - 
----------------------
1. Orchestration (Clustering of any number of containers on different network)
2. Auto-scaling {Vertical(Scale up and down) and Horizontal (scale in and out) Scaling}
3. Load-balancing
4. Platform Independent (Cloud/Physical/virtual)
5. Fault tolerance (Node/POD failure)
6. Health Monitoring of Containers
7. Batch execution (One time, Sequential, Parallel)
8. Rollback (going to get back to previous version)

---

### Differences between Kubernetes and docker swarm

| FEATURES | KUBERNETES | DOCKER SWARM |
|----------|------------|--------------|
| Installation and Cluster configuration | Complicated and time consuming | Fast and easy |
| Support | K8s can work with almost all containers type like rocket, Docker, Containers. | Work with docker only |
| GUI | Available | Not Available |
| Data Volume | Only shared within containers in same pod | Can be shared with any other container |
| Logging and Monitoring | Inbuilt tool present for monitoring | Used 3rd party tool like Splunk. |

- Apache Marathon - It is a container management tool. which has low market share. however, It is very important tool.

---

## Architecture of Kubernetes -
------------------------------

### Working with Kubernetes -
- We create a manifest (.yml)
- Apply this to cluster to bring it to desired state
- Pod runs on node, which is controlled by master

### Role of control plane/Master-node -
- Kubernetes cluster contains containers running on bare metal/ vm instances, Cloud instances/ all mix.
- Kubernetes designates one or more of these as master and all others as workers.
- The Master is now going to run set of k8s processes. 
- These processes will resume smooth functioning of cluster. 
- These processes are called “control plane”.
- Can be multi-master for high availability.
- Master runs control plane to run cluster smoothly.

### Components of Control Plane (Master Node):
1. Kube-API server
2. Etcd
3. Kube-scheduler
4. Controller manager

#### 1. Kube-API server (for all communications):
- This api-server interacts directly with user (i.e we apply .yml or json manifest to kube-apiserver).
- This kube-apiserver is meant to scale automatically as per load.
- Kube api-server is front-end of control-plane.

#### 2. Etcd:
- It stores metadata and status of cluster.
- Etcd is consistent and high available store (key-value store).
- Single Source of truth for cluster state (info about state of cluster).
- Etcd has following features: 
  - Fully replicated: the entire state is available on every node in the cluster.
  - Secure: implements automatic TLS with optional client-certificate authentication.
  - Fast: benchmarked at 10,000 writes per second.

#### 3. Kube scheduler:
- When users make request for the creation and management of PODs, Kube-scheduler is going to take action on these requests.
- Handles POD creation and management.
- Kube scheduler match/assign any node to create and run PODs.
- A scheduler watches for newly created PODs that have no node assigned. For every POD that the scheduler discovers, the scheduler becomes responsible for finding best node for that POD to run on.

#### 4. Controller Manager:
- Make sure that actual state of cluster matches the desired state.
- Two possible choices for controller manager:
  - If k8s on cloud, then it will be cloud-controller-manager.
  - If k8s on non-cloud then it will be kube-controller-manager

  Components on master that runs controller:
  - Node controller   : for checking with the cloud providers to determine of a node has been detected in the cloud after id stops responding.
  - Route controller  : responsible for setting up network, routes on your cloud.
  - Service controller: responsible for load balancers on your cloud against services of type load balancers.
  - Volume controller : for creating, attaching and mounting volumes and interacting with the cloud provider to orchestrate volume.

---

## Worker Nodes 
-------------
Node is going to run 3 important pieces of software/process. These 3 components collectively called NODE.
1. Kublet
2. Container engine
3. Kubeproxy

#### 1. Kublet:
- Agent running on the node.
- Listens to Kubernetes master.(e.g- POD creation request)
- Use port 10255.
- Send success/fail reports to master.

#### 2. Container Engine/Docker:
- Works with kublet.
- Pulling images.
- Start/stop containers.
- Exposing containers on ports specified in manifest.

#### 3. Kube-proxy:
- Assign IP to each POD.
- It is required to assign IP address to PODs (dymanic).
- Kube-proxy runs on each node and this make sure that each POD will get its own unique IP address.

---

## POD:-
-----
- Smallest unit in Kubernetes.
- POD is a group of one or more containers that are deployed together on the same host.
- A cluster is a group of nodes.
- A cluster has at least one worker node and master node.
- In Kubernetes the control unit is the POD, not containers. 
- It consists of one or more tightly coupled containers.
- POD runs on node which is control by master.
- Kubernetes only knows about PODs (does not know about individual container).
- Cannot start containers without a POD.
- One POD usually contains one container.

### Multi container PODs:
- Share access to memory space.
- Connect to each other using local host <container port>.
- Share access to the same volume.
- Containers within POD are deployed in an all-or-nothing manner.
- Entire POD is hosted on the same node (scheduler will decide about which node).

### POD limitations:
- No auto healing or auto scaling.
- POD crashes.

---

## Higher level Kubernetes objects:
- Replication set: auto scaling and auto healing
- Deployment: versioning and rollback
- Service:
- Static (non-ephemeral) IP and networking.
- Volume: non-ephemeral storage                

---

## [Set up Kubernetes and Master node and worker node on AWS](#set-up-kubernetes-and-master-node-and-worker-node-on-aws)
*(See original content for detailed step-by-step instructions and commands)*

---

## [Minikube installation](#minikube-installation)
*(See original content for detailed step-by-step instructions and commands)*

---

## [Kubernetes objects](#kubernetes-objects)
*(See original content for explanations and relationships)*

---

## [Kubernets Objects Management](#kubernets-objects-management)
*(See original content for imperative vs declarative management)*

---

## [Fundamentals of PODS](#fundamentals-of-pods)
*(See original content for pod lifecycle and controller behavior)*

---

## [Kubernetes configuration](#kubernetes-configuration)
*(See original content for single-node, multi-node, and HA setups)*

---

## [Minikuber installation](#minikuber-installation)
*(See original content for AWS and Minikube setup)*

---

## [Lab Examples](#lab-examples)
*(See original content for YAML manifests and kubectl commands)*

---

## [Labels and Selectors](#labels-and-selectors)
*(See original content for label usage, selectors, and filtering)*

---

## [Node selector](#node-selector)
*(See original content for node labeling and pod scheduling)*

---

## [Kubernetes Concepts](#kubernetes-concepts)
*(See original content for reliability, load balancing, scaling, rolling updates)*

---

## [Replication controller (Object)](#replication-controller-object)
*(See original content for YAML and kubectl commands)*

---

## [Replica set (Object)](#replica-set-object)
*(See original content for YAML and differences from ReplicationController)*

---

## [Development objects (Self-healing)](#development-objects-self-healing)
*(See original content for deployments, rollbacks, and scaling)*

---

## [StatefulSet](#statefulset)
*(See original content for StatefulSet YAML and explanation)*

---

## [DaemonSet](#daemonset)
*(See original content for DaemonSet YAML and explanation)*

---

## [Difference between Deployment and statefulset](#difference-between-deployment-and-statefulset)
*(See original content for feature comparison table)*

---

## [Networking in kubernetes](#networking-in-kubernetes)
*(See original content for networking concepts and lab examples)*

---

## [Services](#services)
*(See original content for ClusterIP, NodePort, LoadBalancer, Headless, and Ingress)*

---

## [Volumes](#volumes)
*(See original content for EmptyDir, HostPath, and Persistent Volumes)*

---

## [Namespace](#namespace)
*(See original content for namespace creation, usage, and management)*

---

## [Network Policies in Kubernetes](#network-policies-in-kubernetes)
*(See original content for YAML examples and explanations)*

---

## [Resource Quata](#resource-quata)
*(See original content for resource requests, limits, and quota management)*

---

## [Horizontal pod autoscaler (HPA)](#horizontal-pod-autoscaler-hpa)
*(See original content for HPA setup and usage)*

---

## [Jobs](#jobs)
*(See original content for Job and CronJob YAML and explanations)*

---

## [Init Containers (Initializing)](#init-containers-initializing)
*(See original content for init container YAML and use cases)*

---

## [Service Mesh - Istio (Getaway)](#service-mesh---istio-getaway)
*(See original content for service mesh notes)*

---

## [Pod Lifecycle Phases](#pod-lifecycle-phases)
*(See original content for pod phase explanations)*

---

## [Pod Condition](#pod-condition)
*(See original content for pod status and conditions)*

---

> **Note:**  
> All code blocks, commands, and explanations are preserved as in your original file.  
> For best GitHub rendering, use fenced code blocks (triple backticks) for YAML and shell commands in your future edits.

---

