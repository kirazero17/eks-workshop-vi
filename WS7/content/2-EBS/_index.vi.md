---
title: "EBS"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 2. </b>"
---

#### Chuẩn bị môi trường cho phần này:

```bash timeout=300 wait=30
$ prepare-environment fundamentals/storage/ebs
```

Điều này sẽ thực hiện các thay đổi sau vào môi trường thí nghiệm của bạn:

- Tạo vai trò IAM cần thiết cho tiện ích điều khiển CSI của EBS.

Bạn có thể xem Terraform áp dụng các thay đổi này [tại đây](https://github.com/VAR::MANIFESTS_OWNER/VAR::MANIFESTS_REPOSITORY/tree/VAR::MANIFESTS_REF/manifests/modules/fundamentals/storage/ebs/.workshop/terraform).

[Amazon Elastic Block Store](https://aws.amazon.com/ebs/) là dịch vụ lưu trữ khối dễ sử dụng, có khả năng mở rộng và hiệu suất cao. Nó cung cấp khối lưu trữ có tính liên tục (không bay hơi) cho người dùng. Lưu trữ liên tục cho phép người dùng lưu trữ dữ liệu của họ cho đến khi họ quyết định xóa dữ liệu.

Trong phần thực hành này, chúng ta sẽ tìm hiểu về các khái niệm sau:

- Kubernetes StatefulSets
- Trình điều khiển CSI của EBS
- StatefulSet với Ổ đĩa EBS