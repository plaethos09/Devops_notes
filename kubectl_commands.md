As of my knowledge cutoff in September 2021, the `kubectl` commands are continuously evolving and being updated with new Kubernetes releases. However, I can provide you with a list of common `kubectl` commands that were widely used up until that time. Please note that the list might not include any newer or specific commands released after September 2021.

1. Basic commands:
   - `kubectl get`: Display information about resources.
   - `kubectl describe`: Show detailed information about a resource.
   - `kubectl create`: Create a resource from a file or standard input.
   - `kubectl apply`: Apply a configuration to a resource by filename or stdin.
   - `kubectl delete`: Delete resources by filename, stdin, resource name, or label.
   - `kubectl edit`: Edit resources in the default editor.
   - `kubectl logs`: Print the logs for a container in a pod.
   - `kubectl exec`: Execute a command on a container in a pod.
   - `kubectl port-forward`: Forward one or more local ports to a pod.
   - `kubectl proxy`: Run a proxy to the Kubernetes API server.
   - `kubectl version`: Print the client and server version information.

2. Work with resources:
   - `kubectl get`: Display one or many resources.
   - `kubectl describe`: Show details of a specific resource.
   - `kubectl create`: Create a resource from a file or stdin.
   - `kubectl apply`: Apply a configuration to a resource by filename or stdin.
   - `kubectl delete`: Delete resources by filenames, stdin, resources, and names, or by resources and label selectors.

3. Work with nodes:
   - `kubectl get nodes`: Display nodes in the cluster.
   - `kubectl describe node [node-name]`: Show details of a node.
   - `kubectl cordon [node-name]`: Mark a node as unschedulable.
   - `kubectl uncordon [node-name]`: Mark a node as schedulable.
   - `kubectl drain [node-name]`: Drain a node gracefully.
   - `kubectl taint node [node-name] key=value:taint-effect`: Taint a node.

4. Work with pods:
   - `kubectl get pods`: List pods in the cluster.
   - `kubectl describe pod [pod-name]`: Show details of a pod.
   - `kubectl logs [pod-name]`: Print the logs for a pod.
   - `kubectl exec [pod-name] [command]`: Execute a command on a container in a pod.
   - `kubectl port-forward [pod-name] [local-port]:[pod-port]`: Forward one or more local ports to a pod.
   - `kubectl attach [pod-name] -c [container-name]`: Attach to a running container.

5. Work with services:
   - `kubectl get services`: List services in the cluster.
   - `kubectl describe service [service-name]`: Show details of a service.

6. Work with deployments:
   - `kubectl get deployments`: List deployments in the cluster.
   - `kubectl describe deployment [deployment-name]`: Show details of a deployment.
   - `kubectl scale deployment [deployment-name] --replicas=[replica-count]`: Scale a deployment.

7. Work with namespaces:
   - `kubectl get namespaces`: List namespaces in the cluster.
   - `kubectl describe namespace [namespace-name]`: Show details of a namespace.

8. Work with configmaps and secrets:
   - `kubectl get configmaps`: List configmaps in the cluster.
   - `kubectl get secrets`: List secrets in the cluster.
   - `kubectl describe configmap [configmap-name]`: Show details of a configmap.
   - `kubectl describe secret [secret-name]`: Show details of a secret.

This is not an exhaustive list, and `kubectl` supports many other commands and flags for various operations. You can always refer to the official Kubernetes documentation or use `kubectl --help` to get information about the latest available commands and their usage.
