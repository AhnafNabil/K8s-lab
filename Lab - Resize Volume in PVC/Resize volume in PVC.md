# Resize volume in a PersistentVolumeClaim




`pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  volumes:
    - name: my-pv
      persistentVolumeClaim:
        claimName: my-pvc
  containers:
    - name: nginx
      image: nginx
      ports:
        - containerPort: 80
          name: "http"
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: my-pv
```



`pvc.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```