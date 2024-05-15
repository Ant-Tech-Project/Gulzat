the container file system is ephemeral. files on the container's file system exist only as long as the container exists. if a container is deleted or re-created in k8s, data stored on the container file system is lost

Volumes:
volumes allow you to store data outside the container file system while allowing the container to access the data at runtime. this can allow data to persist beyond the life of the container

volumes offer a simple way to provide external storage to containers within the pods/conatainer spec.

Persistent volumes are a slightly more advanced form of Volume. they allow you to treat storage as an abstract resource and consume it using your pods.

Volume types:
both volumes and Persistent volumes each have a volume type.  The volume type determines how the storage is actually handled. various volume types support storage methods such as: 
1. NFS
2. Cloud storage mechanisms (AWS, Azure, GCP)
3. ConfigMaps and Secrets
4. a simple directory on the k8s node

regular volumes can be set up relatively easily within a pos/container specification
volumes: in the pod spec, these specify the storage volumes available to the pod. they specify the volume type and other data that determines where and how the data is actually stored. 
volumeMounts: in the container spec, these reference he volumes in the Pod spec and provide a mountPath (the location on the file system where the container process will access the volume data)

Sharing Volumes between containers:
you can use volumesMounts to mount the same volume to multiple containers within the same pod.
this is a powerful way to have multiple containers interact with one another. For example, you could create a secondary sidecar container that processes or transforms output from another container.

Common Volume types:
there are many volume types, but there are two you may want to be especially aware of:
1. hostPath: stores data in a specified directory on the k8s node
2. emptyDir: stores data in a dynamically created location on the node. this directory exists only as long as the pod exist on the node. the directory and the data are deleted when the pod is removed. this volume type is very useful for simply sharing data between containers in the same pod


