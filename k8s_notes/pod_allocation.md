Kubernetes scheduling is the process by which Kubernetes decides where to deploy or run your application's pods within the cluster. The scheduler examines various factors such as resource requirements, node affinity/anti-affinity, node taints/tolerations, and other constraints to determine the best node for each pod.

Here's an overview of how Kubernetes scheduling works and how you can influence or schedule it yourself:

### Kubernetes Scheduling Process:

1. **Pod Creation**: When you create a pod, you define its specifications including resource requirements, labels, and other attributes.

2. **Scheduling Decision**: The Kubernetes scheduler watches for new pods without assigned nodes. It then selects an appropriate node based on various factors and constraints.

3. **Node Selection**: The scheduler considers factors such as available resources (CPU, memory), pod resource requests and limits, node affinity/anti-affinity rules, node taints/tolerations, and any other scheduling preferences or constraints you've specified.

4. **Binding**: Once the scheduler selects a node for the pod, it updates the pod's specification to include the node's name. This process is called binding.

5. **Pod Execution**: Finally, the Kubernetes controller manager creates the pod on the selected node, and the Kubernetes kubelet running on that node ensures that the pod is started and maintained according to its specification.

### How to Influence Kubernetes Scheduling:

While Kubernetes automatically schedules pods based on cluster state and pod specifications, you can influence scheduling decisions using various mechanisms:

1. **Node Affinity and Anti-Affinity**: You can use node affinity and anti-affinity rules to specify preferences or restrictions for pod placement based on node labels.

2. **Node Taints and Tolerations**: Nodes can be "tainted" to repel pods unless the pods have specific "tolerations" defined, allowing you to control which pods can be scheduled on which nodes.

3. **Resource Requests and Limits**: Specify resource requests and limits in your pod specifications to guide scheduling decisions based on CPU and memory requirements.

4. **Pod Priority and Preemption**: You can assign priority to pods to influence scheduling decisions, allowing higher priority pods to be scheduled before lower priority ones. Preemption can evict lower priority pods to make room for higher priority ones.

5. **Custom Schedulers**: You can develop custom schedulers to implement custom scheduling policies tailored to your specific needs.

### Scheduling Pods by Yourself:

To schedule pods manually, you can bypass the Kubernetes scheduler and directly assign nodes to pods by specifying the `nodeName` field in the pod specification. However, this approach is not recommended in most cases because it eliminates the benefits of dynamic scheduling and resource optimization provided by Kubernetes.

Here's an example of how to specify a node for a pod manually:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  nodeName: my-node-name
  containers:
  - name: my-container
    image: my-image:latest
    # Container configuration...
```

In this example, the pod `mypod` will be scheduled on the node with the name `my-node-name`, bypassing the Kubernetes scheduler's normal scheduling process.

It's important to use this approach judiciously and only when absolutely necessary, as it reduces the flexibility and scalability of your Kubernetes deployments. Generally, it's best to rely on Kubernetes' built-in scheduler and leverage its features for optimal pod placement within your cluster.


In Kubernetes, a DaemonSet is a resource type that ensures that a copy of a specific pod runs on all (or a subset of) nodes within the cluster. This makes DaemonSets ideal for running system-level daemons or agents that need to be present on every node.

### DaemonSet Overview:

1. **Pod Distribution**: DaemonSets create one pod per node on which they are running, ensuring that exactly one instance of the pod is running on each node.

2. **Pod Lifecycle**: DaemonSet pods follow the normal pod lifecycle, meaning they are subject to the same scheduling, eviction, and deletion behaviors as other pods.

3. **Automatic Scheduling**: When you create a DaemonSet, Kubernetes automatically schedules one instance of the specified pod on each eligible node in the cluster.

4. **Node Addition/Removal**: DaemonSets automatically add pods to new nodes that are added to the cluster and remove pods from nodes that are removed from the cluster.

### Scheduling with DaemonSets:

DaemonSets automatically handle scheduling themselves to ensure that one instance of the specified pod runs on each eligible node in the cluster. However, you can influence scheduling behavior using node selectors, affinity/anti-affinity rules, and taints/tolerations, just like with other pod types.

Here's how you can influence scheduling behavior with DaemonSets:

1. **Node Selectors**: You can use node selectors to specify which nodes the DaemonSet pods should run on based on node labels.

2. **Node Affinity and Anti-Affinity**: Node affinity and anti-affinity rules allow you to control pod placement based on node labels to ensure that DaemonSet pods are placed on specific nodes or avoid certain nodes.

3. **Taints and Tolerations**: You can use node taints to repel DaemonSet pods from nodes unless they have specific tolerations defined, allowing you to control which nodes DaemonSet pods can be scheduled on.

4. **Pod Priority and Preemption**: If needed, you can assign priority to DaemonSet pods to influence scheduling decisions, allowing higher priority pods to be scheduled before lower priority ones.

### Example DaemonSet YAML:

Here's an example YAML configuration for a DaemonSet:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
spec:
  selector:
    matchLabels:
      app: my-daemon
  template:
    metadata:
      labels:
        app: my-daemon
    spec:
      containers:
      - name: my-daemon-container
        image: my-daemon-image:latest
        # Container configuration...
```

In this example:

- The DaemonSet ensures that one instance of the `my-daemon-container` pod runs on each node in the cluster.
- You can further customize scheduling behavior by adding node selectors, affinity/anti-affinity rules, taints/tolerations, or other scheduling-related fields as needed.

DaemonSets are powerful tools for managing system-level daemons or agents in your Kubernetes cluster, ensuring that they are present and running on every node as required.


Static pods in Kubernetes provide a way to run pods without the need for the Kubernetes API server to manage them. Instead, they are defined and managed directly by the Kubelet on a specific node. This can be useful in certain scenarios where you want to run pods that are not managed by the Kubernetes control plane. Here's why we might need static pods and some common use cases:

### Why Use Static Pods?

1. **Node-Level Services**: Sometimes, you need to run services that are closely tied to a specific node, such as monitoring agents or node-specific daemons. Static pods allow you to run these services without relying on the Kubernetes control plane.

2. **Simplicity and Speed**: Static pods can be created and managed quickly, without the overhead of interacting with the Kubernetes API server. This can be advantageous for simple, standalone applications or for bootstrapping a Kubernetes cluster.

3. **Fault Isolation**: By running pods directly through the Kubelet, you can isolate them from the rest of the cluster, reducing the impact of potential failures or issues in the control plane.

4. **Customization**: Static pods provide more flexibility in terms of customization and node-specific configurations compared to regular pods managed by the Kubernetes control plane.

### Use Cases for Static Pods:

1. **Node Monitoring**: Running monitoring agents or node-specific monitoring components directly as static pods can ensure continuous monitoring and observability of each node in the cluster.

2. **Node-Specific Services**: Some applications or services may need to run on specific nodes due to hardware dependencies or other requirements. Static pods can be used to deploy such node-specific services.

3. **Bootstrapping Cluster Components**: During the bootstrapping process of a Kubernetes cluster, you might want to start essential cluster components (like the control plane components) as static pods before the control plane itself is fully operational.

4. **Custom Add-Ons**: Static pods can be used to deploy custom add-ons, utilities, or debugging tools on individual nodes as needed, without impacting the rest of the cluster.

### How to Use Static Pods:

To create a static pod, you simply create a pod manifest file in a directory watched by the Kubelet on the node (typically `/etc/kubernetes/manifests` or a directory specified by the Kubelet's `--pod-manifest-path` flag). The Kubelet automatically starts the pod defined in the manifest file.
Note: you should restart kubelet

Here's an example of a static pod manifest file:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-static-pod
spec:
  containers:
  - name: my-container
    image: my-image:latest
    # Container configuration...
```

You then place this YAML file in the appropriate directory on the node, and the Kubelet will start the pod automatically.

While static pods provide a straightforward way to run pods directly through the Kubelet, they lack the flexibility and manageability of regular pods managed by the Kubernetes control plane. Therefore, they should be used judiciously and only when necessary for specific use cases.

Observe static pod behavior
When the kubelet starts, it automatically starts all defined static Pods. As you have defined a static Pod and restarted the kubelet, the new static Pod should already be running.

You can view running containers (including static Pods) by running (on the node):

# Run this command on the node where the kubelet is running
crictl ps
The output might be something like:

CONTAINER       IMAGE                                 CREATED           STATE      NAME    ATTEMPT    POD ID
129fd7d382018   docker.io/library/nginx@sha256:...    11 minutes ago    Running    web     0          34533c6729106
Note:crictl outputs the image URI and SHA-256 checksum. NAME will look more like: docker.io/library/nginx@sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31.
You can see the mirror Pod on the API server:

kubectl get pods
NAME                  READY   STATUS    RESTARTS        AGE
static-web-my-node1   1/1     Running   0               2m
Note:Make sure the kubelet has permission to create the mirror Pod in the API server. If not, the creation request is rejected by the API server.
Labels from the static Pod are propagated into the mirror Pod. You can use those labels as normal via selectors, etc.

If you try to use kubectl to delete the mirror Pod from the API server, the kubelet doesn't remove the static Pod:

kubectl delete pod static-web-my-node1
pod "static-web-my-node1" deleted
You can see that the Pod is still running:

kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
static-web-my-node1   1/1     Running   0          4s