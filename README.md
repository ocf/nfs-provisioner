nfs-provisioner
======

Configuration files for the [kubernetes-incubator nfs-client provisioner](https://github.com/kubernetes-incubator/external-storage/tree/master/nfs-client). This allows us to run stateful applications over NFS without having to manually create NFS mounts in puppet.

PersistentVolumeClaims should use `storage-class: "managed-nfs-storage"` to utilize the provisioner like so:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test-claim
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Mi
  storageClassName: "managed-nfs-storage"
```

Or alternatively, for a `volumeClaimTemplates`:

```
volumeClaimTemplates:
- metadata:
    name: datadir
  spec:
    accessModes:
      - ReadWriteMany
    resources:
      requests:
        storage: 1Mi
    storageClassName: "managed-nfs-storage"
```

All data for PVCs are stored in separate subdirectories under `/opt/homes/services/nfs-provisioner/` on the NFS host.
