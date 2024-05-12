---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: "<b> 1.4 </b>"
---

#### Giao diện Lưu trữ Container

Trong phần này, chúng ta sẽ tìm hiểu về **Giao diện Lưu trữ Container**.

#### Giao diện Thực thi Container

- Kubernetes sử dụng Docker một mình làm động cơ thực thi container, và toàn bộ mã để làm việc với Docker được nhúng trong mã nguồn Kubernetes. Các động cơ thực thi container khác, như rkt và CRI-O.
- Giao diện Thực thi Container là một tiêu chuẩn xác định cách một giải pháp điều phối như Kubernetes sẽ giao tiếp với các động cơ thực thi container như Docker. Nếu bất kỳ giao diện thực thi container mới nào được phát triển, họ có thể đơn giản là tuân thủ các tiêu chuẩn CRI.

![EKS](/images/0005/00011.png?featherlight=false&width=90pc)

#### Giao diện Mạng Container

- Để hỗ trợ các giải pháp mạng khác nhau, giao diện mạng container được giới thiệu. Bất kỳ nhà cung cấp mạng lưới mới nào cũng có thể đơn giản phát triển plugin của họ dựa trên các tiêu chuẩn CNI và làm cho giải pháp của họ hoạt động với Kubernetes.

![EKS](/images/0005/00012.png?featherlight=false&width=90pc)

#### Giao diện Lưu trữ Container

- Giao diện lưu trữ container được phát triển để hỗ trợ nhiều giải pháp lưu trữ. Với CSI, bạn hiện có thể viết các trình điều khiển của riêng mình cho lưu trữ của riêng bạn để hoạt động với Kubernetes. Portworx, Amazon EBS, Ảo Disk Azure, GlusterFS vv.
- CSI không phải là một tiêu chuẩn cụ thể của Kubernetes. Nó được thiết kế để là một tiêu chuẩn phổ quát và nếu triển khai, cho phép bất kỳ công cụ điều phối container nào làm việc với bất kỳ nhà cung cấp lưu trữ nào có plugin được hỗ trợ. Kubernetes, Cloud Foundry và Mesos đều tham gia với CSI.
- Nó xác định một tập hợp các RPC hoặc cuộc gọi thủ tục từ xa sẽ được gọi bởi trình điều phối container. Các trình điều khiển lưu trữ phải triển khai các RPC này.

![EKS](/images/0005/00013.png?featherlight=false&width=90pc)

#### Giao diện Lưu trữ Container

- https://github.com/container-storage-interface/spec
- https://kubernetes-csi.github.io/docs/
- http://mesos.apache.org/documentation/latest/csi/
- https://www.nomadproject.io/docs/internals/plugins/csi#volume-lifecycle