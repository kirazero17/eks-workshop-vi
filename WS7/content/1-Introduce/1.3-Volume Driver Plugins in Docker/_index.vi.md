---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b> 1.3 </b>"
---

#### Plugin Volume Driver trong Docker

Trong phần này, chúng ta sẽ tìm hiểu về **Plugin Volume Driver trong Docker**

- Chúng ta đã thảo luận về các driver lưu trữ. Các driver lưu trữ giúp quản lý lưu trữ trên hình ảnh và container.
- Chúng ta đã biết rằng nếu bạn muốn lưu trữ lâu dài, bạn phải tạo ra các thùng chứa. Các thùng chứa không được quản lý bởi các driver lưu trữ. Các thùng chứa được quản lý bởi các plugin driver thùng chứa. Plugin driver thùng chứa mặc định là local.
- Plugin thùng chứa local giúp tạo ra một thùng chứa trên máy chủ Docker và lưu trữ dữ liệu của nó dưới thư mục `/var/lib/docker/volumes/`.
- Có nhiều plugin driver thùng chứa khác cho phép bạn tạo ra một thùng chứa trên các giải pháp của bên thứ ba như lưu trữ tệp Azure, Lưu trữ khối DigitalOcean, Portworx, Đĩa cố định Google Compute, v.v.

![EKS](/images/0005/0009.png?featherlight=false&width=90pc)

- Khi bạn chạy một container Docker, bạn có thể chọn sử dụng một plugin driver thùng chứa cụ thể, như RexRay EBS để cung cấp một thùng chứa từ Amazon EBS. Điều này sẽ tạo ra một container và gắn kết một thùng chứa từ điện toán đám mây AWS. Khi container kết thúc, dữ liệu của bạn được an toàn trên điện toán đám mây.

```shell
$ docker run -it --name mysql --volume-driver rexray/ebs --mount src=ebs-vol,target=/var/lib/mysql mysql
```

![EKS](/images/0005/00010.png?featherlight=false&width=90pc)

#### Tài liệu tham khảo Docker

- https://docs.docker.com/engine/extend/legacy_plugins/
- https://github.com/rexray/rexray