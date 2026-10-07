# PHÂN TÍCH: CHỌN CHIẾN LƯỢC PHÂN RÃ EPIC “HỦY CHUYẾN GHÉP”

**Sản phẩm:** RikkeiGo  
**Epic:** Hủy chuyến ghép  
**Đầu vào:** Các User Story đặt xe ghép ở Bài 3.  
**Mục tiêu:** Cho phép khách hủy chuyến đúng chính sách, bảo đảm tính phí công bằng và cập nhật ngay giá của những khách còn lại.

## Phần 1 – Đề xuất hai chiến lược phân rã

### Chiến lược 1: Phân rã theo lát cắt nghiệp vụ và vòng đời hủy chuyến

Mỗi Feature thể hiện một quy tắc hoặc một luồng nghiệp vụ mà người dùng có thể nhận được giá trị. Các Feature có thể được phát triển và kiểm thử theo từng chính sách hủy cụ thể.

| Mã | Feature |
|---|---|
| F1 | **Hủy trước khi tài xế nhận chuyến** – khách được hủy miễn phí; chuyến ghép được cập nhật cho các khách còn lại. |
| F2 | **Hủy sau khi tài xế nhận chuyến** – khách bị tính phí hủy 10.000đ và nhận thông báo kết quả. |
| F3 | **Tính lại giá chuyến ghép sau khi có khách hủy** – hệ thống phân bổ lại giá cho các khách còn lại và thông báo ngay. |
| F4 | **Xử lý hủy tại thời điểm giao nhau** – dùng thời điểm hệ thống ghi nhận trên máy chủ để quyết định khách hủy trước hay sau lúc tài xế nhận chuyến. |

Đây là hướng phân rã theo **quy tắc và tình huống người dùng**, nên mỗi Feature gắn trực tiếp với một kết quả nghiệp vụ có thể quan sát được.

### Chiến lược 2: Phân rã theo lớp kỹ thuật của hệ thống

Mỗi Feature phụ trách một bộ phận kỹ thuật trong quy trình hủy. Các lớp được hoàn thiện dần rồi kết nối thành luồng đầy đủ.

| Mã | Feature |
|---|---|
| F5 | **Giao diện hủy chuyến** – nút hủy, màn hình xác nhận, hiển thị phí dự kiến và kết quả hủy. |
| F6 | **Dịch vụ xử lý trạng thái chuyến** – nhận yêu cầu hủy, kiểm tra trạng thái nhận chuyến và cập nhật trạng thái. |
| F7 | **Dịch vụ tính phí và tính lại giá** – xác định phí 0đ hoặc 10.000đ, sau đó tính lại giá cho các khách còn lại. |
| F8 | **Dịch vụ thông báo và ghi nhận thời điểm máy chủ** – lưu thời điểm xử lý, phát thông báo và cung cấp dữ liệu cho đối soát. |

Đây là hướng phân rã theo **kiến trúc kỹ thuật**, trong đó giá trị cho khách hàng thường chỉ xuất hiện sau khi nhiều Feature được tích hợp.

## Phần 2 – So sánh theo tiêu chí INVEST

Trong bảng, dấu **+** là ưu điểm và dấu **–** là nhược điểm. “Independent” đánh giá mức độ có thể phát triển tương đối độc lập; “Small” đánh giá kích thước vừa đủ cho một Sprint; “Valuable” đánh giá giá trị có thể chuyển giao cho người dùng hoặc nghiệp vụ.

| Tiêu chí INVEST | Chiến lược 1: Theo lát cắt nghiệp vụ | Chiến lược 2: Theo lớp kỹ thuật |
|---|---|---|
| **Independent** | **+** Mỗi Feature tập trung vào một chính sách hoặc tình huống; có thể ưu tiên F1, F2 trước rồi mở rộng F3, F4. Một số quan hệ dữ liệu giữa các Feature vẫn cần được thống nhất, nhưng ít phụ thuộc vào lớp triển khai cụ thể. | **–** Các lớp phụ thuộc mạnh vào nhau: giao diện cần API, API cần dịch vụ tính phí và trạng thái, thông báo cần kết quả xử lý. Một lớp hoàn thành riêng thường chưa tạo thành luồng sử dụng được. |
| **Small** | **+** Mỗi Feature có phạm vi nghiệp vụ hẹp, dễ tách thành User Story và ước lượng theo Sprint. F4 là trường hợp biên riêng, có thể kiểm thử độc lập bằng thời điểm máy chủ. | **+** Mỗi Feature có ranh giới kỹ thuật tương đối rõ và có thể chia cho các thành viên hoặc nhóm nhỏ. Tuy nhiên, việc tích hợp giữa các lớp có thể làm phát sinh thêm công việc. |
| **Valuable** | **+** Mỗi Feature trả lời một nhu cầu cụ thể: miễn phí, tính phí, cập nhật giá hoặc xử lý tranh chấp thời điểm. Sau mỗi phần, Product Owner có thể kiểm thử một quy tắc có ý nghĩa nghiệp vụ. | **–** Hoàn thành giao diện hoặc một dịch vụ riêng chưa giúp khách hủy chuyến trọn vẹn. Giá trị chỉ rõ ràng sau khi các lớp được ghép lại, nên khó nhận phản hồi sớm từ khách hàng và nghiệp vụ. |

### Nhận xét

Chiến lược 1 phù hợp hơn với INVEST vì tạo ra các lát cắt gần với giá trị người dùng. Chiến lược 2 có ưu điểm trong việc tổ chức phát triển kỹ thuật, nhưng cách chia này dễ tạo ra các phần việc phụ thuộc lẫn nhau và chưa thể hiện giá trị độc lập.

## Phần 3 – Lựa chọn và đặc tả

### Chiến lược được chọn

Chọn **Chiến lược 1 – phân rã theo lát cắt nghiệp vụ và vòng đời hủy chuyến**. Cách này bám trực tiếp vào chính sách tính phí của RikkeiGo, giúp Product Owner kiểm thử từng quy tắc qua các Sprint và nhận phản hồi sớm. Đồng thời, tình huống hủy đúng lúc tài xế nhận chuyến được tách thành một Feature riêng nên có thể kiểm soát rõ căn cứ tính phí.

### User Story

**US-F4: Tính phí theo thời điểm ghi nhận trên máy chủ**

Với vai trò là **khách đang đi một chuyến ghép**,  
tôi muốn **hệ thống xác định phí hủy dựa trên thời điểm máy chủ ghi nhận yêu cầu hủy so với thời điểm máy chủ ghi nhận tài xế nhận chuyến**,  
để **tôi được tính phí nhất quán và công bằng khi hai thao tác xảy ra gần như đồng thời**.

### Acceptance Criteria – kịch bản Given – When – Then

**Kịch bản: Khách bấm hủy đúng lúc tài xế vừa bấm nhận chuyến**

**Given**

- Khách đang ở trong một chuyến ghép và chuyến chưa bị hủy.
- Tài xế bấm **Nhận chuyến**, hệ thống ghi nhận thời điểm máy chủ là `T_driver_accept`.
- Khách bấm **Hủy chuyến**, hệ thống ghi nhận thời điểm máy chủ là `T_customer_cancel`.

**When**

- Hệ thống xử lý đồng thời hai yêu cầu và so sánh `T_customer_cancel` với `T_driver_accept`.

**Then**

- Nếu `T_customer_cancel < T_driver_accept`, hệ thống xác nhận hủy **miễn phí**, tức phí hủy bằng **0đ**.
- Nếu `T_customer_cancel >= T_driver_accept`, hệ thống xác nhận hủy với phí **10.000đ**.
- Hệ thống hiển thị cho khách kết quả hủy và số phí đã áp dụng.
- Sau khi hủy thành công, hệ thống tính lại giá cho các khách còn lại trong chuyến ghép và thông báo ngay giá mới cho họ.
- Kết quả xử lý sử dụng thời điểm ghi nhận trên **máy chủ**, không sử dụng thời điểm hiển thị hoặc thời điểm bấm trên thiết bị của khách hay tài xế.

### Kết luận ngắn

Quy tắc so sánh được viết theo dạng `T_customer_cancel < T_driver_accept` để xác định miễn phí; mọi trường hợp bằng hoặc sau thời điểm tài xế nhận chuyến đều áp dụng phí 10.000đ. Cách viết này loại bỏ sự mơ hồ khi hai thao tác xảy ra cùng lúc và bảo đảm các khách còn lại nhận được giá đã tính lại.
