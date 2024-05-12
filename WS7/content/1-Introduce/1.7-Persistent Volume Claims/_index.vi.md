---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: "<b> 1.7 </b>"
---

#### Yêu cầu Khối Lưu Trữ

  - Đưa tôi đến [Bài giảng](https://kodekloud.com/topic/persistent-volume-claims-4/)

Trong phần này, chúng ta sẽ xem xét **Yêu cầu Khối Lưu Trữ**

- Bây giờ chúng ta sẽ tạo một Yêu cầu Khối Lưu Trữ để làm cho lưu trữ có sẵn cho node.
- Khối Lưu Trữ và Yêu cầu Khối Lưu Trữ là hai đối tượng riêng biệt trong không gian tên Kubernetes.
- Khi Yêu cầu Khối Lưu Trữ được tạo, Kubernetes sẽ gán các Khối Lưu Trữ vào yêu cầu dựa trên yêu cầu và thuộc tính được thiết lập trên khối.

![class-17](../../images/class17.PNG)

- Nếu các thuộc tính không khớp hoặc Khối Lưu Trữ không có sẵn cho Yêu cầu Khối Lưu Trữ thì nó sẽ hiển thị trạng thái đang chờ.

```yaml
pvc-definition.yaml

kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: myclaim
spec:
  accessModes: [ "ReadWriteOnce" ]
  resources:
   requests:
     storage: 1Gi
```

```yaml
pv-definition.yaml

kind: PersistentVolume
apiVersion: v1
metadata:
    name: pv-vol1
spec:
    accessModes: [ "ReadWriteOnce" ]
    capacity:
     storage: 1Gi
    hostPath:
     path: /tmp/data
```

#### Tạo Khối Lưu Trữ

```
$ kubectl create -f pv-definition.yaml
persistentvolume/pv-vol1 đã được tạo

$ kubectl get pv
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
pv-vol1   1Gi        RWO            Retain           Available                                   10s
```


#### Tạo Yêu Cầu Khối Lưu Trữ

```
$ kubectl create -f pvc-definition.yaml
persistentvolumeclaim/myclaim đã được tạo

$ kubectl get pvc
NAME      STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
myclaim   Pending                                                     35s

$ kubectl get pvc
NAME      STATUS   VOLUME    CAPACITY   ACCESS MODES   STORAGECLASS   AGE
myclaim   Bound    pv-vol1   1Gi        RWO                           1min

```

#### Xóa Yêu Cầu Khối Lưu Trữ

```
$ kubectl delete pvc myclaim
```

#### Xóa Khối Lưu Trữ

```
$ kubectl delete pv pv-vol1
```


#### Tài liệu Tham Khảo về Yêu Cầu Khối Lưu Trữ Kubernetes

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims
- https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#persistentvolumeclaim-v1-core
- https://docs.cloud.oracle.com/en-us/iaas/Content/ContEng/Tasks/contengcreatingpersistentvolumeclaim.htm