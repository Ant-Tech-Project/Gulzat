1. Docker is a platform for building, running and shipping applications
Containers and virtual machines (VMs) are both technologies used for virtualization, but they operate at different levels of the computing stack and have distinct characteristics.

**Virtual Machines (VMs):**

1. **Hypervisor:**
   - VMs run on a hypervisor, which is a layer of software or hardware that emulates the entire computer, including the operating system (OS). Examples of hypervisors include VMware, Hyper-V, and VirtualBox.

2. **Operating System:**
   - Each VM typically has its own full-fledged operating system. This means that VMs can run different OS types on the same host machine.

3. **Resource Overhead:**
   - VMs have more significant resource overhead because they include a full OS and need to emulate hardware. This can result in slower start-up times and higher resource usage.

4. **Isolation:**
   - VMs provide strong isolation between different instances. Since each VM has its own OS, the failure of one VM does not affect others.

5. **Resource Utilization:**
   - VMs are generally less efficient in terms of resource utilization compared to containers because of the overhead of running multiple operating systems.

**Containers:**

1. **Container Engine:**
   - Containers run on a container engine, such as Docker or containerd. The container engine interacts with the host OS's kernel to provide isolation.

2. **Operating System:**
   - Containers share the host OS kernel but have their own user space. They do not require a full OS installation for each instance.

3. **Resource Overhead:**
   - Containers have minimal resource overhead since they do not need to emulate an entire OS. They share the host OS kernel and use resources more efficiently.

4. **Isolation:**
   - Containers provide isolation at the application level. While they share the host OS kernel, the container runtime ensures that processes within a container are isolated from processes in other containers.

5. **Resource Utilization:**
   - Containers are lightweight and start quickly, making them more efficient in terms of resource utilization. They are well-suited for microservices architectures and scalable applications.

**Summary:**

- VMs provide full hardware virtualization, including a complete OS for each instance.
- Containers share the host OS kernel, have minimal resource overhead, and encapsulate the application and its dependencies.
- Containers are generally more lightweight, start faster, and are more resource-efficient compared to VMs.
- VMs offer stronger isolation between instances but at the cost of higher resource usage and slower start-up times.
  
In practice, the choice between containers and VMs depends on specific use cases, performance requirements, and the desired level of isolation. Often, both technologies are used in tandem for different aspects of an application infrastructure.