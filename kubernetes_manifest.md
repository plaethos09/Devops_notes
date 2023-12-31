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

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-08-01%20at%2012.17.08%20AM.png)


**PERSISTANTVOLUMECLAIM IN KUBERNETES**

In Kubernetes, a PersistentVolumeClaim (PVC) is a request for storage by a user or application. It is a way for pods to dynamically request and use PersistentVolumes (PVs) without having to be concerned with the underlying storage details. PVCs allow you to decouple storage requirements from the pod specification, making it easier to manage and scale storage independently.

When a PVC is created, Kubernetes tries to find an available PersistentVolume that satisfies the PVC's storage requirements, access mode, and other specified criteria. Once the PVC is bound to a PV, the pod can use the PVC as a volume to store data.

Here's a detailed explanation of the components in a PersistentVolumeClaim manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a PersistentVolumeClaim, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `PersistentVolumeClaim`.

3. **metadata**: Contains information about the PersistentVolumeClaim, including its name and optional labels and annotations.

4. **spec**: The specification of the PersistentVolumeClaim, which contains the following key components:
   - **accessModes**: The access modes specify how the pod can use the PersistentVolume. Common access modes include `ReadWriteOnce` (can be mounted by a single node as read-write), `ReadOnlyMany` (can be mounted by multiple nodes as read-only), and `ReadWriteMany` (can be mounted by multiple nodes as read-write).
   - **resources**: Specifies the storage requirements for the PersistentVolumeClaim, including the requested storage capacity.

Now, let's create a simple manifest file for a PersistentVolumeClaim that requests 1Gi of storage with ReadWriteOnce access mode:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

In this manifest file:
- We are using the `v1` API version for the PersistentVolumeClaim.
- The `kind` is set to `PersistentVolumeClaim`.
- Under `metadata`, we provide the name of the PersistentVolumeClaim as `example-pvc`.
- In the `spec` section:
  - The `accessModes` field specifies that the PersistentVolumeClaim can be mounted as ReadWriteOnce, meaning it can be mounted by a single node for read-write access.
  - The `resources` section specifies the storage requirements for the PersistentVolumeClaim:
    - We define a request for storage of 1Gi.

To create the PersistentVolumeClaim in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-pvc.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-pvc.yaml
```

The Kubernetes API server will create the PersistentVolumeClaim as specified in the manifest file. Kubernetes will then try to find an available PersistentVolume that satisfies the PVC's requirements. If a suitable PV is found, the PVC will be bound to that PV, and the pod can use the PVC as a volume to store data. If there is no suitable PV available, the PVC will remain in a pending state until an appropriate PV becomes available.

Please note that the PV that matches the PVC's requirements must be provisioned beforehand, either statically by a cluster administrator or dynamically using a StorageClass. Additionally, PVs are specific to a cluster, so a PVC created in one cluster cannot bind to PVs from another cluster.


**NAMESPACE IN KUBERNETES**

In Kubernetes, a Namespace is a virtual cluster within a physical cluster that provides a way to partition and isolate resources. It allows you to create separate logical environments, projects, or teams within the same Kubernetes cluster. Namespaces help avoid naming conflicts, manage access control, and organize resources more effectively.

By default, Kubernetes creates a few system namespaces, such as `default`, `kube-system`, and `kube-public`, but you can create custom namespaces for your applications and services. Resources (like pods, services, deployments, etc.) are associated with a specific namespace, and other resources within the same namespace can access them without specifying the namespace explicitly.

Here's a detailed explanation of the components in a Namespace manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Namespace, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Namespace`.

3. **metadata**: Contains information about the Namespace, including its name and optional labels and annotations.

Now, let's create a simple manifest file for a Namespace:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: example-namespace
```

In this manifest file:
- We are using the `v1` API version for the Namespace.
- The `kind` is set to `Namespace`.
- Under `metadata`, we provide the name of the Namespace as `example-namespace`.

To create the Namespace in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-namespace.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-namespace.yaml
```

The Kubernetes API server will create the Namespace as specified in the manifest file. You can verify the creation of the Namespace using the `kubectl get namespaces` command:

```bash
kubectl get namespaces
```

The output should show the list of available namespaces, including the one you just created (`example-namespace`).

Once the Namespace is created, you can create other Kubernetes resources (pods, services, deployments, etc.) within this Namespace. When creating resources, you can specify the Namespace in the metadata section, and those resources will belong to that Namespace.

For example, to create a simple Nginx deployment within the `example-namespace`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: example-namespace
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
      containers:
        - name: nginx-container
          image: nginx:latest
```

In this example, we added a `namespace: example-namespace` field under the `metadata` section of the Deployment manifest, specifying that this Deployment should be created within the `example-namespace` Namespace.

By using Namespaces, you can organize resources and manage access control more effectively, especially in multi-team or multi-application Kubernetes clusters.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/azure-kubernetes-service-namespaces-2.png)


**INGRESS IN KUBERNETES**


In Kubernetes, an Ingress is an API object that manages external access to services within the cluster. It acts as a reverse proxy, forwarding incoming requests from external clients to the appropriate services based on rules defined in the Ingress resource. In other words, Ingress allows you to define a set of routing rules to map external URLs to internal services, providing a way to access multiple services using a single external IP address.

Ingress resources work with Ingress Controllers, which are components responsible for implementing the actual load balancing and routing based on the Ingress rules. Ingress Controllers can be provided by Kubernetes itself (e.g., Nginx Ingress Controller, Traefik Ingress Controller) or by cloud providers (e.g., GKE Ingress, AWS ALB Ingress).

Here's a detailed explanation of the components in an Ingress manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For an Ingress, it is typically `networking.k8s.io/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Ingress`.

3. **metadata**: Contains information about the Ingress, including its name and optional labels and annotations.

4. **spec**: The specification of the Ingress, which contains the following key components:
   - **rules**: The rules section defines the routing rules for incoming requests. Each rule specifies a host (optional) and a set of paths that are forwarded to a specific backend service.
   - **tls**: The TLS section is optional and is used to configure HTTPS termination for the Ingress, providing a way to secure traffic to the backend services using TLS certificates.

Now, let's create a simple manifest file for an Ingress that routes incoming requests to an Nginx service:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: example-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /app1
            pathType: Prefix
            backend:
              service:
                name: nginx-service1
                port:
                  number: 80
          - path: /app2
            pathType: Prefix
            backend:
              service:
                name: nginx-service2
                port:
                  number: 80
```

In this manifest file:
- We are using the `networking.k8s.io/v1` API version for the Ingress.
- The `kind` is set to `Ingress`.
- Under `metadata`, we provide the name of the Ingress as `example-ingress`.
- In the `spec` section:
  - The `rules` field defines the routing rules for incoming requests:
    - We specify a single rule that listens for requests with the `Host` header set to `example.com`.
    - The `http` section defines the HTTP routing rules:
      - We define two paths: `/app1` and `/app2`, which will be forwarded to the respective backend services.
      - The `pathType` is set to `Prefix`, meaning the paths `/app1` and `/app2` will match URLs that start with `/app1` and `/app2`, respectively.
      - For each path, we define the backend service to which the requests will be forwarded. In this example, `nginx-service1` and `nginx-service2` are two different backend services.

To create the Ingress in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-ingress.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-ingress.yaml
```

The Kubernetes API server will create the Ingress as specified in the manifest file. The Ingress Controller (e.g., Nginx Ingress Controller) will process the Ingress rules and configure the load balancer to route incoming requests to the appropriate backend services based on the defined rules.

Please note that for the Ingress to work, you need to have a running Ingress Controller in your cluster that supports and understands the Ingress resource. The actual implementation of the Ingress rules depends on the specific Ingress Controller you are using.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/ingress-fanout.png)



**HorizontalPodAutoscaler IN KUBERNETES**


In Kubernetes, a HorizontalPodAutoscaler (HPA) is an API object that automatically adjusts the number of replicas of a Deployment, ReplicaSet, or StatefulSet based on observed CPU utilization or custom metrics. The HPA ensures that the desired number of pods are running to handle varying levels of workload, thus enabling automatic scaling of your application.

Here's a detailed explanation of the components in a HorizontalPodAutoscaler manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a HorizontalPodAutoscaler, it is typically `autoscaling/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `HorizontalPodAutoscaler`.

3. **metadata**: Contains information about the HorizontalPodAutoscaler, including its name and optional labels and annotations.

4. **spec**: The specification of the HorizontalPodAutoscaler, which contains the following key components:
   - **scaleTargetRef**: Specifies the target Deployment, ReplicaSet, or StatefulSet that should be scaled by the HPA. You need to provide the `apiVersion`, `kind`, and `name` of the target resource.
   - **minReplicas**: The minimum number of replicas that the HPA should maintain. It is recommended to set this value to at least 1.
   - **maxReplicas**: The maximum number of replicas that the HPA can scale up to in response to increased load.
   - **targetCPUUtilizationPercentage**: The target average CPU utilization across all pods. The HPA will try to maintain this utilization percentage by adjusting the number of replicas.

Now, let's create a simple manifest file for a HorizontalPodAutoscaler that scales a Deployment based on CPU utilization:

```yaml
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: example-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: example-deployment
  minReplicas: 1
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

In this manifest file:
- We are using the `autoscaling/v1` API version for the HorizontalPodAutoscaler.
- The `kind` is set to `HorizontalPodAutoscaler`.
- Under `metadata`, we provide the name of the HorizontalPodAutoscaler as `example-hpa`.
- In the `spec` section:
  - The `scaleTargetRef` field specifies the target Deployment that should be scaled by the HPA. The HPA will scale the Deployment named `example-deployment`.
  - The `minReplicas` is set to `1`, meaning the HPA should always have at least one replica of the Deployment.
  - The `maxReplicas` is set to `10`, indicating that the HPA can scale the Deployment up to 10 replicas in response to increased load.
  - The `targetCPUUtilizationPercentage` is set to `50`, which means the HPA will try to maintain an average CPU utilization of 50% across all pods.

To create the HorizontalPodAutoscaler in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-hpa.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-hpa.yaml
```

The Kubernetes API server will create the HorizontalPodAutoscaler as specified in the manifest file. The HPA will start monitoring the CPU utilization of the target Deployment and automatically adjust the number of replicas to keep the CPU utilization close to the target value.

As the workload increases, the HPA will gradually scale up the number of replicas, and when the load decreases, it will scale down the replicas to match the desired CPU utilization target. Automatic scaling helps ensure that your application can handle varying levels of demand while optimizing resource utilization.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/azure-kubernetes-service-autoscaling-hpa-1.png)

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/azure-kubernetes-service-autoscaling-hpa-2.png)


**SERVICE ACCOUNT IN KUBERNETES**


In Kubernetes, a ServiceAccount is an API object that provides an identity for pods running in the cluster. It allows pods to authenticate with the Kubernetes API server and access various resources, such as Secrets, ConfigMaps, and other services, with the appropriate permissions.

When a pod is created, it is automatically assigned a default ServiceAccount (usually named "default") unless specified otherwise. The ServiceAccount is associated with a set of Kubernetes RBAC (Role-Based Access Control) rules that define the pod's permissions and what resources it can access.

ServiceAccounts can be used to restrict access to sensitive resources and control which pods can interact with the Kubernetes API server or other cluster services.

Here's a detailed explanation of the components in a ServiceAccount manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a ServiceAccount, it is typically `v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `ServiceAccount`.

3. **metadata**: Contains information about the ServiceAccount, including its name and optional labels and annotations.

4. **automountServiceAccountToken**: (Optional) Specifies whether the service account token should be automatically mounted in the pod. The token allows the pod to authenticate with the Kubernetes API server. If set to `false`, the token will not be mounted in the pod, and the pod won't have access to the Kubernetes API.

Now, let's create a simple manifest file for a ServiceAccount:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: example-serviceaccount
automountServiceAccountToken: true
```

In this manifest file:
- We are using the `v1` API version for the ServiceAccount.
- The `kind` is set to `ServiceAccount`.
- Under `metadata`, we provide the name of the ServiceAccount as `example-serviceaccount`.
- We set `automountServiceAccountToken` to `true`, indicating that the service account token should be automatically mounted in the pod.

To create the ServiceAccount in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-serviceaccount.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-serviceaccount.yaml
```

The Kubernetes API server will create the ServiceAccount as specified in the manifest file. Now, you can assign this ServiceAccount to pods by specifying its name in the `spec.serviceAccountName` field of the pod's manifest.

For example, to use the `example-serviceaccount` in a pod:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: example-pod
spec:
  serviceAccountName: example-serviceaccount
  containers:
    - name: example-container
      image: nginx:latest
```

In this example, the `example-pod` pod uses the `example-serviceaccount` ServiceAccount to authenticate with the Kubernetes API server and access resources according to the RBAC rules associated with that ServiceAccount.

Using ServiceAccounts with appropriate RBAC policies ensures that your pods have the necessary permissions to perform their intended tasks while maintaining security and access control within the Kubernetes cluster.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/1%202Q3SrPsBgNjwvE25b0SgYA.png)



**PodDisruptionBudget (PDB) IN KUBERNETES**

In Kubernetes, a PodDisruptionBudget (PDB) is an API object that specifies the minimum number of pods that must be available during voluntary disruptions, such as node maintenance or scaling down a deployment. It is a way to ensure high availability and fault tolerance by preventing too many pods of a specific deployment or replica set from being unavailable at the same time.

PodDisruptionBudgets work in conjunction with the Kubernetes cluster's control plane to safely drain nodes or perform scaling operations without causing excessive downtime or service disruptions.

Here's a detailed explanation of the components in a PodDisruptionBudget manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a PodDisruptionBudget, it is typically `policy/v1beta1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `PodDisruptionBudget`.

3. **metadata**: Contains information about the PodDisruptionBudget, including its name and optional labels and annotations.

4. **spec**: The specification of the PodDisruptionBudget, which contains the following key components:
   - **selector**: Specifies the label selector that identifies the pods targeted by the PodDisruptionBudget. The PDB applies only to pods matching this selector.
   - **minAvailable**: The minimum number of available replicas that should be maintained during a disruption. For example, setting `minAvailable: 2` ensures that at least two replicas of the targeted pods are available at any time.
   - **maxUnavailable**: The maximum number of replicas that can be unavailable during a disruption. Setting this field allows you to control the number of pods that can be temporarily unavailable during a disruption. If both `minAvailable` and `maxUnavailable` are specified, the PDB takes the higher value of the two.

Now, let's create a simple manifest file for a PodDisruptionBudget:

```yaml
apiVersion: policy/v1beta1
kind: PodDisruptionBudget
metadata:
  name: example-pdb
spec:
  selector:
    matchLabels:
      app: example-app
  minAvailable: 2
```

In this manifest file:
- We are using the `policy/v1beta1` API version for the PodDisruptionBudget.
- The `kind` is set to `PodDisruptionBudget`.
- Under `metadata`, we provide the name of the PodDisruptionBudget as `example-pdb`.
- In the `spec` section:
  - The `selector` field specifies the label selector that identifies the pods targeted by the PodDisruptionBudget. In this example, the PDB applies only to pods labeled with `app: example-app`.
  - The `minAvailable` is set to `2`, indicating that at least two replicas of the targeted pods should be available at any time during a disruption.

To create the PodDisruptionBudget in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-pdb.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-pdb.yaml
```

The Kubernetes API server will create the PodDisruptionBudget as specified in the manifest file. The PDB will enforce that during voluntary disruptions, such as scaling down a deployment or draining nodes, at least two replicas of the pods labeled with `app: example-app` are available at all times.

Using PodDisruptionBudgets helps you ensure that your applications remain available and reliable even during maintenance operations or disruptions, reducing the risk of downtime and service outages.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/zyrz9hdtm0rv3omg264n.png)

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/u76e1c968g6wfmmjgee4.png)

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/kjikii2jyfc5z4zncg7y.png)


**ROLES IN KUBERNETES**


In Kubernetes, a Role is an API object that defines a set of permissions (i.e., rules) within a specific namespace. Roles are used in Role-Based Access Control (RBAC) to grant fine-grained access to resources within the cluster. They allow you to control what actions (e.g., create, read, update, delete) users or service accounts can perform on Kubernetes resources in a particular namespace.

Roles are used to grant permissions within a specific namespace, while ClusterRoles are used to grant permissions across the entire cluster.

Here's a detailed explanation of the components in a Role manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a Role, it is typically `rbac.authorization.k8s.io/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `Role`.

3. **metadata**: Contains information about the Role, including its name and optional labels and annotations.

4. **rules**: The rules section defines the permissions granted by the Role. Each rule specifies a set of API groups, resources, verbs, and (optionally) resource names. The combination of these elements determines the level of access that the Role provides.

Now, let's create a simple manifest file for a Role:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: example-role
rules:
  - apiGroups: [""]
    resources: ["pods", "services"]
    verbs: ["get", "list", "watch"]
  - apiGroups: ["apps"]
    resources: ["deployments"]
    verbs: ["get", "list"]
```

In this manifest file:
- We are using the `rbac.authorization.k8s.io/v1` API version for the Role.
- The `kind` is set to `Role`.
- Under `metadata`, we provide the name of the Role as `example-role`.
- In the `rules` section:
  - The first rule grants the `get`, `list`, and `watch` verbs on the `pods` and `services` resources within the empty API group. This means the Role provides read-only access to pods and services in the namespace where it is applied.
  - The second rule grants the `get` and `list` verbs on the `deployments` resource within the `apps` API group. This means the Role provides read-only access to deployments in the namespace where it is applied.

To create the Role in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-role.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-role.yaml
```

The Kubernetes API server will create the Role as specified in the manifest file. The Role is now available to be bound to a user or service account within the specific namespace.

To bind the Role to a user or service account, you'll need to create a RoleBinding or ClusterRoleBinding manifest. A RoleBinding associates a Role with a specific user or service account within a namespace, while a ClusterRoleBinding associates a ClusterRole with a user or service account across the entire cluster.

Please note that to use RBAC, your Kubernetes cluster must have RBAC enabled and you need sufficient permissions to create Roles and RoleBindings. Care should be taken when granting permissions to ensure that users or service accounts have appropriate access levels based on their roles and responsibilities within the cluster.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screen-Shot-2020-08-13-at-11.17.56-AM.png)


**ROLEBINDING IN KUBERNETES**

In Kubernetes, a RoleBinding is an API object that associates a Role (or ClusterRole) with one or more users or service accounts within a specific namespace. It is used in Role-Based Access Control (RBAC) to grant permissions to users or service accounts, allowing them to perform actions on resources in the specified namespace based on the rules defined in the associated Role.

A RoleBinding is typically used when you want to grant permissions at the namespace level and restrict access to specific resources within that namespace.

Here's a detailed explanation of the components in a RoleBinding manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a RoleBinding, it is typically `rbac.authorization.k8s.io/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `RoleBinding`.

3. **metadata**: Contains information about the RoleBinding, including its name and optional labels and annotations.

4. **subjects**: The subjects section specifies the users or service accounts that will be bound to the associated Role. Each subject entry includes the `kind`, `name`, and `namespace` (if applicable) of the user or service account.

5. **roleRef**: The roleRef section identifies the Role or ClusterRole that the RoleBinding is associated with. It includes the `kind` and `name` of the Role or ClusterRole.

Now, let's create a simple manifest file for a RoleBinding that binds a Role named `example-role` to a service account named `example-serviceaccount` within the namespace `example-namespace`:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: example-rolebinding
  namespace: example-namespace
subjects:
  - kind: ServiceAccount
    name: example-serviceaccount
    namespace: example-namespace
roleRef:
  kind: Role
  name: example-role
  apiGroup: rbac.authorization.k8s.io
```

In this manifest file:
- We are using the `rbac.authorization.k8s.io/v1` API version for the RoleBinding.
- The `kind` is set to `RoleBinding`.
- Under `metadata`, we provide the name of the RoleBinding as `example-rolebinding`, and we specify the namespace where the Role and service account are defined.
- In the `subjects` section, we define a single subject that is a service account with the `kind: ServiceAccount`, `name: example-serviceaccount`, and `namespace: example-namespace`. This service account will be granted the permissions defined in the associated Role.
- In the `roleRef` section, we specify the `kind: Role` and `name: example-role`, indicating that the RoleBinding is associated with the Role named `example-role` within the `example-namespace`.

To create the RoleBinding in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-rolebinding.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-rolebinding.yaml
```

The Kubernetes API server will create the RoleBinding as specified in the manifest file. The service account `example-serviceaccount` within the namespace `example-namespace` will now have the permissions defined in the associated `example-role`, allowing it to perform actions on the resources specified in the Role's rules.

![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screen-Shot-2020-08-13-at-10.58.16-AM.png)


**STORAGE CLASS IN KUBERNETES**




In Kubernetes, a StorageClass is an API object that defines the provisioner and parameters for dynamic provisioning of persistent storage volumes. It allows you to abstract the underlying storage details and dynamically allocate storage volumes when PersistentVolumeClaims (PVCs) are requested.

StorageClasses are especially useful in cloud-based environments or when using dynamic storage provisioning systems (e.g., CSI drivers) because they enable you to request storage without having to pre-provision it manually.

Here's a detailed explanation of the components in a StorageClass manifest:

1. **apiVersion**: The version of the Kubernetes API that the manifest is written for. For a StorageClass, it is typically `storage.k8s.io/v1`.

2. **kind**: Specifies the type of resource being defined, which, in this case, is `StorageClass`.

3. **metadata**: Contains information about the StorageClass, including its name and optional labels and annotations.

4. **provisioner**: The provisioner field specifies the name of the external provisioner that the StorageClass will use to create persistent volumes.

5. **parameters**: The parameters section includes key-value pairs that are specific to the provisioner. These parameters allow you to customize the behavior of the storage provisioning process.

Now, let's create a simple manifest file for a StorageClass that uses the provisioner `example-provisioner` and includes a parameter `type=fast`:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: example-storageclass
provisioner: example-provisioner
parameters:
  type: fast
```

In this manifest file:
- We are using the `storage.k8s.io/v1` API version for the StorageClass.
- The `kind` is set to `StorageClass`.
- Under `metadata`, we provide the name of the StorageClass as `example-storageclass`.
- In the `provisioner` field, we specify `example-provisioner`, which is the name of the external provisioner that will handle the storage provisioning.
- In the `parameters` section, we provide a custom parameter `type: fast`, which is specific to the `example-provisioner` provisioner. This parameter can be used by the provisioner to define the type of storage being provisioned (e.g., fast storage).

To create the StorageClass in your Kubernetes cluster using the manifest file, save the above YAML content to a file (e.g., `example-storageclass.yaml`), and then use the `kubectl apply` command:

```bash
kubectl apply -f example-storageclass.yaml
```

The Kubernetes API server will create the StorageClass as specified in the manifest file. Now, when you create a PersistentVolumeClaim, you can request storage from this StorageClass, and the dynamic provisioning system will create a PersistentVolume that meets the defined parameters.

For example, you can use the `storageClassName` field in your PVC manifest to request storage from the `example-storageclass`:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: example-pvc
spec:
  storageClassName: example-storageclass
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

In this example, the PVC requests 1Gi of storage from the `example-storageclass` StorageClass, which uses the `example-provisioner` provisioner with the `type: fast` parameter.

Dynamic provisioning with StorageClasses simplifies storage management in Kubernetes, allowing you to request and use persistent storage without manual intervention or having to pre-provision volumes.


![alt text](https://github.com/plaethos09/Devops_notes/blob/main/img/Screenshot%202023-08-01%20at%2012.17.08%20AM.png)
