---
layout: post
title: Persistent storage
categories:
  - back-end
tags:
  - kubernetes
---

## Storage in Paas 

- Paas에서는 데이터를 저장하기 위해 특정 물리 공간을 할당해 주지 않는다. 대신, PV(Persistent Volume)을 제공하여 데이터를 저장 하도록 해준다.
- PV는 각 pod에서 직접 mount하여 사용하는것이 아니라 PVC(Persistent Volume Claim)을 호출하면 PVC에서 조건에 맞는 PV를 bind하여 사용하도록 해준다.



### workbench mount path

- server file system

  - DEP_FS_PATH=/workbench/workbench_fs

- mariadb

  ```bash
  MariaDB [(none)]> show variables like 'datadir';
  +---------------+-----------------+
  | Variable_name | Value           |
  +---------------+-----------------+
  | datadir       | /var/lib/mysql/ |
  +---------------+-----------------+
  1 row in set (0.01 sec)
  ```

### Reclaiming

Persistent Volume에 저장된 데이터를 PVC와 bind가 끊어졌을 경우 어떻게 처리하는가에 대한 옵션

Workbench 에서는 CD가 발생할 때마다 데이터가 유지되어야 하기 때문에 Retain을 사용하도록 해야 한다.

#### Retain - usable

- PVC가 삭제 되더라도 PV는 계속 남아 있는다. 
- volume은 released 상태가 되지만 다른 PVC에 의해 사용될 수 없다.
- 다시 PV를 정의 하여 내부 데이터를 재사용할 수 있다. 

#### Delete - recommanded

- PVC가 삭제 되면 PV가 삭제

#### Recycle - do not use

- PVC가 삭제 되면 PV의 내부 데이터를 삭제 후 재사용
- 실제는 특정 controller가 PV를 mount 한 후 "rm -rf /data" 명령어 사용 후 재사용
- 내부 데이터를 삭제 하므로 주의를 요함.

### PV types

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes

#### Access Modes

- ReadWriteOnce(RWO) – the volume can be mounted as read-write by a single node
- ReadOnlyMany(ROX) – the volume can be mounted read-only by many nodes
- **ReadWriteMany(RWX)** – the volume can be mounted as read-write by many nodes

#### nfs

- RWX를 지원하는 PV type은 제한적인데 우리가 쓸수 있는 것은 NFS를 사용 해야 함.
  - sds에서 구현해 놓은 것들을 보면 nfs를 사용하여 RWX를 구현함.



### Storage Class

- PVC에서 storageClassName을 정의 할 경우 해당 class를 상속하여 생성된 PV만을 bind 할 수 있다. 
- 정의하지 않을 경우 defautl class를 사용. (관리자에 의해 정의 또는 기본 설정)

### Example

**PersistentVolume example**

```bash
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-5g-06
spec:
  capacity:
    storage: 5Gi
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Recycle
  nfs:
    path: "/data02/nfs/pv/pv-5g-06"
    server: 182.195.31.123
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-5g-cicd-stg-gitlab-data
spec:
  capacity:
    storage: 5Gi
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  nfs:
    path: "/data02/nfs/pv/pv-5g-cicd-stg-gitlab-data"
    server: 182.195.31.123
```
