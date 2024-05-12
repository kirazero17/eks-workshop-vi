---
title: "Accessing AWS CodeCommit"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 3.1.1 </b>"
---

#### Accessing AWS CodeCommit

🚀 Chúng ta đã tạo thành công một kho AWS CodeCommit trong môi trường thí nghiệm của chúng ta, nhưng trước khi Cloud9 có thể kết nối đến nó, chúng ta cần hoàn thành một số bước.

Để tránh những cảnh báo sau này, chúng ta có thể thêm các khóa SSH cho CodeCommit vào tệp hosts đã biết:

```bash
$ mkdir -p ~/.ssh/
$ ssh-keyscan -H git-codecommit.${AWS_REGION}.amazonaws.com &> ~/.ssh/known_hosts
```

Và chúng ta có thể thiết lập một danh tính mà Git sẽ sử dụng cho các commit của chúng ta:

```bash
$ git config --global user.email "you@eksworkshop.com"
$ git config --global user.name "Tên của bạn"
```