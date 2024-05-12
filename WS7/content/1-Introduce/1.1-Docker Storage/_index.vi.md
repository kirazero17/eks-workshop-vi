---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 1.1 </b>"
---

#### Giới thiệu về Lưu trữ Docker
  
Trong phần này, chúng ta sẽ tìm hiểu về **lưu trữ Docker**.

- Để hiểu lưu trữ trong các công cụ điều phối container như **Kubernetes**, điều quan trọng là hiểu rõ cách lưu trữ hoạt động với các container trước. Hiểu cách lưu trữ hoạt động với **Docker** trước tiên và làm cho tất cả các khái niệm cơ bản đúng sẽ làm cho việc hiểu cách nó hoạt động trong **Kubernetes** dễ dàng hơn nhiều sau này.

- Nếu bạn mới bắt đầu với **Docker** thì bạn có thể học một số khái niệm cơ bản của docker từ khóa học [Docker cho người mới bắt đầu](https://kodekloud.com/courses/docker-for-the-absolute-beginner/), miễn phí.

#### Lưu trữ Docker

- Có hai khái niệm liên quan đến Docker, đó là các trình điều khiển lưu trữ và các plugin trình điều khiển thư mục.

![EKS](/images/0005/0001.png?featherlight=false&width=90pc)

#### Chúng ta sẽ trước tiên thảo luận về các trình điều khiển lưu trữ.

#### Tài liệu Tham khảo Docker

- https://docs.docker.com/storage/storagedriver/
- https://docs.docker.com/storage/volumes/