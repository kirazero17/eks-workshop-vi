---
title: "EFS CSI Driver"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 4.2 </b>"
---


Trước khi đi sâu vào phần này, hãy đảm bảo bạn đã làm quen với các đối tượng lưu trữ trong Kubernetes (volumes, persistent volumes (PV), persistent volume claim (PVC), cấp phát động và lưu trữ tạm thời) chúng tôi đã giới thiệu ở [đầu workshop này](../../1-Introduce/1.10-EKS%20Storage).

Các trình điều khiển CSI cho Amazon FSx for NetApp ONTAP (Amazon FSxN ONTAP) giúp chạy các ứng dụng container dựa trên trạng thái. Amazon FSxN ONTAP CSI cung cấp giao diện lưu trữ container cho phép các cụm Kubernetes chạy trên AWS quản lý vọng đời của các hệ tập tin FSxN ONTAP.

Để cấp phát động hệ tập tin Amazon FSxN ONTAP trên cụm EKS, ta cần xác nhận [trình điều khiển Amazon FSxN ONTAP CSI](https://github.com/NetApp/trident) đã được cài đặt. Trình điều khiển Amazon FSxN ONTAP CSI triển khai các thông số cho trình điều phối container nhằm quản lý vòng đời hệ tập tin Amazon FSxN ONTAP.

Như một phần môi trường workshop, cụm EKS đã được cài đặt sẵn trình điều khiển Amazon FSxN ONTAP CSI. Ta có thể xác nhận điều này bằng lệnh sau:

```bash
$ kubectl get pods -n trident
NAME                                  READY   STATUS    RESTARTS   AGE
trident-controller-68f86749df-tr9nw   6/6     Running   0          25m
trident-node-linux-7wkg9              2/2     Running   0          25m
trident-node-linux-9g6w4              2/2     Running   0          25m
trident-node-linux-vpvnh              2/2     Running   0          25m
trident-operator-56fb7f67c4-vws4m     1/1     Running   0          29m
```
