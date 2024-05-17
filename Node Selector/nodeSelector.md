# NodeSelector in Kubernetes for Pod Scheduling

`nodeSelector` is a feature in the PodSpec that enables the assignment of a Pod to specific nodes in the cluster. It operates by defining a set of key-value pairs, where each pair corresponds to a label on a node. A Pod can only be scheduled on a node if the node's labels match all the key-value pairs specified in the nodeSelector. This mechanism ensures that Pods are placed on nodes that meet their specific requirements.

## Task

The task is to schedule a pod that requires high processing power on a node that has high processing capabilities.

##### Identifying the `node`to add labels:

```
kubectl get nodes
```
this will provide the names of `nodes` in the `cluster`.

Assuming `node1`= desired `<Node-name>`