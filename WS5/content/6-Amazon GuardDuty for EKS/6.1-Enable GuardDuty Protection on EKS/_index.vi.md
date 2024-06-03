---
title: "Kích hoạt lớp bảo vệ GuardDuty trên EKS"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b> 6.1 </b>"
---

Trong lab này, chúng ta sẽ kích hoạt Amazon GuardDuty EKS Protection. Điều này sẽ cung cấp phạm vi phát hiện mối đe dọa cho EKS Audit Log Monitoring và EKS Runtime Monitoring để giúp bảo vệ các cụm của bạn.

EKS Audit Log Monitoring sử dụng các log kiểm tra của Kubernetes để ghi lại các hoạt động theo thứ tự thời gian từ người dùng, ứng dụng sử dụng API Kubernetes và mặt bằng điều khiển để tìm kiếm các hoạt động có thể nghi ngờ.

EKS Runtime Monitoring sử dụng các sự kiện cấp độ hệ điều hành để giúp bạn phát hiện các mối đe dọa tiềm ẩn trong các nút và container Amazon EKS.

**Kích hoạt Amazon GuardDuty qua AWS CLI**

```bash test=false
$ aws guardduty create-detector --enable --features '[{"Name" : "EKS_AUDIT_LOGS", "Status" : "ENABLED"}, {"Name" : "EKS_RUNTIME_MONITORING", "Status" : "ENABLED", "AdditionalConfiguration" : [{"Name" : "EKS_ADDON_MANAGEMENT", "Status" : "ENABLED"}]}]'
{
    "DetectorId": "1qaz0p2wsx9ol3edc8ik4rfv7ujm5tgb6yhn"
}
```

**Kích hoạt Amazon GuardDuty qua AWS Console**

Truy cập [Bảng điều khiển Amazon GuardDuty](https://console.aws.amazon.com/guardduty/home)

Nhấp vào nút **Get Started**.

![](assets/gd_getstart.png)

Nhấp vào **Enable GuardDuty**

![](assets/gd_enable.png)


Truy cập **EKS Protection** trên menu bên trái và kiểm tra lại rằng Bảo vệ EKS đã được kích hoạt cho cả hai Audit Logs và Runtime Monitoring.

![](assets/eksprotection.png)

Hãy kiểm tra tab **EKS clusters runtime coverage.**.

![](assets/runtime-coverage.png)

*Nếu cụm của bạn không xuất hiện trong danh sách Cụm hoặc thống kê phạm vi không hiển thị 1/1 (100%), hãy đợi thêm vài phút để Amazon GuardDuty hoàn thành việc triển khai mô hình giám sát.*

Bạn cũng có thể xác nhận việc triển khai Pod `aws-guardduty-agent` trong Cụm EKS của bạn.

```bash test=false
$ kubectl -n amazon-guardduty get pods                                                                                                                
NAME                        READY   STATUS    RESTARTS   AGE
aws-guardduty-agent-h7qg5   1/1     Running   0          58s
aws-guardduty-agent-hgbsg   1/1     Running   0          58s
aws-guardduty-agent-k7x2b   1/1     Running   0          58s
```

Sau đó, truy cập **Findings** trên menu bên trái. Bạn sẽ thấy rằng chưa có phát hiện nào có sẵn.

![](assets/findings.png)