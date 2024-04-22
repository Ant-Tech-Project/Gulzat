Current versions of Docker include Swarm mode for natively managing a cluster of Docker Engines called a swarm. Use the Docker CLI to create a swarm, deploy application services to a swarm, and manage swarm behavior.

Docker Swarm mode is built into the Docker Engine.

Docker Swarm is a native clustering and orchestration tool for Docker containers. It allows you to create and manage a cluster of Docker hosts, turning them into a single virtual Docker engine. Swarm enables you to deploy, scale, and manage containerized applications across multiple nodes (machines) in a distributed environment.

Key features of Docker Swarm include:

1. **Cluster Management**: Docker Swarm provides a centralized management interface for deploying and managing containerized applications across a cluster of Docker hosts.

2. **High Availability**: Swarm ensures high availability by distributing containers across multiple nodes and providing automatic failover for containers in case of node failures.

3. **Scalability**: You can easily scale your applications by adding or removing nodes from the Swarm cluster. Swarm automatically distributes containers across nodes to balance the workload.

4. **Service Discovery**: Swarm includes built-in service discovery, allowing containers within the cluster to communicate with each other using service names instead of IP addresses.

5. **Load Balancing**: Swarm provides built-in load balancing for distributing incoming traffic across containers running on different nodes, ensuring optimal performance and resource utilization.

6. **Rolling Updates**: Swarm supports rolling updates, allowing you to update your applications without downtime by gradually replacing old containers with new ones.

7. **Security**: Swarm includes security features such as mutual TLS (Transport Layer Security) authentication between nodes and encryption of network traffic to ensure the confidentiality and integrity of communication within the cluster.

Overall, Docker Swarm simplifies the deployment and management of containerized applications in production environments, providing a scalable, resilient, and easy-to-use platform for running container workloads at scale.

What is Docker Swarm?
Docker Swarm, also known as Swarm cluster, is an orchestration management tool that runs on Docker applications and helps end-users create and deploy a cluster of Docker nodes. It comprises the following components:

The Swarm manager nodes provide the control plane that manages the cluster.
The Worker nodes, which are managed and orchestrated by the Swarm manager nodes to perform tasks.
The Load balancers, which are used to route requests across different services and nodes.
Advantages of Docker Swarm
It is a great container orchestration platform for managing even production workloads. Delineated below are some of its advantages.

It comes built into the Docker Engine, making it easy for the developers to set up and work with when building and deploying their containerized applications.
It provides automatic load balancing within the Docker containers.
It works seamlessly with the Docker tools, Docker CLI and Docker-compose.
Services or tools that work well with Docker can also work with it since it comes integrated with the Swarm API.
Disadvantages of Docker Swarm
Despite its advantages, there are some disadvantages that you need to know. They include:

It is strongly tied to the Docker API, meaning all of the commands used in Docker can be used with it. However, some functionalities, such as volume management, are limited in Swarm. This is because Swarm was designed to be a simple and efficient tool for orchestration and does not include all of the features available in standalone Docker.
Customization options and add-ons are limited, unlike Kubernetes, which comes customizable and allows for specifying Custom Resource Definitions (CRDs).
Its capabilities are less robust and it doesnt have a large community when compared with Kubernetes.
What is Kubernetes?
Initially created by Google, Kubernetes is an open source container orchestration system for automating the deployment, management, and scaling of containerized applications with the flexibility of clusters. It is commonly referred to as K8s, which originates from counting the eight letters between the “K” and the “s”.

It is made up of the control plane, which manages all resources in the cluster and assigns tasks to worker nodes that execute these tasks and run your containerized workloads.

Advantages of Kubernetes
It offers many benefits to teams looking for a robust container orchestration platform. This includes:

It is backed by Google and the vendor-neutral Cloud Native Computing Foundation (CNCF).
It offers a wide range of key functionalities, including service discovery, Ingress, load balancing, horizontal and vertical scalability, and so on.
It can manage large and complex workloads.
It is available ”as-a-Service” in public cloud platforms like Google, Microsoft, and AWS.
It has a large community of contributors regularly updating the code base and documentation.
Disadvantages of Kubernetes
While it offers many significant advantages, there are also some disadvantages to consider before using it. Some of them are:

It has a steep learning curve and specialized knowledge in managing the nodes and the different functionalities such as Pods, Namespaces, ConfigMap, etc.
It is too complex for managing simplistic workloads and applications, such as deploying a simple stateless website or as a documentation site.
The shift or migration from another orchestration platform to K8s can be quite a daunting task, as each configuration will have to be translated to a K8s object.
It can be challenging to utilize without proper knowledge and specialization. This lack of knowledge has caused many pending issues and a pile-up in backlogs.
Kubernetes vs Docker Swarm: Similarities & Differences
Both are container orchestration tools, which means they may have many similar features. However, each of these similarities tends to succeed in different categories. This section will cover the similarities and differences between both platforms so you can decide on the one that best suits you.

Installation and setup
Docker swarm: This is relatively easy to install and set up. For instance, a simple one-liner command can install the Docker engine across different Linux operating systems.
Kubernetes: It requires a series of manual installations and configurations to get it up and running, such as installing , a Container Runtime, and other components such as the Kubelet.
Deployment
Docker Swarm: Applications are deployed as services. These services are written as YAML files and are used to manage the application deployed.
Kubernetes: It does provide a plethora of options for configuring and deploying applications, such as Namespaces, Deployments, and StatefulSets.
Autoscaling
Docker Swarm: there is no autoscaling option available. Scripts may have to be written to achieve autoscaling.
Kubernetes: This offers autoscaling options, such as Horizontal Pod Autoscaling, to scale workloads based on triggers such as CPU utilization.
Storage
Docker Swarm: Persisting data in Docker is limited to three options, which are; bind mounts, volumes, and tmpfs mount for Linux systems.
Kubernetes: It supports storage mounts of your choice, either from local storage or as supported by your cloud provider.
Security
Docker Swarm: This relies on transport layer security (TLS) and RBAC to carry out access control and security related tasks. However, the RBAC doesn’t come free. You need to purchase the Docker Enterprise Edition to get this feature.
Kubernetes: It supports more security options, such as role-based access control (RBAC), transport layer security (TLS), third-party authentication, and audit logging.
Load balancing
Docker Swarm: This uses an internal DNS system to assign DNS entries to each Swarm service automatically. In addition, the Swarm manager uses internal load balancing to distribute incoming requests to the different services based on their DNS entries.
Kubernetes: Pods in K8s are exposed via Services which are then used for internal load balancing within the Cluster. The Service configurations can also be set to perform external load balancing. Generally, Ingress controllers such as the Edge Stack are best used for load balancing and provide a network security layer to connections made to your workloads.
Monitoring and Logging
Docker Swarm: While it has built-in server logs and events tools, they may not always be enough for effective monitoring. To get the most out of monitoring your workloads, third-party tools are needed.
Kubernetes: This comes built-in with a dashboard that provides monitoring and logging solutions right out of the box. It also supports integration with other third-party monitoring tools.
Self-healing
Docker Swarm: It comes with native health checks that can be used to determine the availability of services.
Kubernetes: With the self-healing feature, failed or faulty containers are restarted or replaced with new ones if required.
Kubernetes vs Docker Swarm: When to Use?
Both are great options for container orchestration. They are both market leaders in container management, and it can be pretty tough to decide which is good and which is not.

If your workloads are small, relatively simplistic, and easy to manage, Docker swarm could be the right choice for a start.

If your workloads are complex and require features such as service discovery, self-healing, and monitoring, Kubernetes is the right choice for you as it comes with built-in features.

Ultimately, the platform you choose depends on your individual needs, whether personal or business needs.

we have to have 3 or more host machines, it could be virtual machines, install docker for each of them, docker swarm is default in docker engine. 

docker swarm init --advertise-addr 192.168.99.100
Swarm initialized: current node (dxn1zf6l61qsb1josjja83ngz) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join \
    --token SWMTKN-1-49nj1cmql0jkz5s954yi3oex3nedyz0fb0xx14ie39trti4wxv-8vxv8rssmk743ojnwacrr2e7c \
    192.168.99.100:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

The --advertise-addr flag configures the manager node to publish its address as 192.168.99.100. The other nodes in the swarm must be able to access the manager at the IP address.

The output includes the commands to join new nodes to the swarm. Nodes will join as managers or workers depending on the value for the --token flag.


Docker feature:

Deploy to one cloud today and switch to another tomorrow.
You can run multi-cloud.
Can ramp onto a cloud and then ramp off back to on-prem more easily.

Key differences between K8s and Docker Swarm:
1. Kubernetes is well suited for complex applications. On the other hand, Docker Swarm is designed for ease of use, making it a preferable choice for simple applications.
2. Installation and setup
Kubernetes is very customizable but complex to set up. Docker Swarm is easier to install and configure.

Kubernetes: Depending on the operating system, manual installation can differ for each OS. If you are using services from a cloud provider, installation is not required.
Docker Swarm: Docker instances are typically consistent across operating systems and thus fairly simple to set up.

3. Load balancing
Docker Swarm offers automatic load balancing, while Kubernetes does not. However, it is easy to integrate load balancing through third-party tools in Kubernetes.

Kubernetes: Services are made discoverable through a single DNS name. Kubernetes accesses container applications through an IP address or HTTP route.
Swarm: Comes with internal load balancers.

4. Monitoring
Kubernetes: Kubernetes has built-in monitoring along with third-party monitoring tools integration support.
Docker Swarm: In contrast, there are no in-built monitoring mechanisms in Docker Swarm. However, Docker Swarm supports monitoring through third-party applications.

5. Scalability
Kubernetes: Provides scaling based on traffic. Horizontal autoscaling is built in. Scaling in Kubernetes involves creating new pods and scheduling them to nodes with available resources.
Docker Swarm: Offers autoscaling of instances quickly and on-demand. As Docker Swarm deploys containers quicker, it gives the orchestration tool faster reaction times that enable on-demand scaling.

6. When starting, Docker Swarm is an easy-to-use solution to manage your containers at scale. If you or your company does not need to manage complex workloads, then Docker Swarm is the right choice.

If your applications are critical and you are looking to include monitoring, security features, high availability, and flexibility, then Kubernetes is the right choice.