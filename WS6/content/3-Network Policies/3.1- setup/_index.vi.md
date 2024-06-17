---
title: "Cài đặt"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 3.1 </b>"
---

Trong lab này, chúng ta sẽ triển khai các chính sách mạng cho ứng dụng mẫu được triển khai trong cụm lab. Kiến ​​trúc thành phần ứng dụng mẫu được hiển thị như dưới đây.

![EKS](/images/0004/00014.png?featherlight=false&width=60pc)

Mỗi thành phần trong ứng dụng mẫu được triển khai trong namespace riêng của nó. Ví dụ, thành phần **'ui'** được triển khai trong namespace **'ui'**, trong khi dịch vụ web **'catalog'** và cơ sở dữ liệu MySQL **'catalog'** được triển khai trong namespace **'catalog'**.

Hiện tại, không có chính sách mạng nào được xác định, và bất kỳ thành phần nào trong ứng dụng mẫu đều có thể giao tiếp với bất kỳ thành phần nào khác hoặc bất kỳ dịch vụ ngoại vi nào. Ví dụ, thành phần 'catalog' có thể giao tiếp trực tiếp với thành phần 'checkout'. Chúng ta có thể xác minh điều này bằng các lệnh dưới đây:

```bash
$ kubectl exec deployment/catalog -n catalog -- curl -s http://checkout.checkout/health
{"status":"ok","info":{},"error":{},"details":{}}
```

Hãy bắt đầu bằng cách triển khai một số quy tắc mạng để chúng ta có thể kiểm soát tốt hơn luồng dữ liệu cho ứng dụng mẫu.