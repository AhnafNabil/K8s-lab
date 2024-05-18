# Mounting a secret as a volume

## Task

In this lab, our task is to create a new Secret named `basic-auth` of type kubernetes.io/basic-auth. Assign the key-value pairs 

- `username=super`
- `password=my-s8cr3t`

Mount the Secret as a volume with the path `/etc/secret` and `read-only` permissions to the Pods controlled by the Deployment.

![](./images/overview-scrt-vol.png)

# Steps

Here is the Overview of our task, mounting secret as a volume.

![](./images/overview2.png)


## 1. Create a secret

### Create a Secret from Literal Values
We can create a secret using kubectl by providing literal key-value pairs.

```bash
kubectl create secret generic basic-auth --from-literal=username=super --from-literal=password=my-s8cr3t
```

This command creates a secret named `basic-auth` with two keys: `username` and `password`.

### Create a Secret Using a YAML Manifest

First we have to encode the secret value into base64. We can use `base64` command to encode a string or file content.

```bash
echo -n 'super' | base64
```

```bash
echo -n 'my-s8cr3t' | base64
```

Now we wil define a yaml definition file of name `secret.yaml`.

```YAML
apiVersion: v1
kind: Secret
metadata:
  name: basic-auth
type: kubernetes.io/basic-auth
data:
  username: c3VwZXI=
  password: bXktczhjcjN0
```

```bash
kubectl apply -f secret.yaml
```

![alt text](./images/secret-yaml.png)

## 2. Create the deployment

For creating the deployment, we can define a yaml definition file  `nginx-deploy-secret.yaml`

```YAML
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
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
      - name: nginx
        image: nginx:1.17.0
        volumeMounts:
        - name: secret-volume
          mountPath: /etc/secret
          readOnly: true
      volumes:
      - name: secret-volume
        secret:
          secretName: basic-auth
```

```bash
kubectl apply -f nginx-deploy-secret.yaml
```

![alt text](./images/nginx-deploy-secret.png)

## 3. Verify the Deployment and Secret

Now we have to verify the deployment and the secret if they are created properly.

```bash
kubectl get all
```
![alt text](./images/verify-all-created.png)

```bash
kubectl get secret
```

![alt text](./images/view-secret.png)

## 4. View secret details

```bash
kubectl get secret basic-auth -o yaml
```
![alt text](./images/view-secret-wide.png)

## 5. Verify Mount

To verify that the Secret is correctly mounted as a volume, we can exec into one of the Pods:

```bash
kubectl exec -it <pod_name> -- /bin/bash
```
Here, replace the `pod_name` with your pods name.

```bash
ls -1 /etc/secret
```

```bash
cat /etc/secret/username
cat /etc/secret/password
```

![alt text](./images/verfiy-mount-final.png)

So, we have successfully mounted a secret as a volume with the specific path and the given requirements.




