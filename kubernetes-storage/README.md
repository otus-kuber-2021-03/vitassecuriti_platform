# HomeWork storage

```
$ kubectl api-resources | grep -E "^Name|csi|storage|PersistentVolume"
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
csidrivers                                     storage.k8s.io/v1                      false        CSIDriver
csinodes                                       storage.k8s.io/v1                      false        CSINode
storageclasses                    sc           storage.k8s.io/v1                      false        StorageClass
volumeattachments                              storage.k8s.io/v1                      false        VolumeAttachment
```
---
```
$ kubectl apply -f storage-class.yaml
storageclass.storage.k8s.io/csi-hostpath-sc created

$ kubectl get storageclasses.storage.k8s.io
NAME                 PROVISIONER             RECLAIMPOLICY   VOLUMEBINDINGMODE      ALLOWVOLUMEEXPANSION   AGE
csi-hostpath-sc      hostpath.csi.k8s.io     Delete          Immediate              true                   15s
standard (default)   rancher.io/local-path   Delete          WaitForFirstConsumer   false                  7m21s
```
---
```
$ kubectl apply -f storage-pvc.yaml
persistentvolumeclaim/csi-pvc created

$ kubectl get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM             STORAGECLASS      REASON   AGE
pvc-ce3ffa25-9ee6-42e0-b587-f247de8346b7   1Gi        RWO            Delete           Bound    default/csi-pvc   csi-hostpath-sc            
```
---
```
$ kubectl apply -f storage-pod.yaml
pod/storage-pod created


$ kubectl get pods
NAME                   READY   STATUS    RESTARTS   AGE
csi-hostpathplugin-0   9/9     Running   0          15m
storage-pod            1/1     Running   0          32s


$ kubectl exec -it storage-pod -- /bin/sh
/ # ls
bin   data  dev   etc   home  proc  root  sys   tmp   usr   var


$ kubectl describe pod storage-pod
Name:         storage-pod
Namespace:    default
Priority:     0
Node:         stor-control-plane/172.18.0.2
Start Time:   Mon, 23 Aug 2021 23:02:45 +0100
Labels:       <none>
Annotations:  <none>
Status:       Running
IP:           10.244.0.6
IPs:
  IP:  10.244.0.6
Containers:
  test:
    Container ID:  containerd://a90dd16885172aca3f9f9cc7a0f95b51227c46750204cd3ab06dbbc901fb5875
    Image:         busybox
    Image ID:      docker.io/library/busybox@sha256:b37dd066f59a4961024cf4bed74cae5e68ac26b48807292bd12198afa3ecb778
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -ec
      sleep 3600
    State:          Running
      Started:      Mon, 23 Aug 2021 23:02:59 +0100
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /data from csi-vol (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-rmj6d (ro)
...
```
