---
title: "Consume Additional Prefixes"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 6.2 </b>"
---

#### Demonstrating VPC CNI Behavior with Additional Prefixes

Để minh họa hành vi VPC CNI khi thêm các tiền tố bổ sung vào các nút worker của chúng tôi, chúng tôi sẽ triển khai các pod tạm dừng để sử dụng nhiều địa chỉ IP hơn so với số hiện tại được gán. Chúng tôi đang sử dụng một số lượng lớn các pod này để mô phỏng việc thêm các pod ứng dụng vào cụm thông qua triển khai hoặc hoạt động mở rộng.

```file
manifests/modules/networking/prefix/deployment-pause.yaml
```

Điều này sẽ khởi động `150 pod` và có thể mất một thời gian:

```bash
$ kubectl apply -k ~/environment/eks-workshop/modules/networking/prefix
deployment.apps/pause-pods-prefix created
$ kubectl wait --for=condition=available --timeout=60s deployment/pause-pods-prefix -n other
```

Kiểm tra các pod tạm dừng có trong trạng thái chạy:

```bash
$ kubectl get deployment -n other
NAME                READY     UP-TO-DATE   AVAILABLE   AGE
pause-pods-prefix   150/150   150          150         101s
```

Khi các pod chạy thành công, chúng ta nên có thể thấy các tiền tố bổ sung được thêm vào các nút worker.

```bash
$ aws ec2 describe-instances --filters "Name=tag-key,Values=eks:cluster-name" "Name=tag-value,Values=${EKS_CLUSTER_NAME}" \
  --query 'Reservations[*].Instances[].{InstanceId: InstanceId, Prefixes: NetworkInterfaces[].Ipv4Prefixes[]}'
```

Điều này minh họa cách VPC CNI cung cấp và đính kèm động `/28` tiền tố vào các ENI khi có thêm các pod được lên lịch trên một nút cụ thể.