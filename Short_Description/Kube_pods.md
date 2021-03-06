1.  **Print a list of pods:**  
``` bash
kubectl get pods
kubectl get pods -o wide
```
2.  **Show additional information about pod:**  
``` bash
kubectl describe pods/<pod_name>
```
3. **Show yaml file of the pod:**  
``` bash
kubectl get po <pod_name> -o yaml  
kubectl get po <pod_name> -o json  
```
**metadata** - include name, namespaces, tags and other additional information;  
**spec** (specification) - include pod's description: containers, volumes and other;  
**status** - include a current information about pod: status of each container, ip address and other base information.  
**Example of simple yaml manifest:**  
``` bash
apiVersion: v1  
kind: Pod  
metadata:  
  name: kubia-manual  
spec:  
  containers:  
  - image: opercoder/kubia  
    name: kubia
    volumeMounts:
    - name: mongodb-data
      mountPath: /data/db
    livenessProbe:  # Check Pod liveness
      httpGet:
        path: /
        port: 8080
      initialDelaySeconds: 15
    ports:
    - containerPort: 8080
      protocol: TCP   
  volumes:
  - name: mongodb-data
    persistentVolumeClaim:
      claimName: mongodb-pvc
```
4. **Create a pod:**
``` bash
kubectl create -f <yaml_filename>  
kubectl create -f <yaml_filename> -n <namespace_name>
```
5. **Show logs**
``` bash
kubectl logs <pod_name>
kubectl logs <pod_name> --previous // logs of previous pod (if you have a checking of liveness)
kubectl logs <pod_name> -c <container_name>
```
6. **Delete a pod**
``` bash
kubectl delete po <pod_name>
```
7. **Delete pods by label**
``` bash
kubectl delete po -l creation_method=manual
```
8. **Delete a namespace with pods**
``` bash
kubectl delete namespace namespace-test
```
9. **Delete all pods without default namespace**
``` bash
kubectl delete po --all
```
10. **Delete almost all resources except secrets and another**
``` bash
kubectl delete all --all
```
11. **Commands and arguments**  
 
| Docker      | Kubernetes  | Description                             |  
|-------------|-------------|-----------------------------------------|  
| ENTRYPOINT  | command     | An executable file into the container   |  
| CMD         | args        | Arguments passed to the executable file | 
``` bash
kind: Pod
spec:
  containers:
  - image: some/image
    command: ["/bin/command"]
    args: [foo, bar, "25"]
    or
    args:
    - foo
    - bar
    - "25"
```
12. **Environment variables**
``` bash
kind: Pod
spec:
  containers:
  - image: some/image
    env:
    - name: FIRST_VAR
      value: "foo"
    - name: SECOND_VAR
      value: "$(FIRST_VAR)bar"
```
> with ConfigMap
``` bash
env:
  - name: FIRST_VAR
    valueFrom:
      configMapKeyRef:
        name: configmap-name
        key: somekey
        optional: true # allow to create a Pod without actual configmap
```
> with ConfigMap and env names equaled keys (with or without prefix)
``` bash
spec:
  containers:
  - image: some/image
    envFrom:
    - prefix: CONFIG_
      configMapRef:
        name: configmap-name
```
13. **Create a pod with the ConfigMap as the first argument:**
``` bash
    name: fortunearg
    env:
    - name: INTERVAL
      valueFrom: 
        configMapKeyRef:
          name: fortune-config
          key: sleep-interval
    args: ["$(INTERVAL)"]
```
14. **Create a pod with the volume with the config file from the configMap:**
``` bash
- image: nginx:alpine
    name: web-server
    volumeMounts:
    ...
    - name: config
      mountPath: /etc/nginx/conf.d    
      readOnly: true
    ports:
    - containerPort: 80
      protocol: TCP
  volumes:
  ...
  - name: config
    configMap:
      name: fortune-config \\ all configMap
        or
      name: fortune-config \\ part of configMap
      items:
      - key: my-nginx-config.conf
        path: gzip.conf
 ```
15. **First start init container, then main container is started**
``` bash
spec:
  initContainers:
  - name: init
    image: registry.local/busybox
    command:
    - sh
    - -c
    - 'while true; do echo "Waiting for postgre to come up..."; nc -z zabbix-db-service 25432 && break; sleep 1; done; echo "Postgre is up!"'
```
