# This repo contains the tips to crack CKAD exam

## POD

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


