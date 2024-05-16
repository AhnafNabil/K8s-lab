# Taints and Tolerations in Kubernetes

Taints and tolerations in Kubernetes are a way to ensure that certain nodes in a cluster only run certain pods.

**Taints**:A taint is a property you can apply to a node, marking it as "tainted". This means that no pod will be scheduled to run on it, unless the pod has a matching toleration.
In Kubernetes, taints can be assigned one of three effects: NoSchedule, PreferNoSchedule, and NoExecute. 

- **NoSchedule**: This is the hardest restriction. It means that no pod will be scheduled on the node unless it has a matching toleration.

- **PreferNoSchedule**: This is a softer version of NoSchedule. The system will try to avoid placing a pod that doesn't have a matching toleration on the node, but it's not a hard requirement.

- **NoExecute**: This means that pods without a matching toleration will be evicted from the node if they are already running. If a pod has a matching toleration, it can continue running.

**Toleration**:A toleration is a property you can apply to a pod, allowing it to be scheduled on nodes with matching taints.

## Task:

 let's consider a scenario where we have a Kubernetes cluster with multiple nodes and we want to ensure that a particular set of pods (let's say, res-intensive pods) only run on a specific node (let's call it node1).

<div style="text-align:center"><img src="./images/Screenshot 2024-05-16 163039.png" width="800"></div>

### Create two pods (pod1 & pod2)with different toleration

Specification for `pod1`(with toleration):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod1
spec:
  containers:
  - name: pod1-container
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "res-intensive"
    effect: "NoSchedule"
```
Specification for `pod2`(without toleration):

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod2
spec:
  containers:
  - name: main
    image: busybox
```

### Applying Taints on node

##### Identifying the `node`to taints:

```
kubectl get nodes
```
this will provide the names of `nodes` in the `cluster`.

Assuming `node1`= desired `<Node-name>`


##### Apply the taint to the `node1`:

```
kubectl taint nodes node1 app=res-intensive:NoSchedule
```

This command applies a taint with the key `app`, value `res-intensive`, and effect `NoSchedule` to node1.

We can also apply taint with a specific effect:

```
kubectl taint nodes node1 key=value:PreferNoSchedule
```
or

```
kubectl taint nodes node1 key=value:NoExecute
```

### Verify the pods 

```
kubectl get pods -o wide
```
This will display more information about each pod, including the `node` each pod is running on
