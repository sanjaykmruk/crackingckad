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

### Kubectl expose to create service
```
kubectl expose po|svc|rc|rs|deploy <name> --port= --target-port= --name= --protocal=TCP|UDP  --labels= --type=ClusterIP|NodePort|LoadBalancer
```
### Kubectl set to edit attributes of a resource

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




