---
title: "Các Bước Chuẩn Bị"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 2. </b>"
---

#### Lưu trữ trên EKS

[Storage on EKS](https://docs.aws.amazon.com/eks/latest/userguide/storage.html) sẽ cung cấp một cái nhìn tổng quan về cách tích hợp hai dịch vụ Lưu trữ của AWS với cụm EKS của bạn.

Trước khi chúng ta đào sâu vào việc triển khai, dưới đây là một tóm tắt về hai dịch vụ lưu trữ của AWS mà chúng ta sẽ sử dụng và tích hợp với EKS:

- [Amazon Elastic Block Store](https://aws.amazon.com/ebs/) (hỗ trợ EC2): một dịch vụ lưu trữ khối cung cấp truy cập trực tiếp từ các EC2 Instance và container đến một ổ lưu trữ được thiết kế cho cả hiệu suất và khối lượng công việc giao dịch ở bất kỳ quy mô nào.
- [Amazon Elastic File System](https://aws.amazon.com/efs/) (hỗ trợ Fargate và EC2): một hệ thống tệp được quản lý hoàn toàn, có khả năng mở rộng và đàn hồi phù hợp cho phân tích dữ liệu lớn, phục vụ web và quản lý nội dung, phát triển và kiểm thử ứng dụng, quy trình công việc phương tiện và giải trí, sao lưu cơ sở dữ liệu và lưu trữ container. EFS lưu trữ dữ liệu của bạn một cách dự phòng trên nhiều Khu vực Khả dụng (AZ) và cung cấp truy cập độ trễ thấp từ các pod Kubernetes bất kể AZ chúng đang chạy ở.
- [Amazon FSx for NetApp ONTAP](https://aws.amazon.com/fsx/netapp-ontap/) (hỗ trợ EC2): Lưu trữ chia sẻ được quản lý hoàn toàn dựa trên hệ thống tệp ONTAP phổ biến của NetApp. FSx cho NetApp ONTAP lưu trữ dữ liệu của bạn một cách dự phòng trên nhiều Khu vực Khả dụng (AZ) và cung cấp truy cập độ trễ thấp từ các pod Kubernetes bất kể AZ chúng đang chạy ở.
- [FSx for Lustre](https://aws.amazon.com/fsx/lustre/) (hỗ trợ EC2): một hệ thống tệp hoàn toàn được quản lý, hiệu suất cao được tối ưu hóa cho các khối công việc như học máy, tính toán hiệu suất cao, xử lý video, mô hình tài chính, tự động hóa thiết kế điện tử và phân tích. Với FSx cho Lustre, bạn có thể nhanh chóng tạo ra một hệ thống tệp hiệu suất cao liên kết với kho lưu trữ dữ liệu S3 của bạn và truy cập các đối tượng S3 một cách trong suốt như các tệp. **FSx sẽ được thảo luận trong các module tương lai của workshop này**

Quan trọng hơn cả là nắm vững một số khái niệm về [Lưu trữ Kubernetes](https://kubernetes.io/docs/concepts/storage/):

- [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/): Các tệp trên đĩa trong một container là tạm thời, điều này đặt ra một số vấn đề đối với các ứng dụng phức tạp khi chạy trong các container. Một vấn đề là mất các tệp khi một container gặp sự cố. Kubelet khởi động lại container nhưng với một trạng thái sạch sẽ. Một vấn đề thứ hai xảy ra khi chia sẻ tệp giữa các container chạy cùng nhau trong một Pod. Trừu tượng hóa khối lưu trữ Kubernetes giải quyết cả hai vấn đề này. Việc quen thuộc với Pods được đề xuất.
- [Ephemeral Volumes](https://kubernetes.io/docs/concepts/storage/ephemeral-volumes/) được thiết kế cho các trường hợp sử dụng này. Vì các khối lưu trữ tuân theo vòng đời của Pod và được tạo ra và xóa cùng với Pod, Pods có thể bị dừng lại và khởi động lại mà không bị giới hạn ở nơi mà một số khối lưu trữ lâu dài có sẵn.
- [Persistent Volumes (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) là một phần của lưu trữ trong một cụm đã được cung cấp bởi một quản trị viên hoặc được cung cấp động bằng cách sử dụng Lớp Lưu trữ. Đó là một tài nguyên trong cụm giống như một nút là một tài nguyên của cụm. PVs là plugin khối lưu trữ giống như Volumes, nhưng có vòng đời độc lập với bất kỳ Pod cụ thể nào sử dụng PV. Đối tượng API này ghi lại các chi tiết của việc triển khai lưu trữ, có thể là NFS, iSCSI hoặc hệ thống lưu trữ cụ thể của nhà cung cấp đám mây.
