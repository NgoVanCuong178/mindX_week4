# Scenario 5: Branch B và Branch C (bản soạn)

Đề yêu cầu với **mỗi branch** nộp: step-by-step theo 7 bước, email/chat draft tiếng Việt đúng audience, nội dung update Odoo ngắn có timestamp. Branch A được xử lý thật trên Odoo ([scenario-05-multi-user.md](scenario-05-multi-user.md)). Hai branch dưới đây là phương án nếu kết quả kiểm tra cho dấu hiệu khác. Giờ trong các ghi chú là giờ giả định theo tình huống (sự cố bắt đầu ~19:40).

Bước 1–2 giống nhau ở cả 3 branch, vì lúc đó chưa biết nguyên nhân: tạo ticket chính, gom 2 ticket liên quan, ACK xin 3 thông tin và đưa workaround nhanh (xem Email 1 trong hướng dẫn Branch A). Khác biệt bắt đầu từ bước 3.

---

## Branch B: Vấn đề hệ thống / CDN

### Dấu hiệu (giả định)

- Nhiều lesson và khoá khác cũng bắt đầu lỗi từ ~19:40.
- Monitoring: error rate video endpoint tăng từ 0.4% lên 37%, response time p95 từ 420 ms lên 12 s.
- CDN/gateway: `502/504` tăng đột biến.

### Step-by-step

| Bước | Việc làm |
|---|---|
| 1. Tiếp nhận | Ticket chính + 2 ticket liên quan, Priority ★★. Ghi số người bị ảnh hưởng và thời điểm bắt đầu. |
| 2. Phản hồi | ACK < 15 phút, xin 3 thông tin, workaround nhanh. |
| 3. Chẩn đoán | Monitoring cho thấy lỗi 5xx tăng trên nhiều khoá → sự cố hệ thống. Kiểm tra số ticket mới: nếu > 25 người bị ảnh hưởng thì **nâng lên Expedite**. |
| 4. Giải quyết | **Escalate DevOps/Infra ngay**, không tự xử lý. Workaround: tài liệu PDF, giáo viên chuyển sang hoạt động không cần video. |
| 5. Giao tiếp | **Broadcast** cho nhóm CXO/giáo viên vì ảnh hưởng rộng. Cập nhật mỗi 15–30 phút, kể cả khi chưa có tiến triển. |
| 6. Theo dõi | Sau khi fix: error rate về bình thường chưa, user xác nhận chưa. Theo dõi thêm 30 phút rồi mới đóng. |
| 7. Phân tích | Nguyên nhân gốc từ DevOps, cảnh báo sớm 5xx của CDN, post-mortem nếu ảnh hưởng lớn. |

### Chat cho DevOps/Infra (Teams)

```
@devops @infra 🔴 PRIORITY - Video không phát, CDN 502/504 tăng đột biến
Ticket: #[SỐ]
Tác động: bắt đầu từ 12 học viên JS-ADV-HN-2412, nay nhiều khoá khác cũng lỗi video, số ticket đang tăng
Bắt đầu: ~19:40
Monitoring:
- Error rate video endpoint 0.4% -> 37%
- Response time p95 420 ms -> 12 s
- CDN/gateway 502/504 spike từ 19:40
Log mẫu: GET /media/videos/.../master.m3u8 504 requestId=<...>
Phạm vi: nhiều lesson/khoá, cả web và mobile -> không phải lỗi nội dung hay phía người dùng
Yêu cầu: kiểm tra CDN/origin ngay; support cập nhật CXO/giáo viên mỗi 15-30 phút
```

### Broadcast cho nhóm CXO/giáo viên (nội bộ)

```
[Thông báo từ Tech Support] Sự cố phát video trên LMS

Từ khoảng 19:40, video bài học trên LMS đang phát chậm hoặc không phát được ở nhiều khoá học. Đội kỹ thuật đang xử lý khẩn trương.

Trong lúc chờ, thầy/cô có thể dùng tài liệu PDF của bài học hoặc chuyển sang hoạt động không cần video. Thầy/cô không cần gửi thêm ticket cho cùng lỗi này.

Tech Support sẽ cập nhật trong vòng 30 phút.
```

### Email cập nhật cho CXO

```
Chào chị,

Team cập nhật tình hình video bài 3 lớp JS-ADV-HN-2412:

- Kết quả kiểm tra: lỗi không chỉ ở lớp mình mà ở hệ thống phát video chung, nhiều khoá học khác cũng bị ảnh hưởng. Lỗi không do mạng hay thiết bị của học viên.
- Đội hạ tầng kỹ thuật đã được báo và đang xử lý trực tiếp.
- Trong lúc chờ: lớp có thể dùng tài liệu PDF của bài 3 hoặc chuyển sang hoạt động không cần video.

Team sẽ cập nhật tiếp trong vòng 30 phút, kể cả khi chưa có kết quả mới.

Trân trọng,
Ngô Văn Cương - MindX Tech Support
Ticket #[SỐ]
```

### Email xác nhận giải quyết / follow-up

```
Chào chị,

Đội kỹ thuật đã khắc phục sự cố hệ thống phát video. Team đã theo dõi thêm 30 phút: tỷ lệ lỗi đã về mức bình thường và video bài 3 phát được trên cả máy tính và điện thoại.

Chị giúp team nhờ học viên tải lại trang và báo lại nếu còn ai chưa xem được. Team giữ ticket mở tới khi chị xác nhận.

Trân trọng,
Ngô Văn Cương - MindX Tech Support
Ticket #[SỐ]
```

### Update Odoo (ghi chú, có timestamp)

```
[19:58] Monitoring: error rate video 0.4% -> 37%, p95 12s, CDN 502/504 tăng từ 19:40. Nhiều khoá lỗi -> Branch B (hệ thống/CDN).
[20:00] Escalate @devops @infra qua Teams (đính kèm số liệu). Theo dõi số ticket, nếu > 25 người -> nâng Expedite.
[20:03] Broadcast nhóm CXO/giáo viên. Email cập nhật CXO, hẹn 30 phút.
[20:30] Cập nhật CXO: DevOps đang xử lý, chưa có kết quả mới.
[20:45] DevOps: đã chuyển traffic sang CDN dự phòng. 5xx giảm.
[21:15] Theo dõi 30 phút: error rate 0.5%, p95 450ms. Email xác nhận giải quyết. Chờ CXO xác nhận.
```

---

## Branch C: Nguyên nhân phía người dùng

### Dấu hiệu (giả định)

- Chỉ một nhóm nhỏ user bị (ví dụ 12 học viên dùng cùng extension chặn quảng cáo, hoặc cùng một nhà mạng).
- Mở bằng Incognito thì video phát bình thường.
- Video phát OK trên mobile nhưng lỗi trên desktop (hoặc ngược lại).
- Monitoring và log server bình thường, không có 403/5xx cho video.

### Step-by-step

| Bước | Việc làm |
|---|---|
| 1. Tiếp nhận | Ticket chính + 2 ticket liên quan, Priority ★★ (vẫn 12 người bị chặn việc học). |
| 2. Phản hồi | ACK < 15 phút, xin 3 thông tin, workaround nhanh (Incognito, tắt adblock, đổi trình duyệt). |
| 3. Chẩn đoán | Server không lỗi; user thử Incognito phát được; tìm điểm chung (extension/nhà mạng/trình duyệt) → user-side. |
| 4. Giải quyết | Gửi hướng dẫn user-side. **Không** escalate Dev vì phía hệ thống không lỗi. |
| 5. Giao tiếp | Verify với 1–2 user mẫu trước, rồi gửi hướng dẫn cho cả lớp. |
| 6. Theo dõi | Điều kiện đóng ticket: **user xác nhận OK**. |
| 7. Phân tích | Thêm bài Knowledge hướng dẫn user-side; nếu một extension phổ biến gây lỗi, báo Dev cân nhắc thêm thông báo trên trình phát. |

### Email cập nhật cho CXO (kèm hướng dẫn user-side)

```
Chào chị,

Team đã kiểm tra: hệ thống phát video vẫn hoạt động bình thường, và khi mở bằng cửa sổ ẩn danh thì video bài 3 phát được. Lỗi nhiều khả năng đến từ tiện ích chặn quảng cáo hoặc bộ nhớ đệm của trình duyệt trên máy học viên.

Chị nhờ học viên làm lần lượt, thử lại sau mỗi bước:
1. Tắt tiện ích chặn quảng cáo (AdBlock, uBlock...) cho trang LMS, hoặc mở bằng cửa sổ ẩn danh
2. Xoá bộ nhớ đệm trình duyệt (Ctrl + Shift + Delete -> Hình ảnh và tệp được lưu trong bộ nhớ đệm) rồi tải lại trang
3. Thử trình duyệt khác hoặc đổi sang mạng 4G
4. Nếu vẫn lỗi: đổi DNS sang 8.8.8.8 hoặc 1.1.1.1 (team có thể hướng dẫn chi tiết)

Chị cho team biết kết quả của 1-2 học viên thử trước nhé. Team giữ ticket mở tới khi các học viên xem được video.

Trân trọng,
Ngô Văn Cương - MindX Tech Support
Ticket #[SỐ]
```

### Email xác nhận giải quyết / follow-up

```
Chào chị,

Cảm ơn chị đã xác nhận các học viên đã xem được video bài 3 sau khi tắt tiện ích chặn quảng cáo và xoá bộ nhớ đệm. Team sẽ đóng ticket #[SỐ]. Nếu học viên nào còn gặp lỗi, chị chỉ cần trả lời email này.

Trân trọng,
Ngô Văn Cương - MindX Tech Support
Ticket #[SỐ]
```

### Update Odoo (ghi chú, có timestamp)

```
[19:58] Monitoring/log: video endpoint bình thường, không có 403/5xx cho Lesson 3. Tài khoản test phát được.
[20:02] User mẫu student_jsadv_03: Incognito phát được; 12 user đều dùng extension chặn quảng cáo -> Branch C (user-side). Không escalate Dev.
[20:05] Gửi hướng dẫn user-side cho CXO, nhờ 2 user mẫu thử trước.
[20:20] Verify: student_jsadv_03 (Chrome) và student_jsadv_09 (Safari) phát OK sau khi tắt adblock + xoá cache. Chờ cả lớp xác nhận.
[20:40] CXO xác nhận 12/12 xem được -> đủ điều kiện đóng ticket. Gửi follow-up.
```
