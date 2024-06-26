---
title: "Docker và ContainerD"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b> 1.2 </b>"
---


#### Docker và ContainerD

Trong phần này, chúng ta sẽ xem xét sự khác biệt giữa **Docker** và **ContainerD**.

Khi bạn bắt đầu tìm hiểu, bạn sẽ thường xuyên bắt gặp **Docker** và **containerd**. Khi đọc các bài blog cũ hoặc trang tài liệu, bạn sẽ thấy **Docker** được nhắc đến cùng với **Kubernetes**. Còn khi đọc blog mới, bạn sẽ thấy **containerd** và tự hỏi sự khác biệt giữa chúng là gì. Có một số công cụ CLI như **ctr**, **crictl** hay **nerdctl**, và bạn sẽ tự hỏi những công cụ CLI này là gì và bạn nên sử dụng cái nào. Đó chính là những gì tôi sẽ giải thích.

Hãy quay lại thời điểm bắt đầu của kỷ nguyên container. Ban đầu, chỉ có **Docker**. Có một số công cụ khác như **Rocket (rkt)**, nhưng trải nghiệm người dùng của **Docker** đã làm cho việc làm việc với container trở nên cực kỳ đơn giản và do đó, **Docker** trở thành công cụ container chiếm ưu thế nhất. Sau đó, **Kubernetes** ra đời để điều phối **Docker**, vì vậy **Kubernetes** được xây dựng để điều phối cụ thể **Docker** ngay từ đầu. **Docker** và **Kubernetes** được kết hợp chặt chẽ và vào thời điểm đó, **Kubernetes** chỉ hoạt động với **Docker** và không hỗ trợ bất kỳ giải pháp container nào khác.

Sau đó, **Kubernetes** ngày càng phổ biến như một công cụ điều phối container và bây giờ các runtime container khác như **rkt** muốn được hỗ trợ. Vì vậy, người dùng **Kubernetes** cần nó hoạt động với các runtime container khác ngoài **Docker**. Do đó, **Kubernetes** giới thiệu một giao diện gọi là giao diện runtime container (**CRI**), cho phép bất kỳ nhà cung cấp nào hoạt động như một runtime container cho **Kubernetes** miễn là họ tuân thủ các tiêu chuẩn **OCI**. **OCI** là viết tắt của **Open Container Initiative**, bao gồm một thông số kỹ thuật hình ảnh và một thông số kỹ thuật runtime. Thông số kỹ thuật hình ảnh định nghĩa cách một hình ảnh được xây dựng. Thông số kỹ thuật runtime định nghĩa các tiêu chuẩn về cách phát triển bất kỳ runtime container nào. Với những tiêu chuẩn này, bất kỳ ai cũng có thể xây dựng một runtime container có thể được sử dụng bởi bất kỳ ai để làm việc với **Kubernetes**.

![Kubernetes](/images/4/0003.png?featherlight=false&width=60pc)

**Rkt** và các runtime container khác tuân thủ các tiêu chuẩn **OCI** giờ đây được hỗ trợ làm runtime container cho **Kubernetes** thông qua **CRI**. Tuy nhiên, **Docker** không được xây dựng để hỗ trợ các tiêu chuẩn **CRI**. **Docker** được xây dựng trước khi **CRI** được giới thiệu và **Docker** vẫn là công cụ container chiếm ưu thế được sử dụng nhiều nhất. Vì vậy, **Kubernetes** phải tiếp tục hỗ trợ **Docker** và do đó giới thiệu **dockershim**, một cách tạm thời nhưng không hoàn hảo để hỗ trợ **Docker** ngoài **CRI**.

Trong khi hầu hết các runtime container khác làm việc theo **CRI**, **Docker** tiếp tục hoạt động mà không cần nó. Vì vậy, **Docker** không chỉ đơn thuần là một runtime container. **Docker** bao gồm nhiều công cụ được kết hợp lại, ví dụ như **Docker CLI**, **Docker API**, các công cụ xây dựng giúp xây dựng hình ảnh. Có hỗ trợ cho volumes, bảo mật, và cuối cùng là runtime container gọi là **runc**, và daemon quản lý **runc** được gọi là **containerd**. **Containerd** tương thích với **CRI** và có thể làm việc trực tiếp với **Kubernetes** như tất cả các runtime khác, vì vậy **containerd** có thể được sử dụng như một runtime riêng biệt, tách biệt khỏi **Docker**.

Vì vậy, bây giờ bạn có **containerd** như một runtime riêng biệt và **Docker** riêng biệt. **Kubernetes** tiếp tục duy trì hỗ trợ trực tiếp cho **Docker engine**, tuy nhiên, việc duy trì **dockershim** là một nỗ lực không cần thiết và gây ra rắc rối nên đã quyết định trong phiên bản 1.24 của **

Kubernetes** loại bỏ hoàn toàn **dockershim** và do đó hỗ trợ cho **Docker** đã được loại bỏ. Nhưng tất cả các hình ảnh được xây dựng trước khi **Docker** bị loại bỏ vẫn tiếp tục hoạt động vì **Docker** tuân thủ thông số kỹ thuật hình ảnh từ các tiêu chuẩn **OCI**, vì vậy tất cả các hình ảnh được xây dựng bởi **Docker** tuân thủ tiêu chuẩn và tiếp tục hoạt động với **containerd** nhưng **Docker** bản thân đã bị loại bỏ làm runtime được hỗ trợ bởi **Kubernetes**. Đó chính là toàn bộ câu chuyện, giờ hãy xem xét cụ thể hơn về **containerd**.

**Containerd**, mặc dù là một phần của **Docker**, giờ đây là một dự án riêng biệt và là thành viên của **CNCF** với tư cách là thành viên đã tốt nghiệp, vì vậy bạn có thể cài đặt **containerd** mà không cần phải cài đặt **Docker**. Nếu bạn không thực sự cần các tính năng khác của **Docker**, bạn có thể lý tưởng chỉ cài đặt **containerd**. Thông thường, chúng ta chạy container bằng lệnh `docker run` khi có **Docker**. Nếu **Docker** không được cài đặt thì làm thế nào để chạy container chỉ với **containerd**? Sau khi cài đặt **containerd**, nó đi kèm với một công cụ dòng lệnh gọi là **ctr**, và công cụ này được tạo ra chỉ để gỡ lỗi **containerd** và không thân thiện với người dùng vì nó chỉ hỗ trợ một số tính năng hạn chế và đây là tất cả những gì bạn có thể thấy trong các trang tài liệu cho công cụ này. Vì vậy, ngoài bộ tính năng hạn chế, bất kỳ cách tương tác nào khác với **containerd** bạn phải dựa vào việc gọi API trực tiếp, không phải là cách thân thiện nhất để chúng ta vận hành.

Chỉ để bạn có một ý tưởng, lệnh `ctr` có thể được sử dụng để thực hiện các hoạt động liên quan đến container cơ bản như kéo hình ảnh, ví dụ để kéo hình ảnh **redis** bạn sẽ chạy

```
ctr images pull docker.io/library/redis:alpine
```

Để chạy một container, chúng ta sử dụng lệnh `ctr run`

```
ctr run docker.io/library/redis:alpine redis
```

Nhưng như tôi đã đề cập, công cụ này chỉ dành cho gỡ lỗi **containerd** và không thân thiện với người dùng, không nên được sử dụng để quản lý container trong môi trường sản xuất. Một lựa chọn tốt hơn được khuyến nghị là công cụ **nerdctl**. Công cụ **nerdctl** là một công cụ dòng lệnh rất giống với **Docker**, vì vậy đây là một CLI giống như **Docker** cho **containerd**. Nó hỗ trợ hầu hết các tùy chọn CLI mà **Docker** hỗ trợ và ngoài ra, nó còn có lợi ích bổ sung là có thể truy cập vào các tính năng mới nhất được triển khai trong **containerd**, ví dụ như chúng ta có thể làm việc với hình ảnh container được mã hóa hoặc các tính năng mới khác sẽ cuối cùng được triển khai vào lệnh **Docker** thông thường trong tương lai. Nó cũng hỗ trợ kéo hình ảnh lười biếng, phân phối hình ảnh P2P, ký và xác minh hình ảnh và không gian tên trong **Kubernetes**, những thứ không có sẵn trong **Docker**. Vì vậy, công cụ **nerdctl** hoạt động rất giống với CLI **Docker**, vì vậy thay vì **Docker**, bạn chỉ cần thay thế nó bằng **nerdctl** để có thể chạy hầu hết các lệnh **Docker** tương tác với container như thế này.

Vì vậy, đó khá dễ dàng và trực tiếp. Bây giờ chúng ta đã nói về **ctr** và công cụ **nerdctl**, điều quan trọng là phải nói về một công cụ dòng lệnh khác được gọi là **crictl**. Trước đó chúng ta đã nói về **CRI**, là một giao diện đơn lẻ được sử dụng để kết nối với các runtime container tương thích với **CRI**, **containerd**, **rkt** và các runtime khác. **Crictl** là một tiện ích dòng lệnh được sử dụng để tương tác với runtime container tương thích với **CRI**, vì vậy đây là loại tương tác từ góc độ **Kubernetes**. Công cụ này được phát triển và b

ảo trì bởi cộng đồng **Kubernetes** và công cụ này hoạt động trên tất cả các runtime container khác nhau. Vì trước đây bạn có **ctr** và tiện ích **nerdctl** được xây dựng bởi cộng đồng **containerd** cụ thể cho **containerd**, nhưng công cụ cụ thể này là từ góc độ **Kubernetes** hoạt động trên các runtime container khác nhau.

Nó phải được cài đặt riêng và được sử dụng để kiểm tra và gỡ lỗi các runtime container vì vậy một lần nữa không lý tưởng được sử dụng để tạo container không giống như tiện ích **Docker** hoặc **nerdctl** nhưng lại là một công cụ gỡ lỗi. Bạn có thể kỹ thuật tạo container với tiện ích **crictl** nhưng không dễ dàng. Nó chỉ được sử dụng cho một số mục đích gỡ lỗi đặc biệt. Và nhớ rằng nó hoạt động cùng với **kubelet** vì vậy chúng ta biết rằng **kubelet** chịu trách nhiệm đảm bảo một số lượng nhất định container hoặc pod có sẵn trên một node tại một thời điểm, vì vậy nếu bạn đi qua tiện ích **crictl** và cố gắng tạo container với nó, cuối cùng **kubelet** sẽ xóa chúng vì **kubelet** không biết về một số container hoặc pod đó được tạo ra bên ngoài kiến thức của nó vì vậy bất cứ thứ gì nó thấy, nó sẽ đi và xóa nó, vì những lý do đó nhớ rằng tiện ích **crictl** chỉ được sử dụng cho mục đích gỡ lỗi và tiếp cận vào container và tất cả những điều đó.

Hãy xem một số ví dụ về lệnh dòng lệnh vì vậy bạn chỉ cần chạy lệnh **crictl** cho điều này và điều này có thể được sử dụng để thực hiện các hoạt động liên quan đến container cơ bản như kéo hình ảnh, hoặc liệt kê hình ảnh hiện có, liệt kê container, rất giống với lệnh **Docker** khi bạn sử dụng các lệnh PS, vì vậy trong **Docker** bạn chạy lệnh ps, và ở đây bạn chạy lệnh **crictl** ps và để chạy một lệnh trong một container **Docker** nhớ rằng chúng ta sử dụng lệnh exec và nó giống nhau ở đây và cùng với các tùy chọn giống như -i và -t và bạn chỉ định id container. Để xem nhật ký, bạn sử dụng lệnh **crictl** logs, một lần nữa rất giống với lệnh **Docker**.

Một sự khác biệt lớn là lệnh **crictl** cũng biết đến các pod vì vậy bạn có thể liệt kê các pod bằng cách chạy lệnh **crictl** pods vì vậy điều này không phải là điều mà **Docker** biết đến. Vì vậy, khi làm việc với **Kubernetes** trong quá khứ, chúng ta đã sử dụng rất nhiều lệnh **Docker** để khắc phục sự cố container và xem nhật ký đặc biệt là trên các node worker và bây giờ bạn sẽ sử dụng lệnh **crictl** để làm điều đó. Cú pháp rất giống nhau vì vậy nó không thực sự khó. Đây là một biểu đồ liệt kê so sánh giữa công cụ dòng lệnh **Docker** và **crictl**. Như bạn có thể thấy, rất nhiều lệnh như attach exec, images, info, inspect, logs, ps, stats, version, v.v., hoạt động chính xác cùng một cách, và một số lệnh để tạo, xóa và bắt đầu và dừng hình ảnh hoạt động tương tự. Một danh sách đầy đủ các sự khác biệt có thể được tìm thấy trong liên kết này.

Vì vậy, như tôi đã đề cập, **crictl** có thể được sử dụng để kết nối với bất kỳ runtime container nào tương thích với **CRI**, nhớ đặt điểm cuối phù hợp nếu bạn có nhiều runtime container được cấu hình, hoặc nếu bạn muốn **crictl** tương tác với một runtime cụ thể, ví dụ nếu bạn không cấu hình gì mặc định nó sẽ kết nối với các socket theo thứ tự cụ thể này, vì vậy nó sẽ cố gắng kết nối với **dockershim** trước, sau đó là **containerd**, sau đó là **CRI-O**, sau đó là **CRI-dockerd** – đó là thứ tự mà nó tuân theo. Nhưng nếu bạn muốn ghi đè điều đó và

 muốn nó kết nối với **containerd**, bạn có thể chỉ định một điểm cuối, hoặc nếu bạn muốn nó kết nối với **CRI-O**, bạn có thể chỉ định một điểm cuối khác. Vì vậy, điều này chỉ đơn giản là cần thiết khi bạn cấu hình nhiều runtime container, nếu bạn chỉ có một runtime container, bạn không cần phải lo lắng về điều này.

Vì vậy, đó là một cái nhìn tổng quan về những khái niệm cơ bản và công cụ mà bạn cần phải hiểu để có thể làm việc với **containerd** và **CRI** và **Kubernetes**.