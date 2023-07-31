**PODS IN KUBERNETES**

A Pod is the smallest and simplest deployable unit in Kubernetes. It represents a single instance of a process running in your cluster. A Pod can contain one or more tightly coupled containers that share the same network namespace and can communicate with each other using `localhost`. Pods are typically used to run a single primary container alongside supporting sidecar containers or helper containers.

Here's a detailed explanation of the components in a Pod manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Pod, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Pod`.

3. **metadata**: Contains information about the Pod, including its name and optional labels and annotations.

4. **spec**: The specification of the Pod, which contains the following key components:
   - **containers**: An array of container specifications running inside the Pod. Each container defines an image, command, and optional environment variables, ports, volumes, and other configurations.
   - **volumes**: An array of volumes that can be shared among the containers in the Pod. Volumes persist data beyond the lifecycle of individual containers.
   - **initContainers**: Optional. An array of init containers that run before the main containers in the Pod. Init containers are useful for setup and configuration tasks before the primary containers start.

Now, let's create a simple manifest file for a Pod running an Nginx web server:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  labels:
    app: nginx
spec:
  containers:
    - name: nginx-container
      image: nginx:latest
      ports:
        - containerPort: 80
```

In this manifest file:
- We are using the `v1` API version for the Pod.
- The `kind` is set to `Pod`.
- Under `metadata`, we provide the name of the Pod as `my-nginx-pod`, and we add a label `app: nginx` to the Pod.
- In the `spec` section:
  - We define a single container named `nginx-container` that runs the `nginx:latest` image.
  - The container exposes port 80 using the `containerPort` field.

To create the Pod in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `nginx-pod.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f nginx-pod.yaml
```

The Kubernetes API server will create the Pod as specified in the manifest file. You can check the status of the Pod using `kubectl get pods` and access the Nginx web server at the specified port (in this case, port 80).


![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/module_03_pods.svg)



**DEPLOYMENTS IN KUBERNETES**


In Kubernetes, a Deployment is an object that provides declarative updates for managing ReplicaSets. It is a higher-level abstraction that ensures a specified number of replicas (pods) are running and maintains the desired state of the application. Deployments enable easy scaling, rolling updates, and rollbacks of application changes.

A Deployment uses a ReplicaSet as its template to create and manage multiple replicas of a pod. When you update the Deployment's configuration, such as the container image or the number of replicas, Kubernetes automatically creates a new ReplicaSet with the desired changes and gradually scales it up while scaling down the old ReplicaSet, thus achieving seamless updates.

Here's a detailed explanation of the components in a Deployment manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Deployment, it is typically `apps/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Deployment`.

3. **metadata**: Contains information about the Deployment, including its name and optional labels and annotations.

4. **spec**: The specification of the Deployment, which contains the following key components:
   - **replicas**: The desired number of replicas (pods) to maintain. This determines the number of replicas that will be created during the initial deployment.
   - **selector**: A label selector used to match the pods controlled by this Deployment. The selector is used to identify which pods to manage or update.
   - **template**: The pod template that specifies the desired configuration for the pods controlled by this Deployment. It includes specifications for containers, volumes, environment variables, etc.

Now, let's create a simple manifest file for a Deployment running an Nginx web server with two replicas:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
```

In this manifest file:
- We are using the `apps/v1` API version for the Deployment.
- The `kind` is set to `Deployment`.
- Under `metadata`, we provide the name of the Deployment as `nginx-deployment`.
- In the `spec` section:
  - We specify that we want to have 2 replicas (`replicas: 2`) of the pod managed by this Deployment.
  - The `selector` field defines the label selector used to match the pods controlled by this Deployment. In this case, the selector is `app: nginx`, which matches the label of the pods defined in the `template`.
  - The `template` section contains the pod template specification:
    - We define a single container named `nginx-container` that runs the `nginx:latest` image.
    - The container exposes port 80 using the `containerPort` field.
    - The pod is labeled with `app: nginx`, which matches the selector defined in the `selector` field.

To create the Deployment in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `nginx-deployment.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f nginx-deployment.yaml
```

The Kubernetes API server will create the Deployment as specified in the manifest file, which will, in turn, create and manage two replicas of the Nginx pod. You can check the status of the Deployment and its replicas using `kubectl get deployments` and `kubectl get pods`.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/07751442-deployment.png)

**REPLICASETS IN KUBERNETES**

In Kubernetes, a ReplicaSet is an object that ensures a specified number of replicas (pods) are running at all times. It is a lower-level abstraction than Deployments but serves a similar purpose. ReplicaSets are useful when you need fine-grained control over the number of replicas, rolling updates, or managing multiple versions of the same pod template.

ReplicaSets continuously monitor the number of running pods and automatically adjust the pod count to match the desired state. If a pod fails or gets deleted, the ReplicaSet creates a new pod to replace it, ensuring that the desired number of replicas is always maintained.

Here's a detailed explanation of the components in a ReplicaSet manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a ReplicaSet, it is typically `apps/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `ReplicaSet`.

3. **metadata**: Contains information about the ReplicaSet, including its name and optional labels and annotations.

4. **spec**: The specification of the ReplicaSet, which contains the following key components:
   - **replicas**: The desired number of replicas (pods) to maintain. This determines the number of replicas that the ReplicaSet will create and maintain.
   - **selector**: A label selector used to match the pods controlled by this ReplicaSet. The selector is used to identify which pods the ReplicaSet is responsible for.
   - **template**: The pod template that specifies the desired configuration for the pods controlled by this ReplicaSet. It includes specifications for containers, volumes, environment variables, etc.

Now, let's create a simple manifest file for a ReplicaSet running an Nginx web server with two replicas:

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
```

In this manifest file:
- We are using the `apps/v1` API version for the ReplicaSet.
- The `kind` is set to `ReplicaSet`.
- Under `metadata`, we provide the name of the ReplicaSet as `nginx-replicaset`.
- In the `spec` section:
  - We specify that we want to have 2 replicas (`replicas: 2`) of the pod managed by this ReplicaSet.
  - The `selector` field defines the label selector used to match the pods controlled by this ReplicaSet. In this case, the selector is `app: nginx`, which matches the label of the pods defined in the `template`.
  - The `template` section contains the pod template specification:
    - We define a single container named `nginx-container` that runs the `nginx:latest` image.
    - The container exposes port 80 using the `containerPort` field.
    - The pod is labeled with `app: nginx`, which matches the selector defined in the `selector` field.

To create the ReplicaSet in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `nginx-replicaset.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f nginx-replicaset.yaml
```

The Kubernetes API server will create the ReplicaSet as specified in the manifest file, which will, in turn, create and manage two replicas of the Nginx pod. You can check the status of the ReplicaSet and its replicas using `kubectl get replicasets` and `kubectl get pods`.


**THEORY**

One of the biggest reasons that we don’t deploy naked pods in production is that they are not trustworthy. By this I mean that we can’t count on them to always be running. Kubernetes doesn’t ensure that a pod will continue running if it crashes. A pod could die for all kinds of reasons such as a node that it was running on had failed, it ran out of resources, it was stopped for some reason, etc. If the pod dies, it stays dead until someone fixes it which is not ideal, but with containers we should expect them to be short lived anyway, so let’s plan for it.

Replica Sets are a level above pods that ensures a certain number of pods are always running. A Replica Set allows you to define the number of pods that need to be running at all times and this number could be “1”. If a pod crashes, it will be recreated to get back to the desired state. For this reason, replica sets are preferred over a naked pod because they provide some high availability.
![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-07-31%20at%2011.25.42%20PM.png)

If one of the pods thats is part of a replica set crashes, one will be created to take its place.
![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-07-31%20at%2011.26.06%20PM.png)



**STATEFULSET IN KUBERNETES**

In Kubernetes, a StatefulSet is an object used to manage stateful applications, such as databases, that require unique network identities and stable storage across rescheduling. Unlike ReplicaSets or Deployments, StatefulSets provide guarantees about the ordering and uniqueness of pods, making them suitable for applications that rely on stable network identities and persistent storage.

StatefulSets maintain a unique, stable hostname for each pod, based on the pod's ordinal index. When you scale a StatefulSet or replace a pod, Kubernetes ensures that the new pod retains its identity and any associated persistent storage.

Here's a detailed explanation of the components in a StatefulSet manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a StatefulSet, it is typically `apps/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `StatefulSet`.

3. **metadata**: Contains information about the StatefulSet, including its name and optional labels and annotations.

4. **spec**: The specification of the StatefulSet, which contains the following key components:
   - **replicas**: The desired number of replicas (pods) to maintain. This determines the number of replicas that the StatefulSet will create and maintain.
   - **serviceName**: The name of the headless service that StatefulSet manages. The headless service provides DNS for the StatefulSet's pods, allowing them to have stable, predictable hostnames.
   - **selector**: A label selector used to match the pods controlled by this StatefulSet. The selector is used to identify which pods the StatefulSet is responsible for.
   - **template**: The pod template that specifies the desired configuration for the pods controlled by this StatefulSet. It includes specifications for containers, volumes, environment variables, etc.
   - **volumeClaimTemplates**: An array of PersistentVolumeClaim templates that define the storage requirements for each pod. Each pod gets its own PersistentVolumeClaim based on these templates.

Now, let's create a simple manifest file for a StatefulSet running an example stateful application:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: example-statefulset
spec:
  replicas: 3
  serviceName: example-statefulset-headless
  selector:
    matchLabels:
      app: example-app
  template:
    metadata:
      labels:
        app: example-app
    spec:
      containers:
        - name: example-container
          image: nginx:latest
          ports:
            - containerPort: 80
          volumeMounts:
            - name: data-volume
              mountPath: /data
  volumeClaimTemplates:
    - metadata:
        name: data-volume
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 1Gi
```

In this manifest file:
- We are using the `apps/v1` API version for the StatefulSet.
- The `kind` is set to `StatefulSet`.
- Under `metadata`, we provide the name of the StatefulSet as `example-statefulset`.
- In the `spec` section:
  - We specify that we want to have 3 replicas (`replicas: 3`) of the stateful application.
  - The `serviceName` field is set to `example-statefulset-headless`, which will be the name of the headless service created for DNS resolution.
  - The `selector` field defines the label selector used to match the pods controlled by this StatefulSet. In this case, the selector is `app: example-app`, which matches the label of the pods defined in the `template`.
  - The `template` section contains the pod template specification:
    - We define a single container named `example-container` that runs the `nginx:latest` image.
    - The container exposes port 80 using the `containerPort` field.
    - The pod is labeled with `app: example-app`, which matches the selector defined in the `selector` field.
    - The container mounts a volume named `data-volume` at the path `/data`.
  - The `volumeClaimTemplates` section contains an array of PersistentVolumeClaim templates that define storage requirements for each pod:
    - We define a single PersistentVolumeClaim template named `data-volume`.
    - The PersistentVolumeClaim requests 1Gi of storage and uses the `ReadWriteOnce` access mode, which means it can be mounted as read-write by a single node at a time.

To create the StatefulSet in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-statefulset.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-statefulset.yaml
```

The Kubernetes API server will create the StatefulSet as specified in the manifest file, which will, in turn, create and manage three replicas of the stateful application. Each pod will have its unique, stable hostname based on the pod's ordinal index. You can check the status of the StatefulSet and its pods using `kubectl get statefulsets` and `kubectl get pods`.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/stateful-set-bp-2.png)


**DAEMONSET IN KUBERNETES**


In Kubernetes, a DaemonSet is an object that ensures that a copy of a pod is running on all (or a subset of) nodes in the cluster. Unlike Deployments or ReplicaSets, which ensure a specific number of replicas running across the cluster, DaemonSets focus on running one replica of a pod on each node.

DaemonSets are useful for tasks that need to be performed on all or specific nodes, such as log collection, monitoring agents, or network management components. Whenever a new node is added to the cluster, the DaemonSet ensures that the corresponding pod is scheduled and running on that node. Similarly, if a node is removed from the cluster, the DaemonSet will automatically terminate the associated pod.

Here's a detailed explanation of the components in a DaemonSet manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a DaemonSet, it is typically `apps/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `DaemonSet`.

3. **metadata**: Contains information about the DaemonSet, including its name and optional labels and annotations.

4. **spec**: The specification of the DaemonSet, which contains the following key components:
   - **selector**: A label selector used to match the nodes where the DaemonSet's pods will be scheduled.
   - **template**: The pod template that specifies the desired configuration for the pods managed by the DaemonSet. It includes specifications for containers, volumes, environment variables, etc.

Now, let's create a simple manifest file for a DaemonSet running an example log collector on all nodes:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: log-collector-daemonset
spec:
  selector:
    matchLabels:
      app: log-collector
  template:
    metadata:
      labels:
        app: log-collector
    spec:
      containers:
        - name: log-collector-container
          image: my-log-collector-image:latest
          # Other container configuration such as ports, volumes, etc.
```

In this manifest file:
- We are using the `apps/v1` API version for the DaemonSet.
- The `kind` is set to `DaemonSet`.
- Under `metadata`, we provide the name of the DaemonSet as `log-collector-daemonset`.
- In the `spec` section:
  - The `selector` field defines the label selector used to match the nodes where the DaemonSet's pods will be scheduled. In this case, the selector is `app: log-collector`, which matches the label of the pods defined in the `template`.
  - The `template` section contains the pod template specification:
    - We define a single container named `log-collector-container` that runs the `my-log-collector-image:latest` image.
    - You would typically configure the container with additional settings such as ports, volumes, environment variables, etc., based on the requirements of your log collector.

To create the DaemonSet in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `log-collector-daemonset.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f log-collector-daemonset.yaml
```

The Kubernetes API server will create the DaemonSet as specified in the manifest file, and it will automatically ensure that a pod of the log collector is scheduled and running on all nodes in the cluster. You can check the status of the DaemonSet and its pods using `kubectl get daemonsets` and `kubectl get pods`.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/daemonset-explained.png)


**JOB IN KUBERNETES**

In Kubernetes, a Job is an object used to manage batch jobs or one-off tasks that run to completion. It creates one or more pods to perform a specific task and ensures that the task is successfully completed before terminating the pod(s). Jobs are useful for running tasks that should be executed once, without manual intervention, and where completion is essential.

When you create a Job, Kubernetes creates one or more pods to run the task. If the task is successfully completed (i.e., the pod(s) exit with a success status), the Job terminates. If the task fails (i.e., the pod(s) exit with a failure status), Kubernetes restarts the pod(s) to retry the task until it succeeds or reaches the maximum number of retries.

Here's a detailed explanation of the components in a Job manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Job, it is typically `batch/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Job`.

3. **metadata**: Contains information about the Job, including its name and optional labels and annotations.

4. **spec**: The specification of the Job, which contains the following key components:
   - **template**: The pod template that specifies the desired configuration for the pods managed by the Job. It includes specifications for containers, volumes, environment variables, etc.
   - **completions**: The desired number of successful completions of the task. The Job terminates when this number is reached.
   - **parallelism**: The maximum number of pods that can run in parallel to complete the task.
   - **restartPolicy**: Specifies the restart policy for the pods. For a Job, it is typically set to `OnFailure`, meaning the pod is restarted when it fails.

Now, let's create a simple manifest file for a Job that performs a one-off task:

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: example-job
spec:
  completions: 1
  parallelism: 1
  template:
    spec:
      containers:
        - name: example-container
          image: busybox:latest
          command: ["echo", "Hello, Kubernetes!"]
      restartPolicy: OnFailure
```

In this manifest file:
- We are using the `batch/v1` API version for the Job.
- The `kind` is set to `Job`.
- Under `metadata`, we provide the name of the Job as `example-job`.
- In the `spec` section:
  - We set `completions: 1`, which means the Job is considered successful when one pod completes the task successfully.
  - We set `parallelism: 1`, which specifies that only one pod should run at a time to complete the task.
  - The `template` section contains the pod template specification:
    - We define a single container named `example-container` that runs the `busybox:latest` image.
    - The container executes the command `echo "Hello, Kubernetes!"`.
  - The `restartPolicy` is set to `OnFailure`, which means the pod will be restarted if it fails.

To create the Job in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-job.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-job.yaml
```

The Kubernetes API server will create the Job as specified in the manifest file, and it will create one pod to perform the task. Once the task is successfully completed, the Job will terminate. You can check the status of the Job and its pods using `kubectl get jobs` and `kubectl get pods`.
![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-07-31%20at%2011.46.13%20PM.png)


**SERVICE IN KUBERNETES**

In Kubernetes, a Service is an abstraction that defines a logical set of pods and a policy to access them. It provides a stable endpoint (IP address or DNS name) for connecting to the pods, allowing other components within or outside the cluster to communicate with the pods without knowing their exact IP addresses. Services enable decoupling between the frontend and backend components, making it easier to scale and update the underlying pods without affecting the consumers of the service.

Kubernetes supports several types of services, including ClusterIP, NodePort, LoadBalancer, and ExternalName. Each type serves different purposes and provides different levels of network accessibility.

Here's a detailed explanation of the components in a Service manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Service, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Service`.

3. **metadata**: Contains information about the Service, including its name and optional labels and annotations.

4. **spec**: The specification of the Service, which contains the following key components:
   - **selector**: A label selector used to match the pods that the Service will route traffic to.
   - **type**: The type of Service. It can be `ClusterIP`, `NodePort`, `LoadBalancer`, or `ExternalName`.
   - **ports**: The ports configuration for the Service, which defines the ports on which the Service listens and forwards traffic to the pods.

Now, let's create a simple manifest file for a Service that exposes an Nginx web server:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  type: ClusterIP
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
```

In this manifest file:
- We are using the `v1` API version for the Service.
- The `kind` is set to `Service`.
- Under `metadata`, we provide the name of the Service as `nginx-service`.
- In the `spec` section:
  - The `selector` field defines the label selector used to match the pods to which the Service will route traffic. In this case, the selector is `app: nginx`, which means the Service will forward traffic to pods labeled with `app: nginx`.
  - The `type` is set to `ClusterIP`, which means the Service is only accessible within the cluster and is assigned an internal IP address.
  - The `ports` section defines the ports configuration for the Service:
    - We define a single port named `http` that listens on TCP port 80 and forwards traffic to the target port 80 of the pods.

To create the Service in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `nginx-service.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f nginx-service.yaml
```

The Kubernetes API server will create the Service as specified in the manifest file, providing a stable internal IP address to access the Nginx pods. You can check the status of the Service using `kubectl get services`.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-07-31%20at%2011.52.10%20PM.png)

**NODE PORT IN THE KUBERNETES**

In Kubernetes, a NodePort is a type of Service that exposes a specific port on each node in the cluster. It allows external traffic to reach services running inside the cluster by forwarding traffic from a port on the nodes to the Service's port. NodePort Services are typically used to make services accessible from outside the cluster, enabling external clients to reach applications running on the cluster's nodes.

Here's a detailed explanation of the components in a NodePort Service manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Service, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Service`.

3. **metadata**: Contains information about the Service, including its name and optional labels and annotations.

4. **spec**: The specification of the Service, which contains the following key components:
   - **selector**: A label selector used to match the pods that the Service will route traffic to.
   - **type**: The type of Service. For a NodePort Service, it should be set to `NodePort`.
   - **ports**: The ports configuration for the Service, which defines the ports on which the Service listens and forwards traffic to the pods. In a NodePort Service, one of the ports should have `nodePort` defined to specify the port on the nodes where the Service will be exposed.

Now, let's create a simple manifest file for a NodePort Service that exposes an Nginx web server:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-nodeport-service
spec:
  selector:
    app: nginx
  type: NodePort
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30080
```

In this manifest file:
- We are using the `v1` API version for the Service.
- The `kind` is set to `Service`.
- Under `metadata`, we provide the name of the Service as `nginx-nodeport-service`.
- In the `spec` section:
  - The `selector` field defines the label selector used to match the pods to which the Service will route traffic. In this case, the selector is `app: nginx`, which means the Service will forward traffic to pods labeled with `app: nginx`.
  - The `type` is set to `NodePort`, which makes it a NodePort Service.
  - The `ports` section defines the ports configuration for the Service:
    - We define a single port named `http` that listens on TCP port 80 and forwards traffic to the target port 80 of the pods.
    - The `nodePort` is set to `30080`, which means the Service will be exposed on port 30080 on each node.

To create the NodePort Service in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `nginx-nodeport-service.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f nginx-nodeport-service.yaml
```

The Kubernetes API server will create the NodePort Service as specified in the manifest file, exposing the Nginx pods on port 30080 on each node in the cluster. You can check the status of the Service using `kubectl get services`. External clients can access the Nginx web server by reaching any node's IP address on port 30080.



**EXTERNAL NAME SERVICE IN KUBERNETES**



In Kubernetes, an ExternalName Service is a type of Service that provides a DNS-based mapping to an external service, outside of the cluster. It does not expose any ports on the cluster's nodes or provide load balancing. Instead, it serves as a simple alias to an external resource, allowing applications inside the cluster to access an external service by its DNS name.

ExternalName Services are particularly useful when you want to access an external service by name without the need for complex configurations within the cluster. For example, you can use an ExternalName Service to map an internal DNS name to an external database service hosted outside the Kubernetes cluster.

Here's a detailed explanation of the components in an ExternalName Service manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Service, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Service`.

3. **metadata**: Contains information about the Service, including its name and optional labels and annotations.

4. **spec**: The specification of the Service, which contains the following key components:
   - **type**: The type of Service. For an ExternalName Service, it should be set to `ExternalName`.
   - **externalName**: The DNS name of the external service to be accessed by applications within the cluster.

Now, let's create a simple manifest file for an ExternalName Service that maps an internal DNS name to an external service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: external-db-service
spec:
  type: ExternalName
  externalName: external-db.example.com
```

In this manifest file:
- We are using the `v1` API version for the Service.
- The `kind` is set to `Service`.
- Under `metadata`, we provide the name of the Service as `external-db-service`.
- In the `spec` section:
  - The `type` is set to `ExternalName`, making it an ExternalName Service.
  - The `externalName` is set to `external-db.example.com`, which is the DNS name of the external service to be accessed.

To create the ExternalName Service in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `external-db-service.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f external-db-service.yaml
```

The Kubernetes API server will create the ExternalName Service as specified in the manifest file. Now, applications inside the cluster can access the external service `external-db.example.com` by using the `external-db-service` DNS name.

Please note that an ExternalName Service does not provide load balancing or any other features that are typical of other Service types. It is simply a DNS alias to the specified external resource.

**LOADBALANCER IN KUBERNETES**

In Kubernetes, a LoadBalancer Service is a type of Service that exposes pods to the external world and automatically provisions an external load balancer. The external load balancer allows external clients to access the pods running in the cluster. LoadBalancer Services are commonly used when you want to expose a service externally and distribute traffic among multiple pods in a cluster.

Please note that the availability of an external load balancer depends on the underlying infrastructure and the cloud provider you are using. Not all Kubernetes setups support LoadBalancer Services out of the box. In some cases, you may need to configure external load balancers separately.

Here's a detailed explanation of the components in a LoadBalancer Service manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Service, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Service`.

3. **metadata**: Contains information about the Service, including its name and optional labels and annotations.

4. **spec**: The specification of the Service, which contains the following key components:
   - **type**: The type of Service. For a LoadBalancer Service, it should be set to `LoadBalancer`.
   - **selector**: A label selector used to match the pods that the Service will route traffic to.
   - **ports**: The ports configuration for the Service, which defines the ports on which the Service listens and forwards traffic to the pods.

Now, let's create a simple manifest file for a LoadBalancer Service that exposes an Nginx web server:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
```

In this manifest file:
- We are using the `v1` API version for the Service.
- The `kind` is set to `Service`.
- Under `metadata`, we provide the name of the Service as `nginx-loadbalancer-service`.
- In the `spec` section:
  - The `type` is set to `LoadBalancer`, making it a LoadBalancer Service.
  - The `selector` field defines the label selector used to match the pods to which the Service will route traffic. In this case, the selector is `app: nginx`, which means the Service will forward traffic to pods labeled with `app: nginx`.
  - The `ports` section defines the ports configuration for the Service:
    - We define a single port named `http` that listens on TCP port 80 and forwards traffic to the target port 80 of the pods.

To create the LoadBalancer Service in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `nginx-loadbalancer-service.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f nginx-loadbalancer-service.yaml
```

The Kubernetes API server will create the LoadBalancer Service as specified in the manifest file. Depending on your infrastructure and cloud provider, an external load balancer will be automatically provisioned and direct traffic to the Nginx pods. You can check the status of the Service using `kubectl get services`. External clients can access the Nginx web server through the external IP address provided by the load balancer.



**CONFIGMAP IN KUBERNETES**



In Kubernetes, a ConfigMap is an API object used to store configuration data separately from the application's container image. It allows you to decouple configuration details from your application code, making it easier to manage and update configurations without modifying the application itself. ConfigMaps are typically used to store environment variables, command-line arguments, configuration files, or any other settings that your application needs.

ConfigMaps can be used as volumes to inject configuration data into pods at runtime, or they can be used as environment variables or command-line arguments within containers.

Here's a detailed explanation of the components in a ConfigMap manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a ConfigMap, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `ConfigMap`.

3. **metadata**: Contains information about the ConfigMap, including its name and optional labels and annotations.

4. **data**: The data section contains key-value pairs that represent the configuration data to be stored in the ConfigMap.

Now, let's create a simple manifest file for a ConfigMap that stores some application configuration:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: example-configmap
data:
  APP_ENV: production
  MAX_CONNECTIONS: "1000"
  LOG_LEVEL: "info"
```

In this manifest file:
- We are using the `v1` API version for the ConfigMap.
- The `kind` is set to `ConfigMap`.
- Under `metadata`, we provide the name of the ConfigMap as `example-configmap`.
- In the `data` section:
  - We define several key-value pairs to represent the application configuration data.
  - For example, `APP_ENV: production` sets the `APP_ENV` environment variable to `production`.
  - Similarly, `MAX_CONNECTIONS: "1000"` and `LOG_LEVEL: "info"` set the `MAX_CONNECTIONS` and `LOG_LEVEL` environment variables to integer and string values, respectively.

To create the ConfigMap in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-configmap.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-configmap.yaml
```

The Kubernetes API server will create the ConfigMap as specified in the manifest file. Once the ConfigMap is created, you can use it to inject configuration data into your pods, either as environment variables or as volumes.

For example, to use the ConfigMap as environment variables in a pod, you can define the `envFrom` field in the pod's manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  containers:
    - name: example-container
      image: my-app-image:latest
      envFrom:
        - configMapRef:
            name: example-configmap
```

In this example, the `example-pod` pod uses the `example-configmap` ConfigMap's data as environment variables in the `example-container` container.

To use the ConfigMap as a volume in a pod, you can define the `volumes` field in the pod's manifest:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  volumes:
    - name: config-volume
      configMap:
        name: example-configmap
  containers:
    - name: example-container
      image: my-app-image:latest
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
```

In this example, the `example-pod` pod mounts the `example-configmap` ConfigMap's data as a volume at the path `/etc/config` inside the `example-container` container.

By using ConfigMaps, you can easily manage and update your application's configurations without modifying the application itself, providing greater flexibility and separation of concerns.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-08-01%20at%2012.10.06%20AM.png)


**PERSISTANT VOLUME IN KUBERNETES**


In Kubernetes, a PersistentVolume (PV) is a storage resource in the cluster that has a lifecycle independent of any individual pod. PVs provide a way to decouple storage from pods, allowing the data to persist even if the pod that uses it is deleted or rescheduled to a different node. PVs are used to provide storage to applications that require durable data storage, such as databases or file systems.

PersistentVolumes are provisioned by cluster administrators and are then available for consumption by users or applications via PersistentVolumeClaims (PVCs). PVCs act as a request for storage with specific requirements, and they are bound to available PVs that meet the requested criteria.

Here's a detailed explanation of the components in a PersistentVolume manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a PersistentVolume, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `PersistentVolume`.

3. **metadata**: Contains information about the PersistentVolume, including its name and optional labels and annotations.

4. **spec**: The specification of the PersistentVolume, which contains the following key components:
   - **capacity**: The size of the storage that the PersistentVolume offers.
   - **accessModes**: The access modes specify how the volume can be mounted by pods. Common access modes include `ReadWriteOnce` (can be mounted by a single node as read-write), `ReadOnlyMany` (can be mounted by multiple nodes as read-only), and `ReadWriteMany` (can be mounted by multiple nodes as read-write).
   - **persistentVolumeReclaimPolicy**: Defines what happens to the PersistentVolume when it is released by the user. Common policies include `Retain` (the volume is retained and must be manually reclaimed), `Recycle` (data is removed, and the volume can be reused), and `Delete` (the underlying storage asset is deleted).
   - **storageClassName**: A way to dynamically provision PersistentVolumes using StorageClasses. This field is optional and is used to match the PVC's `storageClassName`.
   - **volumeMode**: Specifies whether the PersistentVolume is formatted with a file system (`Filesystem`) or it's raw block storage (`Block`).
   - **persistentVolumeSource**: Specifies the source of the volume. It can be either an `awsElasticBlockStore` (for AWS EBS volumes), `azureDisk` (for Azure Disk volumes), `csi` (for volumes provisioned by a CSI driver), `hostPath` (for using the local node's filesystem), or other volume types supported by Kubernetes.

Now, let's create a simple manifest file for a PersistentVolume using `hostPath` as the volume source:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: example-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: ""
  volumeMode: Filesystem
  hostPath:
    path: /data/persistent_volume
```

In this manifest file:
- We are using the `v1` API version for the PersistentVolume.
- The `kind` is set to `PersistentVolume`.
- Under `metadata`, we provide the name of the PersistentVolume as `example-pv`.
- In the `spec` section:
  - The `capacity` field specifies that the volume has a capacity of 1Gi.
  - The `accessModes` field specifies that the volume can be mounted as ReadWriteOnce, meaning it can be mounted by a single node for read-write access.
  - The `persistentVolumeReclaimPolicy` is set to `Retain`, which means the volume will be retained after it is released by the user and must be manually reclaimed.
  - The `storageClassName` is set to an empty string, indicating that no StorageClass is used for dynamic provisioning.
  - The `volumeMode` is set to `Filesystem`, indicating that the volume is formatted with a file system.
  - The `hostPath` field specifies that the volume source is a directory on the local node's filesystem located at `/data/persistent_volume`.

To create the PersistentVolume in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-pv.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-pv.yaml
```

The Kubernetes API server will create the PersistentVolume as specified in the manifest file. This PersistentVolume is now available for consumption by creating PersistentVolumeClaims (PVCs) that request storage with specific access modes and capacity requirements.
