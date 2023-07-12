Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF). Here are some basic concepts and components of Kubernetes:

1. **Node**: A node is a physical or virtual machine that runs containerized applications. It is also called a worker or minion in some contexts.

2. **Cluster**: A cluster is a group of nodes that work together to run containerized applications. It includes a control plane and one or more worker nodes.

3. **Control Plane**: The control plane is responsible for managing the cluster. It consists of several components:

   - **API Server**: It exposes the Kubernetes API, which allows users and other components to interact with the cluster.

   - **Scheduler**: The scheduler assigns pods to nodes based on resource availability and constraints.

   - **Controller Manager**: It manages various controllers that handle different aspects of the cluster, such as node and pod lifecycle.

   - **etcd**: It is a distributed key-value store used for storing cluster data, such as configuration settings and cluster state.

4. **Pod**: A pod is the smallest deployable unit in Kubernetes. It represents one or more containers that are scheduled together on the same node and share the same network namespace and storage volumes.

5. **Deployment**: A deployment is a higher-level abstraction that manages the creation and scaling of pods. It ensures that a specified number of replicas of a pod are running at all times.

6. **Service**: A service provides network connectivity to a set of pods. It abstracts the underlying network details and provides a stable DNS name and IP address for accessing the pods.

7. **Namespace**: A namespace provides a way to logically partition a cluster into multiple virtual clusters. It helps in organizing and isolating resources.

8. **Container**: A container is a lightweight and isolated runtime environment that encapsulates an application and its dependencies. Kubernetes uses containerization technologies like Docker to run applications inside containers.

9. **Label**: Labels are key-value pairs attached to Kubernetes objects like pods, services, and deployments. They are used for grouping and selecting objects for operations.

10. **ReplicaSet**: A ReplicaSet ensures that a specified number of replicas (pods) are running at all times. It is an older form of deployment management and has been mostly replaced by the Deployment resource.

These are some of the fundamental concepts of Kubernetes. Kubernetes offers many more features and advanced concepts to manage containerized applications effectively, such as volumes, secrets, ingress, stateful sets, and more.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/kube_arch.png)
