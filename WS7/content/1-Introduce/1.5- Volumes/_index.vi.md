---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b> 1.5 </b>"
---

#### Tích Hợp Ổ Đĩa (Volumes)

Trong phần này, chúng ta sẽ xem xét về **Tích Hợp Ổ Đĩa (Volumes)**

- Chúng ta đã thảo luận về lưu trữ Docker, Nếu chúng ta không gắn kết ổ đĩa vào thời gian chạy container, khi container bị hủy và sau đó tất cả dữ liệu sẽ bị mất. Vì vậy, chúng ta cần lưu trữ dữ liệu vào bên trong container Docker để đính kèm một ổ đĩa cho các container khi chúng được tạo ra.
- Dữ liệu được xử lý bởi container hiện đang được đặt trong ổ đĩa này, giữ nó vĩnh viễn. Ngay cả khi container bị xóa, dữ liệu vẫn tồn tại trong ổ đĩa.

- Trong thế giới Kubernetes, các POD được tạo ra trong Kubernetes có tính chất tạm thời. Khi một POD được tạo ra để xử lý dữ liệu và sau đó bị xóa, dữ liệu được xử lý bởi nó cũng bị xóa đi.
- Ví dụ, chúng ta tạo ra một POD đơn giản tạo ra một số ngẫu nhiên từ 1 đến 100 và ghi nó vào một tệp tại `/opt/number.out`. Để lưu trữ vào ổ đĩa.
- Chúng ta tạo ra một ổ đĩa cho điều đó. Trong trường hợp này, tôi chỉ định một đường dẫn `/data` trên máy chủ. Các tệp được lưu trữ trong thư mục data trên nút của tôi. Chúng ta sử dụng trường volumeMounts trong mỗi container để gắn ổ đĩa dữ liệu vào thư mục `/opt` bên trong container. Số ngẫu nhiên sẽ được ghi vào `/opt` mount bên trong container, đó cũng là ổ đĩa dữ liệu nằm trong thư mục `/data` trên máy chủ thực tế. Khi POD bị xóa, tệp chứa số ngẫu nhiên vẫn còn tồn tại trên máy chủ.

![EKS](/images/0005/00014.png?featherlight=false&width=90pc)



#### Các Lựa Chọn Lưu Trữ Ổ Đĩa

- Trong các ổ đĩa, loại ổ đĩa hostPath là tốt cho một nút duy nhất. Nó không được khuyến nghị sử dụng trong các cụm nhiều nút.
- Trong Kubernetes, nó hỗ trợ một số loại giải pháp lưu trữ tiêu chuẩn như NFS, GlusterFS, CephFS hoặc các giải pháp đám mây công cộng như AWS EBS, Azure Disk hoặc Google's Persistent Disk.

![EKS](/images/0005/00015.png?featherlight=false&width=90pc)



```
volumes:
- name: data-volume
  awsElasticBlockStore:
    volumeID: <volume-id>
    fsType: ext4
```

#### Tài Liệu Tham Khảo Ổ Đĩa Kubernetes

- https://kubernetes.io/docs/concepts/storage/volumes/
- https://kubernetes.io/docs/tasks/configure-pod-container/configure-volume-storage/
- https://unofficial-kubernetes.readthedocs.io/en/latest/concepts/storage/volumes/
- https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.18/#volume-v1-core