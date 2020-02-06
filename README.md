# This repo contains the tips to crack CKAD exam

## attributes of differnet rescource to mug up

### pod.spec.containers

#### container command and args
```
command:["/bin/sh"]
args:["-c","echo welcome"]
```

#### container ports
```
ports:
- containerPort: $(myenv1)
```

#### container env
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

#### container envFrom
```
envFrom:
- configMapRef
  name: cfgMapName
- secretRef
  name: secretName
```

### pod.spec

#### pod volumes
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
- name: my-vol-5
  configMap:     ## Note the diffrence between the envFrom configMapRef 
    name: mycfg
```

### pod.spec.containers

#### containers|pod securityContext

```
securityContext:
  runAsUser: <userId>
  runAsGroup: <groupId>
  fsGroup: <groupId applies to all container is pod>
  capabilities: ## Not supported by Pod level sercurityContext
        add: ["NET_ADMIN", "SYS_TIME"]
```

#### container volumeMounts
```
volumeMounts:
- name: my-vol-1
  mountPath: /etc/data
- name: my-vol-2
  mountPath: /etc/data2
- name: my-vol-3
  mountPath: /etc/data3
- name: my-vol-4
  mountPath: /etc/data4
- name: my-vol-5
  mountPath: /etc/data5
```

### pod.spec.containers
#### container resources
```
resources:
  requests:
    cpu: .2
    memory: 100Mi
  limits:
    cpu: 1
    memory: 300Mi
```


## configmap

```
kubectl create configmap|cm <name> --from-literal=key=val --from-literal=key2=val2

kubectl create configmap|cm <name> --from-file=path/to/bar

kubectl create configmap|cm <name> --from-env-file=cfgfile.env

```

## Jobs and Cronjobs

```
kubectl create job <jobname> --image=<image>

kubectl create cronjob <cjname> --image=<image>  --schedule="* * * * *"
```

### run `x` number of same job
### job.spec
```
completions: x
```

### run `y` number of same job in parallel
### job.spec
```
parallelism: y
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

### kubectl `scale` to scale deployment, ReplicaSet, Replication Controller or Stateful sets
```
kubectl scale deploy|rs|rc <name> --replicas=10 
```

### kubectl `autoscale` to scale deployment, ReplicaSet, Replication Controller or Stateful sets
```
kubectl autoscale deploy|rs|rc <name> --min=2 --max=10 --cpu-percent=80
```

### kubectl `rollout` to check the deployment update and undo it in case of issue
```
kubectl rollout history deploy <name> # show rollout history
kubectl rollout undo deploy <name> # undo the rollout to last revision
kubectl rollout undo --to-version=<revision_number> deploy <name> # undo the rollout to given revision
kubectl rollout history deploy <name> --revision=5  #to see the detail about the revision
kubectl rollout pause deploy <name> # pause the rollout
kubectl rollout resume deploy <name> # to resume a paused rollout
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

