---
title: "Giới thiệu"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 1.2 </b>"
---

#### Lưu trữ trong Docker

Trong phần này, chúng ta sẽ xem xét **Driver Lưu trữ và Hệ thống tệp của Docker**.

- Chúng ta sẽ xem xét nơi và cách Docker lưu trữ dữ liệu và cách quản lý hệ thống tệp của các container.

#### Hệ thống tệp

- Hãy xem cách Docker lưu trữ dữ liệu trên hệ thống tệp cục bộ. Lần đầu tiên, khi bạn cài đặt Docker trên một hệ thống, nó tạo ra cấu trúc thư mục này tại /var/lib/docker.

  ```
  $ cd /var/lib/docker/

  ```

![EKS](/images/0005/0002.png?featherlight=false&width=90pc)

- Bạn có nhiều thư mục con dưới đó gọi là aufs, containers, image, volumes, vv.
- Đây là nơi mà Docker lưu trữ tất cả dữ liệu của nó theo mặc định.
- Tất cả các tệp liên quan đến các container được lưu trữ dưới thư mục containers và các tệp liên quan đến các hình ảnh được lưu trữ dưới thư mục image. Bất kỳ thư mục nào được tạo bởi các container Docker đều được tạo dưới thư mục volumes.
- Bây giờ, hãy chỉ hiểu nơi Docker lưu trữ các tệp của hình ảnh và một container và định dạng nào.
- Để hiểu điều đó, chúng ta cần hiểu về kiến ​​trúc lớp của Docker.

#### Kiến ​​trúc lớp

- Khi Docker xây dựng hình ảnh, nó xây dựng chúng trong một kiến ​​trúc lớp. Mỗi dòng lệnh trong tệp Docker tạo một lớp mới trong hình ảnh Docker chỉ với các thay đổi từ lớp trước đó.
- Như bạn có thể thấy trong hình ảnh, lớp-1 là lớp cơ sở Ubuntu, lớp-2 cài đặt các gói, lớp-3 cài đặt các gói Python, lớp-4 cập nhật mã nguồn, lớp-5 cập nhật điểm vào của hình ảnh. Khi mỗi lớp chỉ lưu trữ các thay đổi từ lớp trước đó. Nó cũng được phản ánh trong kích thước.

![EKS](/images/0005/0003.png?featherlight=false&width=90pc)

- Để hiểu rõ hơn về các ưu điểm của kiến ​​trúc lớp này, hãy xem một Dockerfile khác, điều này rất giống với ứng dụng đầu tiên của chúng tôi. chỉ mã nguồn và điểm vào là khác nhau để tạo ra ứng dụng này. 
- Khi hình ảnh được xây dựng, Docker sẽ không xây dựng ba lớp đầu tiên mà thay vào đó sẽ sử dụng lại ba lớp giống như các lớp đã xây dựng cho ứng dụng đầu tiên từ bộ nhớ cache. Chỉ tạo ra hai lớp cuối cùng với nguồn và điểm vào mới. 
- Như vậy, Docker xây dựng hình ảnh nhanh chóng và hiệu quả tiết kiệm không gian đĩa. Điều này cũng áp dụng nếu bạn muốn cập nhật mã ứng dụng của mình. Docker đơn giản là sử dụng lại tất cả các lớp trước đó từ bộ nhớ cache và nhanh chóng xây dựng lại hình ảnh ứng dụng bằng cách cập nhật mã nguồn mới nhất. 

![EKS](/images/0005/0004.png?featherlight=false&width=90pc)

- Hãy sắp xếp lại các lớp từ dưới lên để chúng ta có thể hiểu rõ hơn. Tất cả các lớp này được tạo ra khi chúng ta chạy lệnh Docker build để hình thành hình ảnh Docker cuối cùng. Khi quá trình xây dựng hoàn tất, bạn không thể chỉnh sửa nội dung của các lớp này và vì vậy chúng là chỉ đọc và bạn chỉ có thể chỉnh sửa chúng bằng cách khởi tạo một quá trình xây dựng mới.

![EKS](/images/0005/0005.png?featherlight=false&width=90pc)

- Khi bạn chạy một container dựa trên hình ảnh này, bằng cách sử dụng lệnh **Docker run** Docker tạo ra một container dựa trên các lớp này và tạo ra một lớp có thể ghi mới trên lớp hình ảnh. Lớp có thể ghi được sử dụng để lưu trữ dữ liệu được tạo ra bởi container như các tệp nhật ký được viết bởi các ứng dụng, bất kỳ tệp tạm thời nào được tạo ra bởi container.

![EKS](/images/0005/0006.png?featherlight=false&width=90pc)

- Khi container bị hủy, lớp này và tất cả các thay đổi được lưu trữ trong đó cũng bị hủy. Hãy nhớ rằng cùng một lớp hình ảnh được chia sẻ bởi tất c