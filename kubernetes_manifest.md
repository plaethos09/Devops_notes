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
