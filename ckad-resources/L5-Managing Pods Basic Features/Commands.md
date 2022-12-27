# Resources Generic

<mark>Generate a yaml file</mark>
```dockerfile
kubectl run ngx --image=nginx:alpine --dry-run=client -o yaml > my.yaml
```
> **dry run** as an argument of <u>kubectl run</u>  and  <u>kubectl create</u> 


<mark>Create resources from a yaml file</mark>
```dockerfile
kubectl create -f my.yaml
```

<mark>Create/Update resources from a yaml file</mark>
```dockerfile
kubectl apply -f my.yaml
```

<mark>Will replace a resources from a yaml as it is</mark>
```dockerfile
kubectl replace -f my.yaml
```

<mark>delete everything specified in a yaml file</mark>
```dockerfile
kubectl delete -f my.yaml
```

<mark>run commands in a container</mark>
```dockerfile
kubectl run mybusybox --image=busybox --dry-run=client -o yaml -- sleep 3600
```
> the command '--' means everything after will be executed as a command in a container, and not as a command of Kubectl.   
Above code results in:
```dockerfile
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mybusybox
  name: mybusybox
spec:
  containers:
  - args:
    - sleep
    - "3600"
    image: busybox
    name: mybusybox
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

<mark>Get into a sidecar Pod</mark>
```dockerfile
kubectl exec -it sidecar-pod -c sidecar -- /bin/bash
```
> above code is based in the below yaml:
```dockerfile
kind: Pod
apiVersion: v1
metadata:
  name: sidecar-pod
spec:
  volumes:
  - name: logs
    emptyDir: {}

  containers:
  - name: app
    image: busybox
    command: ["/bin/sh"]
    args: ["-c", "while true; do date >> /var/log/date.txt; sleep
10;done"]
    volumeMounts:
    - name: logs
      mountPath: /var/log

  - name: sidecar
    image: centos/httpd
    ports:
    - containerPort: 80
    volumeMounts:
    - name: logs
      mountPath: /var/www/html

```

<mark>Inside a container, ie: exec</mark>
```dockerfile
yum install -y curl
```
> install curl app  
then:  
curl http://localhost/date.txt


---

<mark>Namespaces</mark>
```dockerfile
kubectl create namespace mynamespace

```
> to create a namespace

```dockerfile
kubectl ... -n namespace
```
> to work in specific namespace 


```dockerfile
kubectl get ... --all-n namespaces

kubectl get all -A
```
> to see resources in all namespaces 

```dockerfile
kubectl create ns secret
```
> create a secret namespace 

```dockerfile
kubectl create -f busybox-ns.yaml
```
> create a anoter namespace 

```dockerfile
kubectl run secretginx --image=nginx -n secret
```
> run a container in specific namespace 

```dockerfile
kubectl describe ns secret
```
> describe specific namespace 


---