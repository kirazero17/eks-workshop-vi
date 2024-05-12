---
title: "Secrets"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b> 1.5 </b>"
---

#### Secrets

Trong phần này, chúng ta sẽ tìm hiểu về các Secret trong Kubernetes.

#### Ứng dụng Web-Mysql

![EKS](/images/0006/00021.png?featherlight=false&width=90pc)

- Một cách là di chuyển các thuộc tính/env của ứng dụng vào một configmap. Nhưng configmap lưu trữ dữ liệu dưới dạng văn bản thô. Điều này chắc chắn không phải là nơi đúng để lưu trữ mật khẩu.
  ```
  apiVersion: v1
  kind: ConfigMap
  metadata:
   name: app-config
  data:
    DB_Host: mysql
    DB_User: root
    DB_Password: paswrd
  ```
![EKS](/images/0006/00022.png?featherlight=false&width=90pc)
  
- Secret được sử dụng để lưu trữ thông tin nhạy cảm. Chúng tương tự như configmaps nhưng chúng được lưu trữ dưới dạng mã hóa hoặc mã hóa băm.

#### Có 2 bước liên quan đến Secret
- Thứ nhất, Tạo một Secret
- Thứ hai, Tiêm Secret vào một pod.

![EKS](/images/0006/00023.png?featherlight=false&width=90pc)
  
#### Có 2 cách tạo Secret
  ```
  $ kubectl create secret generic app-secret --from-literal=DB_Host=mysql --from-literal=DB_User=root --from-literal=DB_Password=paswrd
  $ kubectl create secret generic app-secret --from-file=app_secret.properties
  ```

![EKS](/images/0006/00024.png?featherlight=false&width=90pc)
  
- Cách tuyên bố
  ```
  Tạo một giá trị băm của mật khẩu và truyền nó vào giá trị định nghĩa secret-data.yaml như là một giá trị cho biến DB_Password.
  $ echo -n "mysql" | base64
  $ echo -n "root" | base64
  $ echo -n "paswrd"| base64
  ```
  
  Tạo một tệp định nghĩa Secret và chạy `kubectl create` để triển khai nó
  ```
  apiVersion: v1
  kind: Secret
  metadata:
   name: app-secret
  data:
    DB_Host: bX1zcWw=
    DB_User: cm9vdA==
    DB_Password: cGFzd3Jk
  ```
  ```
  $ kubectl create -f secret-data.yaml
  ```

![EKS](/images/0006/00025.png?featherlight=false&width=90pc)
  
#### Mã hóa Secret

![EKS](/images/0006/00026.png?featherlight=false&width=90pc)
  
#### Xem Secret
- Để xem các Secret
  ```
  $ kubectl get secrets
  ```
- Để mô tả Secret
  ```
  $ kubectl describe secret
  ```
- Để xem các giá trị của Secret
  ```
  $ kubectl get secret app-secret -o yaml
  ```
  
![EKS](/images/0006/00027.png?featherlight=false&width=90pc)
  
#### Giải mã Secret
- Để giải mã Secret
  ```
  $ echo -n "bX1zcWw=" | base64 --decode
  $ echo -n "cm9vdA==" | base64 --decode
  $ echo -n "cGFzd3Jk" | base64 --decode
  ```
![EKS](/images/0006/00028.png?featherlight=false&width=90pc)
  
#### Cấu hình Secret với một pod
- Để tiêm một Secret vào một pod, thêm một thuộc tính mới **`envFrom`** theo sau là tên **`secretRef`** và sau đó tạo định nghĩa pod
  
  ```
  apiVersion: v1
  kind: Secret
  metadata:
   name: app-secret
  data:
    DB_Host: bX1zcWw=
    DB_User: cm9vdA==
    DB_Password: cGFzd3Jk
  ```
  ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: simple-webapp-color
   spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      envFrom:
      - secretRef:
          name: app-secret
   ```
  ```
  $ kubectl create -f pod-definition.yaml
  ```
 ![EKS](/images/0006/00029.png?featherlight=false&width=90pc)
  
#### Có cách khác để tiêm Secret vào các pod.
- Bạn có thể tiêm như **`biến ENV đơn`**
- Bạn có thể tiêm toàn bộ Secret như các tệp trong một **`Thư mục`**

 ![EKS](/images/0006/00030.png?featherlight=false&width=90pc)
  
#### Secret trong pod như thư mục
- Mỗi thuộc tính trong Secret được tạo thành một tệp với giá trị của Secret là nội dung của nó.
  
 ![EKS](/images/0006/00031.png?featherlight=false&width=90pc)
  

#### Ghi chú Bổ sung: [Một lưu ý về Secret](https://kodekloud.com/topic/a-note-on-secrets/)

Hãy nhớ rằng Secret mã hóa dữ liệu dưới dạng base64. Bất kỳ ai có Secret được mã hóa base64 có thể dễ dàng giải mã nó. Vì vậy, Secret có thể được coi là không an toàn.

Khái niệm về sự an toàn của Secret trong Kubernetes hơi rắc rối. Trang [tài liệu Kubernetes](https://kubernetes.io/docs/concepts/configuration/secret) và nhiều blog khác trên mạng đề cập đến Secret như một "lựa chọn an toàn" để lưu trữ dữ liệu nhạy cảm. Chúng an toàn hơn so với việc lưu trữ dưới dạng văn bản thô vì chúng giảm nguy cơ tiết lộ mật khẩu và dữ liệu nhạy cảm

 khác. Theo quan điểm của tôi, không phải là Secret chính nó là an toàn, mà là các thực hành xung quanh nó.

Secret không được mã hóa, vì vậy không an toàn trong ý nghĩa đó. Tuy nhiên, một số thực hành tốt xung quanh việc sử dụng Secret làm cho nó an toàn hơn. Như trong các thực hành tốt như:

- Không kiểm tra tệp định nghĩa đối tượng Secret vào các kho lưu trữ mã nguồn.
- [Kích hoạt Mã hóa ở Trạng thái nghỉ](https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/) cho Secret để chúng được lưu trữ dưới dạng mã hóa trong ETCD.

Cũng như cách Kubernetes xử lý các Secret. Như:

- Một Secret chỉ được gửi đến một nút nếu một pod trên nút đó cần nó.
- Kubelet lưu trữ Secret vào một tmpfs để Secret không được ghi vào bộ nhớ lưu trữ.
- Khi Pod phụ thuộc vào Secret được xóa, kubelet sẽ xóa bản sao cục bộ của dữ liệu Secret của mình.

Đọc về [bảo vệ](https://kubernetes.io/docs/concepts/configuration/secret/#protections) và [nguy cơ](https://kubernetes.io/docs/concepts/configuration/secret/#risks) của việc sử dụng Secret [ở đây](https://kubernetes.io/docs/concepts/configuration/secret/#risks)

Tuy nhiên, còn có các cách khác tốt hơn để xử lý dữ liệu nhạy cảm như mật khẩu trong Kubernetes, như sử dụng các công cụ như Helm Secrets, [HashiCorp Vault](https://www.vaultproject.io/). Tôi hy vọng sẽ có một bài giảng về chúng trong tương lai.

#### Tài liệu Tham khảo K8s
- https://kubernetes.io/docs/concepts/configuration/secret/
- https://kubernetes.io/docs/concepts/configuration/secret/#use-cases
- https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/

