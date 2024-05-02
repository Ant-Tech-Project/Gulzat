In Docker, a network is a virtualized communication pathway that allows containers to communicate with each other and with other networked entities. Docker provides various types of networks, each serving different purposes and offering distinct functionalities. Here's an overview of the functions of Docker networks:

1. **Isolation**: Docker networks provide isolation between containers, allowing them to run independently without interfering with each other. Containers connected to the same network can communicate with each other, while containers on different networks are isolated from each other by default.

2. **Connectivity**: Docker networks enable containers to communicate with each other using network protocols such as TCP/IP. Containers on the same network can interact with each other using their IP addresses and network ports, just like traditional networked entities.

3. **Routing**: Docker manages routing between containers within the same network, ensuring that network traffic is properly directed to its intended destination. Docker's built-in routing mechanisms handle communication between containers efficiently, without requiring manual configuration.

4. **Scalability**: Docker networks support scalable architectures, allowing containers to be distributed across multiple hosts or nodes while maintaining connectivity. Docker's overlay network, for example, facilitates communication between containers deployed on different hosts, enabling seamless scaling of containerized applications.

5. **Security**: Docker networks provide security features to protect communication between containers and external entities. By default, Docker implements network isolation to prevent unauthorized access between containers on different networks. Additionally, Docker supports network policies and access controls to enforce security rules and restrictions.

6. **Integration**: Docker networks seamlessly integrate with other Docker features and services, such as container orchestration platforms like Docker Swarm and Kubernetes. Docker Swarm, for example, leverages Docker networks to enable communication between containers deployed across a cluster of nodes, facilitating service discovery and load balancing.

Overall, Docker networks play a crucial role in facilitating communication and connectivity between containers, enabling the deployment of distributed and scalable applications within Dockerized environments. Understanding Docker networks and their functions is essential for designing and managing containerized applications effectively.


Network drivers overview
Docker's networking subsystem is pluggable, using drivers. Several drivers exist by default, and provide core networking functionality:

bridge: The default network driver. If you don't specify a driver, this is the type of network you are creating. Bridge networks are commonly used when your application runs in a container that needs to communicate with other containers on the same host. See Bridge network driver.

host: Remove network isolation between the container and the Docker host, and use the host's networking directly. See Host network driver.

overlay: Overlay networks connect multiple Docker daemons together and enable Swarm services and containers to communicate across nodes. This strategy removes the need to do OS-level routing. See Overlay network driver.

ipvlan: IPvlan networks give users total control over both IPv4 and IPv6 addressing. The VLAN driver builds on top of that in giving operators complete control of layer 2 VLAN tagging and even IPvlan L3 routing for users interested in underlay network integration. See IPvlan network driver.

macvlan: Macvlan networks allow you to assign a MAC address to a container, making it appear as a physical device on your network. The Docker daemon routes traffic to containers by their MAC addresses. Using the macvlan driver is sometimes the best choice when dealing with legacy applications that expect to be directly connected to the physical network, rather than routed through the Docker host's network stack. See Macvlan network driver.

none: Completely isolate a container from the host and other containers. none is not available for Swarm services. See None network driver.

Network plugins: You can install and use third-party network plugins with Docker.

Network driver summary
The default bridge network is good for running containers that don't require special networking capabilities.
User-defined bridge networks enable containers on the same Docker host to communicate with each other. A user-defined network typically defines an isolated network for multiple containers belonging to a common project or component.
Host network shares the host's network with the container. When you use this driver, the container's network isn't isolated from the host.
Overlay networks are best when you need containers running on different Docker hosts to communicate, or when multiple applications work together using Swarm services.
Macvlan networks are best when you are migrating from a VM setup or need your containers to look like physical hosts on your network, each with a unique MAC address.
IPvlan is similar to Macvlan, but doesn't assign unique MAC addresses to containers. Consider using IPvlan when there's a restriction on the number of MAC addresses that can be assigned to a network interface or port.
Third-party network plugins allow you to integrate Docker with specialized network stacks.


Overlay, Bridge, and Host Networks are three types of network modes provided by Docker for facilitating communication between containers running on multiple Docker hosts. Each network type serves different purposes and has specific characteristics. Here's an overview of each:

1. **Overlay Network**:
   - **Purpose**: Overlay networks are primarily used for communication between containers across multiple Docker hosts in a swarm mode cluster.
   - **Characteristics**:
     - Allows containers to communicate with each other regardless of which host they are running on.
     - Supports multi-host networking in Docker Swarm mode.
     - Provides automatic service discovery and load balancing.
     - Uses VXLAN (Virtual Extensible LAN) encapsulation to create virtual network overlays over existing physical networks.
   - **Usage**: To create an overlay network, you can use the `docker network create` command with the `--driver overlay` option.

2. **Bridge Network**:
   - **Purpose**: Bridge networks are the default network type for Docker containers and are commonly used for communication between containers on the same Docker host.
   - **Characteristics**:
     - Creates an isolated network bridge on each Docker host where containers are connected.
     - Each container connected to a bridge network gets its own unique IP address within the network.
     - Containers on the same bridge network can communicate with each other directly using their IP addresses.
   - **Usage**: By default, when you create a container without specifying a network, Docker assigns it to the default bridge network.

3. **Host Network**:
   - **Purpose**: Host networks allow containers to share the network namespace with the Docker host, essentially bypassing Docker's network isolation.
   - **Characteristics**:
     - Containers on the host network share the same network interfaces and IP address as the Docker host.
     - Provides the highest level of network performance since there is no additional network encapsulation or overhead.
     - May be suitable for applications that require direct access to host network resources or for optimizing network throughput.
   - **Usage**: To run a container with the host network mode, you can use the `--network host` option when creating the container.

To create and manage networks for container communication across multiple Docker hosts, you can use Docker's networking commands (`docker network create`, `docker network ls`, `docker network inspect`, etc.) or Docker Swarm commands if you're working with a Swarm cluster. By understanding the characteristics and use cases of each network type, you can choose the appropriate network mode for your containerized applications based on your specific requirements.

Service discovery is a crucial aspect of distributed applications, enabling components to locate and communicate with each other dynamically. There are several tools and techniques available for service discovery in distributed systems, including Consul and Docker's built-in DNS. Let's explore each of them:

1. **Consul**:
   - **Purpose**: Consul is a service mesh and service discovery tool developed by HashiCorp. It provides features such as service discovery, health checking, key-value storage, and distributed coordination.
   - **Features**:
     - Service Discovery: Consul maintains a registry of services and their associated locations (IP addresses and ports). Clients can query Consul to discover available services and obtain their network addresses dynamically.
     - Health Checking: Consul performs regular health checks on services to ensure they are available and responsive. It can automatically remove unhealthy services from the registry.
     - Key-Value Storage: Consul includes a distributed key-value store that can be used for configuration management, feature toggles, and other purposes.
     - DNS Interface: Consul provides a DNS interface that allows clients to resolve service names to network addresses. This makes it easy for applications to locate services using standard DNS resolution.
   - **Usage**: Consul can be deployed as a cluster of server nodes for high availability. Clients (such as application instances) interact with Consul's API or DNS interface to discover and communicate with services.

2. **Docker's Built-in DNS**:
   - **Purpose**: Docker includes a built-in DNS server that provides service discovery functionality for containers running on the same Docker network.
   - **Features**:
     - DNS Resolution: Docker's DNS server automatically assigns DNS names to containers based on their container names. Containers can use these DNS names to communicate with each other over the Docker network.
     - Service Discovery: Containers can resolve DNS names to obtain the IP addresses of other containers on the same Docker network. This allows for seamless communication between services without needing to hardcode IP addresses.
     - Automatic Updates: Docker's DNS server automatically updates DNS records as containers are started, stopped, or restarted. This ensures that DNS names remain up-to-date with the current state of the Docker environment.
   - **Usage**: Docker's built-in DNS is enabled by default for containers running on user-defined Docker networks. Applications can use standard DNS resolution mechanisms to discover and communicate with other containers on the same network.

Both Consul and Docker's built-in DNS provide service discovery capabilities for distributed applications, but they differ in their features, complexity, and deployment requirements. Consul offers additional features such as health checking and distributed coordination, making it suitable for more complex service discovery scenarios. Docker's built-in DNS is simpler and more lightweight, making it a convenient option for containerized applications running on Docker Swarm or Kubernetes clusters. The choice between these tools depends on your specific requirements and the complexity of your application architecture.

Certainly! Here are some common use cases where service discovery is essential and examples of how it can be used:

1. **Microservices Architecture**:
   - **Use Case**: In a microservices architecture, where applications are composed of multiple loosely coupled services, service discovery is crucial for enabling communication between services.
   - **Example**: Consider an e-commerce application composed of several microservices such as product catalog, user authentication, payment processing, and order fulfillment. Service discovery allows each microservice to dynamically locate and communicate with other services it depends on, regardless of their locations or instances.

2. **Container Orchestration**:
   - **Use Case**: In container orchestration platforms like Kubernetes or Docker Swarm, where applications are deployed as containerized services across a cluster of nodes, service discovery is necessary for managing the dynamic lifecycle of containers.
   - **Example**: Suppose you have a web application deployed on a Kubernetes cluster, consisting of multiple microservices such as front-end, back-end API, and database services. Kubernetes provides built-in service discovery mechanisms, allowing each component to discover and communicate with other services using DNS names or environment variables.

3. **Service Mesh**:
   - **Use Case**: In a service mesh architecture, where communication between services is managed by a dedicated infrastructure layer, service discovery is essential for routing and load balancing traffic between services.
   - **Example**: Imagine a financial services application that consists of multiple microservices responsible for processing transactions, customer accounts, and fraud detection. A service mesh solution like Istio or Linkerd provides service discovery capabilities along with advanced features such as traffic management, circuit breaking, and observability, enabling fine-grained control and monitoring of service-to-service communication.

4. **High Availability and Load Balancing**:
   - **Use Case**: In high-availability environments where redundant instances of services are deployed across multiple nodes or regions, service discovery helps distribute incoming traffic evenly and route requests to healthy instances.
   - **Example**: Consider a web application deployed on multiple AWS regions for redundancy and disaster recovery purposes. A service discovery tool like Consul or Route 53 can be used to register and discover instances of services across regions, ensuring that requests are routed to the nearest and healthiest instance for optimal performance and availability.

5. **Dynamic Infrastructure Scaling**:
   - **Use Case**: In cloud environments where infrastructure resources are dynamically provisioned and scaled based on demand, service discovery enables newly created instances of services to be discovered and integrated seamlessly into the system.
   - **Example**: Suppose you have a web application deployed on AWS using auto-scaling groups, where instances of services are automatically scaled up or down based on CPU utilization. Service discovery allows newly launched instances to register themselves with a central registry (e.g., Consul, etcd), enabling clients to discover and communicate with them without manual intervention.

In summary, service discovery is essential in various scenarios where applications are distributed, dynamic, and composed of multiple interconnected services. It enables seamless communication between services, facilitates dynamic infrastructure management, and enhances the reliability, scalability, and resilience of distributed systems.


how to expose application 
how to connect two containers in different host

Networking with overlay networks
This series of tutorials deals with networking for swarm services. For networking with standalone containers, see Networking with standalone containers. If you need to learn more about Docker networking in general, see the overview.

This page includes the following tutorials. You can run each of them on Linux, Windows, or a Mac, but for the last one, you need a second Docker host running elsewhere.

Use the default overlay network demonstrates how to use the default overlay network that Docker sets up for you automatically when you initialize or join a swarm. This network is not the best choice for production systems.

Use user-defined overlay networks shows how to create and use your own custom overlay networks, to connect services. This is recommended for services running in production.

Use an overlay network for standalone containers shows how to communicate between standalone containers on different Docker daemons using an overlay network.

Prerequisites
These require you to have at least a single-node swarm, which means that you have started Docker and run docker swarm init on the host. You can run the examples on a multi-node swarm as well.

Use the default overlay network
In this example, you start an alpine service and examine the characteristics of the network from the point of view of the individual service containers.

This tutorial does not go into operation-system-specific details about how overlay networks are implemented, but focuses on how the overlay functions from the point of view of a service.

Prerequisites
This tutorial requires three physical or virtual Docker hosts which can all communicate with one another. This tutorial assumes that the three hosts are running on the same network with no firewall involved.

These hosts will be referred to as manager, worker-1, and worker-2. The manager host will function as both a manager and a worker, which means it can both run service tasks and manage the swarm. worker-1 and worker-2 will function as workers only,

If you don't have three hosts handy, an easy solution is to set up three Ubuntu hosts on a cloud provider such as Amazon EC2, all on the same network with all communications allowed to all hosts on that network (using a mechanism such as EC2 security groups), and then to follow the installation instructions for Docker Engine - Community on Ubuntu.

Walkthrough
Create the swarm
At the end of this procedure, all three Docker hosts will be joined to the swarm and will be connected together using an overlay network called ingress.

On manager. initialize the swarm. If the host only has one network interface, the --advertise-addr flag is optional.


 docker swarm init --advertise-addr=<IP-ADDRESS-OF-MANAGER>
Make a note of the text that is printed, as this contains the token that you will use to join worker-1 and worker-2 to the swarm. It is a good idea to store the token in a password manager.

On worker-1, join the swarm. If the host only has one network interface, the --advertise-addr flag is optional.


 docker swarm join --token TOKEN \
  --advertise-addr <IP-ADDRESS-OF-WORKER-1> \
  <IP-ADDRESS-OF-MANAGER>:2377
On worker-2, join the swarm. If the host only has one network interface, the --advertise-addr flag is optional.


 docker swarm join --token TOKEN \
  --advertise-addr <IP-ADDRESS-OF-WORKER-2> \
  <IP-ADDRESS-OF-MANAGER>:2377
On manager, list all the nodes. This command can only be done from a manager.


 docker node ls

ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
d68ace5iraw6whp7llvgjpu48 *   ip-172-31-34-146    Ready               Active              Leader
nvp5rwavvb8lhdggo8fcf7plg     ip-172-31-35-151    Ready               Active
ouvx2l7qfcxisoyms8mtkgahw     ip-172-31-36-89     Ready               Active
You can also use the --filter flag to filter by role:


 docker node ls --filter role=manager

ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
d68ace5iraw6whp7llvgjpu48 *   ip-172-31-34-146    Ready               Active              Leader

 docker node ls --filter role=worker

ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
nvp5rwavvb8lhdggo8fcf7plg     ip-172-31-35-151    Ready               Active
ouvx2l7qfcxisoyms8mtkgahw     ip-172-31-36-89     Ready               Active
List the Docker networks on manager, worker-1, and worker-2 and notice that each of them now has an overlay network called ingress and a bridge network called docker_gwbridge. Only the listing for manager is shown here:


 docker network ls

NETWORK ID          NAME                DRIVER              SCOPE
495c570066be        bridge              bridge              local
961c6cae9945        docker_gwbridge     bridge              local
ff35ceda3643        host                host                local
trtnl4tqnc3n        ingress             overlay             swarm
c8357deec9cb        none                null                local
The docker_gwbridge connects the ingress network to the Docker host's network interface so that traffic can flow to and from swarm managers and workers. If you create swarm services and do not specify a network, they are connected to the ingress network. It is recommended that you use separate overlay networks for each application or group of applications which will work together. In the next procedure, you will create two overlay networks and connect a service to each of them.

Create the services
On manager, create a new overlay network called nginx-net:


 docker network create -d overlay nginx-net
You don't need to create the overlay network on the other nodes, because it will be automatically created when one of those nodes starts running a service task which requires it.

On manager, create a 5-replica Nginx service connected to nginx-net. The service will publish port 80 to the outside world. All of the service task containers can communicate with each other without opening any ports.

Note

Services can only be created on a manager.


 docker service create \
  --name my-nginx \
  --publish target=80,published=80 \
  --replicas=5 \
  --network nginx-net \
  nginx
The default publish mode of ingress, which is used when you do not specify a mode for the --publish flag, means that if you browse to port 80 on manager, worker-1, or worker-2, you will be connected to port 80 on one of the 5 service tasks, even if no tasks are currently running on the node you browse to. If you want to publish the port using host mode, you can add mode=host to the --publish output. However, you should also use --mode global instead of --replicas=5 in this case, since only one service task can bind a given port on a given node.

Run docker service ls to monitor the progress of service bring-up, which may take a few seconds.

Inspect the nginx-net network on manager, worker-1, and worker-2. Remember that you did not need to create it manually on worker-1 and worker-2 because Docker created it for you. The output will be long, but notice the Containers and Peers sections. Containers lists all service tasks (or standalone containers) connected to the overlay network from that host.

From manager, inspect the service using docker service inspect my-nginx and notice the information about the ports and endpoints used by the service.

Create a new network nginx-net-2, then update the service to use this network instead of nginx-net:


 docker network create -d overlay nginx-net-2

 docker service update \
  --network-add nginx-net-2 \
  --network-rm nginx-net \
  my-nginx
Run docker service ls to verify that the service has been updated and all tasks have been redeployed. Run docker network inspect nginx-net to verify that no containers are connected to it. Run the same command for nginx-net-2 and notice that all the service task containers are connected to it.

Note

Even though overlay networks are automatically created on swarm worker nodes as needed, they are not automatically removed.

Clean up the service and the networks. From manager, run the following commands. The manager will direct the workers to remove the networks automatically.


 docker service rm my-nginx
 docker network rm nginx-net nginx-net-2