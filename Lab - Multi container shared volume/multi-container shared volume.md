# Multi-container Shared Volume

In Kubernetes, a multi-container shared volume is a mechanism that allows multiple containers within a single pod to share the same storage space. This is achieved through Kubernetes volumes, which can be mounted into multiple containers within a pod. 

![alt text](image-3.png)

The most commonly used volume type for this purpose is `emptyDir`, which provides a temporary storage space that exists as long as the pod is running.

## How it Works
- **Volume Definition:** A volume is defined at the pod level and can be mounted into multiple containers.

- **emptyDir Volume:** An `emptyDir` volume is created when a pod is assigned to a node and remains available until the pod is deleted. 

- **Volume Mounting:** Each container in the pod can mount the volume at a specified path, enabling them to read and write to the same files.



## Task

![alt text](./images/image-5.png)

**Create a Pod:** Create a pod `my-pod` with `3` containers. Each container will perform a specific task using a shared volume named `log-volume`.  All containers will be using `busybox` iamge.

- Container `c1` will echo a message and save it to a file.
- Container `c2` will echo a different message and append it to the same file.
- Container `c3` will read the contents of the file and display it.

**Use Volumes and Volume Mounts:** Ensure that all containers share the same volume mounted at `/var/log/`.

**Prevent CrashLoopBackOff:** Use a `restartPolicy` of `OnFailure` to avoid the pod entering a `CrashLoopBackOff` state after the containers complete their tasks.

## Create the Pod 

1. Create a YAML manifest file `pod-def.yaml` for the pod definition and open it using the `vim editor`:

```bash
vim pod-def.yaml
```

2. Use the following contents to create the pod definition:

```yaml
apiVersion: v1

kind: Pod

metadata:
  name: my-pod

spec:
  volumes:
  - name: log-volume
    emptyDir: {}

  restartPolicy: OnFailure

  containers:
  - name: c1
    image: busybox
    command: ["sh", "-c", 'echo "Hello from c1 container." | tee -a /var/log/newfile']
    volumeMounts:
    - name: log-volume
      mountPath: /var/log

  - name: c2
    image: busybox
    command: ["sh", "-c", 'echo "Hello from c2 container." | tee -a /var/log/newfile']
    volumeMounts:
    - name: log-volume
      mountPath: /var/log

  - name: c3
    image: busybox
    command: ["sh", "-c", 'cat /var/log/newfile']
    volumeMounts:
    - name: log-volume
      mountPath: /var/log

```

Save and exit the editor.

3. Now, create the pod using the following command:

```bash
kubectl create -f pod-def.yaml
```

Expected output:

![alt text](image.png)

## Verification

### Check Pod Status 

Ensure the pod is running without errors using the following command:

```bash
kubectl get pods my-pod
```

Expected output:

![alt text](image-1.png)


### View Logs from Each Container

Check the logs of container c1:

```bash
kubectl logs my-pod -c c1
```
Check the logs of container c2:

```bash
kubectl logs my-pod -c c2
```

Check the logs of container c3:

```bash
kubectl logs my-pod -c c3
```

Expected output:

![alt text](image-2.png)

