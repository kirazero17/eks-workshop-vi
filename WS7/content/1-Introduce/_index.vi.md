---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 1. </b>"
---

#### Giới thiệu

#### Hiểu về mạng trong Kubernetes

Hiểu về mạng trong Kubernetes là vô cùng quan trọng để vận hành cụm và ứng dụng của bạn một cách hiệu quả. Trong chương này, chúng ta sẽ đào sâu vào các khía cạnh khác nhau của mạng trong Kubernetes, bao gồm mạng Pod, mạng dịch vụ, và giao tiếp dịch vụ.

Trong Amazon EKS, mạng Pod, còn được gọi là mạng cụm, được giải quyết thông qua việc sử dụng một plugin CNI của Kubernetes gọi là Amazon VPC CNI. Chúng tôi rất khuyến khích khám phá các tùy chọn khác nhau có sẵn với Amazon VPC CNI trước khi chuyển sang Amazon VPC Lattice.

- Các phần cần tìm hiểu 
    - Switching, Routing and Gateways
      - Switching
      - Routing
      - Default Gateway
    - DNS
      - DNS Configuration on Linux
    - CoreDNS
    - Network Namespace
    - Docker Networking
    - CNI
- Cluster Networking
- Pod Networking
- CNI in Kubernetes
- CoreDNS
- Ingress
 