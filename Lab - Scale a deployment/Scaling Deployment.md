# Scaling the kubernetes deployment 

A critical feature of Kubernetes that enables users to effectively manage the performance, availability, and resource use of their applications is scaling. Kubernetes provides strong mechanisms for application scaling whether handling changing workloads, traffic patterns, or changing business needs. Let's look at the task below to understand scaling.

![alt text](./images/image-4.png)


## Task: Create and scale a deployment

We will create a deployment named `nginx-deployment` using the `nginx:latest` image setting the replicas to `three` initially. Then we will scale the deployment to `seven replicas` using the `kubectl scale` command. We will also varify the deployment and replicaset and pods.


## Creating a deployment
![alt text](./images/image-2.png)
Use the following command to create a deployment:

```bash
kubectl create deployment nginx-deployment --image=nginx:latest --replicas=3 --port=80
```

It will create a deployment with a replicaset of the specified number of replicas. It uses the nginx image for the containers. we can see the created deployment, replicaset and pods using the following commands:

```bash
kubectl get deployment
kubectl get replicasets
kubectl get pods
```

Here's the expected output:
![alt text](./images/image.png)


## Scale the deployment

![alt text](./images/image-3.png)

Here we can scale the deployment using the scale command:

```bash
kubectl scale deployment nginx-deployment --replicas=7
```

Now, if we see the deployment, replicaset and pods using the following commands:

```bash
kubectl get deployment
kubectl get replicasets
kubectl get pods
```

we will get the following output:
![alt text](./images/image-1.png)

In the output, we can see that the number of pods in this deployment scaled upto seven.




We can again scale down the deployment if we want. Here is a command to scale down the deployment to four replicas.

```bash
kubectl scale deployment nginx-deployment --replicas=4
```

Now, if we check the pods, we will see that the number of pods in this deployment is four.
