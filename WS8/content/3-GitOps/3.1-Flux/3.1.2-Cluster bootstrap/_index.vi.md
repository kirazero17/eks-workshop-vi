---
title: "Cluster bootstrap"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 3.1.2 </b>"
---

#### Cluster bootstrap

Quá trình khởi động nhanh (bootstrap) cài đặt các thành phần Flux trên một cụm và tạo các tệp liên quan trong kho lưu trữ để quản lý đối tượng cụm bằng GitOps với Flux.

Trước khi khởi động nhanh một cụm, Flux cho phép chúng ta chạy các kiểm tra trước khởi động để xác minh mọi thứ đã được thiết lập đúng cách. Chạy lệnh sau để Flux CLI thực hiện các kiểm tra:

```bash
$ flux check --pre
> kiểm tra tiền điều kiện
> Kubernetes VAR::KUBERNETES_NODE_VERSION >=1.20.6-0
> các kiểm tra tiền điều kiện đã qua
```

Bây giờ hãy khởi động nhanh Flux trên cụm EKS của chúng tôi bằng cách sử dụng kho lưu trữ CodeCommit:

```bash
$ flux bootstrap git \
  --url=ssh://${GITOPS_IAM_SSH_KEY_ID}@git-codecommit.${AWS_REGION}.amazonaws.com/v1/repos/${EKS_CLUSTER_NAME}-gitops \
  --branch=main \
  --private-key-file=${HOME}/.ssh/gitops_ssh.pem \
  --network-policy=false \
  --silent
```

Hãy phân tích lệnh ở trên:

- Trước tiên, chúng tôi cho biết cho Flux biết kho Git nào để sử dụng để lưu trữ trạng thái của nó
- Sau đó, chúng tôi truyền `branch` Git mà chúng tôi muốn phiên bản của Flux này sử dụng, vì một số mẫu liên quan đến nhiều nhánh trong cùng một kho Git
- Cuối cùng, chúng tôi sẽ sử dụng SSH để Flux kết nối và xác thực bằng cách sử dụng khóa SSH tại `/home/ec2-user/gitops_ssh.pem`

Bây giờ, hãy xác minh rằng quá trình khởi động nhanh đã hoàn thành thành công bằng cách chạy lệnh sau:

```bash
$ flux get kustomization
NAME            REVISION        SUSPENDED       READY   MESSAGE
flux-system     main/6e6ae1d    False           True    Applied revision: main/6e6ae1d
```

Điều đó cho thấy rằng Flux đã tạo ra kustomization cơ bản và rằng nó đồng bộ với cụm.