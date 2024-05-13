1. Introduction to High Availability in K8s:
Definition: High availability (HA) in Kubernetes refers to the ability of the cluster to remain operational and accessible even in the event of component failures.
Components: Achieving high availability typically involves redundant configurations for critical components such as the control plane (API server, controller manager, scheduler), etcd, and worker nodes.
Strategies: Strategies for achieving high availability include:
Replication: Running multiple replicas of critical components across different nodes to tolerate failures.
Load Balancing: Using load balancers to distribute traffic across multiple instances of services.
Health Checks and Auto-Recovery: Implementing health checks and auto-recovery mechanisms to detect and recover from failures automatically.

Essentially, high availability for the Kubernetes cluster means having multiple instances of various control plane components. In this setup, multiple control plane nodes house multiple instances of the kube-api-server.

Options for Highly Available Topology
This page explains the two options for configuring the topology of your highly available (HA) Kubernetes clusters.

You can set up an HA cluster:

With stacked control plane nodes, where etcd nodes are colocated with control plane nodes
With external etcd nodes, where etcd runs on separate nodes from the control plane
You should carefully consider the advantages and disadvantages of each topology before setting up an HA cluster.


Stacked etcd topology
A stacked HA cluster is a topology where the distributed data storage cluster provided by etcd is stacked on top of the cluster formed by the nodes managed by kubeadm that run control plane components.

Each control plane node runs an instance of the kube-apiserver, kube-scheduler, and kube-controller-manager. The kube-apiserver is exposed to worker nodes using a load balancer.
Each control plane node creates a local etcd member and this etcd member communicates only with the kube-apiserver of this node. The same applies to the local kube-controller-manager and kube-scheduler instances.
This topology couples the control planes and etcd members on the same nodes. It is simpler to set up than a cluster with external etcd nodes, and simpler to manage for replication.
However, a stacked cluster runs the risk of failed coupling. If one node goes down, both an etcd member and a control plane instance are lost, and redundancy is compromised. You can mitigate this risk by adding more control plane nodes.
You should therefore run a minimum of three stacked control plane nodes for an HA cluster.
This is the default topology in kubeadm. A local etcd member is created automatically on control plane nodes when using kubeadm init and kubeadm join --control-plane.


External etcd topology
An HA cluster with external etcd is a topology where the distributed data storage cluster provided by etcd is external to the cluster formed by the nodes that run control plane components.
Like the stacked etcd topology, each control plane node in an external etcd topology runs an instance of the kube-apiserver, kube-scheduler, and kube-controller-manager. And the kube-apiserver is exposed to worker nodes using a load balancer. However, etcd members run on separate hosts, and each etcd host communicates with the kube-apiserver of each control plane node.
This topology decouples the control plane and etcd member. It therefore provides an HA setup where losing a control plane instance or an etcd member has less impact and does not affect the cluster redundancy as much as the stacked HA topology.
However, this topology requires twice the number of hosts as the stacked HA topology. A minimum of three hosts for control plane nodes and three hosts for etcd nodes are required for an HA cluster with this topology.



There is a variety of management tools available for k8s. These tools interface with k8s to provide addintional functionality. When using k8s it is a good idea to be aware of some of these tools.
Kubernetes management tools are essential for simplifying the deployment, configuration, monitoring, and maintenance of Kubernetes clusters and applications. They automate repetitive tasks, provide standardized interfaces, and offer higher-level abstractions to manage complex infrastructure efficiently. Let's look at each of the mentioned tools:

### 1. kubectl:
- **What**: `kubectl` is the command-line interface (CLI) tool for Kubernetes. It allows users to interact with Kubernetes clusters to deploy applications, inspect and manage cluster resources, and troubleshoot issues.
- **Why**: `kubectl` is essential for performing various tasks, including creating and managing pods, services, deployments, replica sets, and other Kubernetes resources from the command line.
- **Short Information**: `kubectl` provides a wide range of commands and options for interacting with Kubernetes clusters. It's the primary tool used by developers, administrators, and operators to manage Kubernetes environments.

### 2. kubeadm:
- **What**: `kubeadm` is a command-line tool for bootstrapping and managing Kubernetes clusters. It automates the process of setting up the control plane components and joining worker nodes to the cluster.
- **Why**: `kubeadm` simplifies the process of deploying and upgrading Kubernetes clusters, making it accessible to users who need more control over the cluster setup.
- **Short Information**: With `kubeadm`, users can initialize a Kubernetes cluster, join nodes to the cluster, perform upgrades, and perform other cluster management tasks. It's a fundamental tool for cluster administrators.

### 3. minikube:
- **What**: `minikube` is a tool that enables users to run a single-node Kubernetes cluster locally on their development machine. It sets up a lightweight Kubernetes cluster using a virtual machine (e.g., VirtualBox, HyperKit, or Docker) to facilitate application development and testing.
- **Why**: `minikube` provides a simple and convenient way for developers to experiment with Kubernetes features, test applications, and develop locally without needing access to a full-scale Kubernetes cluster.
- **Short Information**: Developers can use `minikube` to quickly spin up a local Kubernetes cluster, deploy and test applications, and validate configurations before deploying to production environments.

### 4. Helm:
- **What**: Helm is a package manager for Kubernetes that simplifies the deployment and management of applications and services. It allows users to define, install, upgrade, and roll back complex Kubernetes applications using pre-defined packages called charts.
- **Why**: Helm streamlines the process of deploying and managing applications on Kubernetes by providing a standardized way to package, version, and distribute applications and their dependencies.
- **Short Information**: With Helm, users can leverage a vast library of community-maintained charts or create custom charts to package and share their own applications and configurations.

### 5. kompose:
- **What**: `kompose` is a tool for converting Docker Compose files into Kubernetes manifests. It allows users to migrate existing applications defined in Docker Compose format to Kubernetes with minimal effort.
- **Why**: `kompose` simplifies the process of migrating applications from Docker Compose to Kubernetes by automatically generating Kubernetes manifests (YAML files) based on the Docker Compose file.
- **Short Information**: Users can use `kompose` to convert Docker Compose files into Kubernetes resources, making it easier to deploy and manage applications on Kubernetes without having to rewrite configurations manually.

### 6. kustomize:
- **What**: `kustomize` is a tool built into `kubectl` that allows users to customize, parameterize, and overlay Kubernetes resource configurations. It provides a way to manage Kubernetes manifests declaratively with a focus on simplicity and flexibility.
- **Why**: `kustomize` simplifies the management of Kubernetes configurations by allowing users to define common configuration patterns, share configurations across environments, and apply overlays to customize configurations for specific use cases.
- **Short Information**: With `kustomize`, users can organize, customize, and manage Kubernetes manifests in a more modular and maintainable way, improving consistency and reducing duplication in configuration management.

Each of these tools serves a specific purpose in managing Kubernetes clusters and applications, offering features and capabilities that cater to different use cases and preferences. Whether you're a developer, administrator, or operator, having familiarity with these tools can greatly enhance your ability to work with Kubernetes effectively.

3. Safely Draining a K8s Node:
Purpose: Draining a Kubernetes node safely ensures that workloads running on the node are gracefully terminated or migrated to other nodes before performing maintenance or decommissioning the node.
Steps:
Cordon the Node: Mark the node as unschedulable to prevent new pods from being scheduled on it.
Drain the Node: Evict all pods from the node by gracefully terminating them or migrating them to other nodes.
Verify: Ensure that all pods have been successfully evacuated from the node and that no workload is running on it.
Perform Maintenance: Perform the required maintenance tasks on the node.
Uncordon the Node: Mark the node as schedulable again to allow new pods to be scheduled on it.

what is draining: when perfoming maintenance, you may sometimes need to remove a k8s node from service. to do this, you can drain the node. containers running on the node will be gracefully terminated (and potentially rescheduled on another node) 
command to do this: kubectl drain <node name> --ignore-daemonsets 
notes: wehn draining a node, you may need to ignore DaemonSets (pods that are tied to each node). it you have any DaemonSet pods running on the node, you will likely need to use th --ignore-daemonsets

if the node remains part of the cluster, you can allow pods to run on the node again when maintenance is complete using the kubectl uncordon command: kubectl uncordon <node name>


Here is information how to upgrate cluster using kubeadm: https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/