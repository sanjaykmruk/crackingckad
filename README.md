# This repo contains the tips to crack CKAD exam

## attributes of differnet rescource to mug up

### pod.spec.containers
```
command:["/bin/sh"]
args:["-c","echo welcome"]
```
```
ports:
- containerPort: $(myenv1)
```
```
env:
- name: myenv1
  value: myval1
- name: myenv2
  valueFrom:
    configMapKeyRef:
    name: cfgMapName
    key: keyNameInCfg
- name: myevn3
  valueFrom:
    secretKeyRef:
      name: secretName
      key: secretKeyName
- name: myenv4
  valueFrom:
    fieldRef:
      fieldPath: metadata.name
- name: myenv5
  valueFrom:
    resourceFieldRef:
      containerName: <name of the container>
      resource: requests.cpu | requests.memory
```
```
envFrom:
- configMapRef
  name: cfgMapName
- secretRef
  name: secretName
```

### pod.spec
```
volumes:
- name: my-vol-1
  emptyDir: {}
- name: my-vol-2
  hostPath :
    path: /mnt/data
- name: my-vol-3
  persistentVolumeClaim:
    claimName: my-claim
- name: my-vol-4
  secret:
    secretName: my-secret
```

### pod.spec.containers
```
volumeMounts:
- name: my-vol-1
  mountPath: /etc/data
- name: my-vol-2
  mountPath: /etc/data2
```

### pod.spec.containers
```
resources:
  requests:
    cpu: .2
    memory: 100Mi
  limits:
    cpu: 1
    memory: 300Mi
```

## PersistentVolume

### persistentVolume.spec
```
capacity:
  storage: 10Gi
storageClassName: myStorageClassName
accessMode:
- ReadWriteOnce| ReadWriteMany | ReadOnlyMany
hostPath:
  path: /mnt/data
```

## PersistentVolumeClaim

### persistentVolumeClaim.spec
```
storageClassName: myStorageClassName
accessModes:
- ReadWriteOne | ReadWriteMany | ReadOnlyMany
resources:
  limits:
    memory: 300Mi
    cpu: 1
  requests:
    memory: 100Mi
    cpu: .5
```

## Commands to mug up

### Running the pod on specific node 
- pod.spec.nodeName <string> # name of the node
- pod.spec.nodeSelector <map[string]string> # labels to select the node/nodes


### kubectl `expose` to create service
```
kubectl expose po|svc|rc|rs|deploy <name> --port= --target-port= --name= --protocal=TCP|UDP  --labels= --type=ClusterIP|NodePort|LoadBalancer
```
### kubectl `set` to edit attributes of a resource

```
kubectl set env po|deploy|rc|ds|job|rs --overwrite  <env_var_name>=<value>
kubectl set env po|deploy|rc|ds|job|rs --all --list
kubectl set env po|deploy|rc|ds|job|rs --all <env_var_name>=<value>
kubectl set env po|deploy|rc|ds|job|rs  env_var_name-
```
`set image` update the image of containers in a pod, deployment,rs,rc,ds
```
kubectl set image po|deploy|rc|ds|rs <name> <container1_name>=<image>:<version>  <container2_name>=<image>:<version> 
kubectl set image po|deploy|rc|ds|rs <name> *=<image>:<version>

```

`set resources` Specify compute resource requirements (cpu, memory) for any resource that defines a pod template
```
kubectl set resources deployment|pod <name>  --limits=cpu=200m,memory=512Mi --requests=cpu=100m,memory=256Mi
```

### kubectl `label`to set or change label of a resource
```
kubectl label pod|deploy|service|rs|rc|ds --overwrite label_name=label_value
kubectl label pod|deploy|service|rs|rc|ds label_name-  #remove the label
```

### kubectl `annotate`to set or change annotation of a resource
```
kubectl annotate pod|deploy|service|rs|rc|ds --overwrite annotation_description="can be multi line description"
kubectl annotate pod|deploy|service|rs|rc|ds annotation_description-  #remove the annotation
```

### kubectl `-o jsonpath` and `-o custom-columns` to print any element value of the resource

`-o jsonpath`
```
kubectl get po -o jsonpath="{.items[*]['.metadata.name','.metadata.namespace']}"

kubectl get po <pod_name> -o jsonpath="{['.metadata.name','.metadata.namespace']}"

```

 `-o custom-columns`
 ```
 kubectl get po -o custom-columns="POD_NAME:.metadata.name, POD_NS:.metadata.namespace"
 ```


### kubectl  `--sort-by`  to sort the display by resource attribute
```
kubectl get po --sort-by=".metadata.creationTimestamp"
```

### kubectl `run` to create a pod
```
kubectl run busybox --image=busybox --env="DNS_DOMAIN=cluster" --env="POD_NAMESPACE=default" --labels="app=boxbusy,env=prod" --restart=Never
```

### kubectl `-l` or `--selector` for filtering the list of resource based on labels
```
kubectl get po -l app=nginx
kubectl get po -l app=busybox
kubectl get po -l "app in (nginx,busybox)"
kubectl get po -l app
```

