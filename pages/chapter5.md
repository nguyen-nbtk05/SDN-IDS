---
# FILE: pages/chapter5.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter5.md
# DESC: Chương 5 - Kết luận và Hướng phát triển
layout: chapter
chapterNumber: 5
transition: slide-left
---

# CHƯƠNG 5: KẾT LUẬN VÀ HƯỚNG PHÁT TRIỂN

## Kết quả, ưu điểm, hạn chế và định hướng mở rộng

- Tổng hợp các kết quả đạt được của đề tài.
- Đánh giá ưu điểm và hạn chế của mô hình.
- Đề xuất hướng cải tiến cho nghiên cứu và triển khai thực tế.

---
layout: content-card
transition: slide-left
---

# Kết quả đạt được

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Nền tảng và mô hình" compact>

- Nghiên cứu cơ sở lý thuyết về SDN, OpenFlow, bộ điều khiển Ryu và Mininet.
- Xây dựng mô hình IDS dựa trên thống kê luồng và Shannon Entropy.
- Mô phỏng cấu trúc mạng thực nghiệm với nhóm tấn công, nạn nhân và người dùng hợp lệ.

</GlassBox>

<GlassBox title="Phát hiện và ngăn chặn" compact>

- Phát hiện các hành vi bất thường như DDoS, dò quét cổng và giả mạo ARP.
- Triển khai cơ chế ngăn chặn tự động bằng luật loại bỏ trên thiết bị chuyển mạch.
- Ghi nhận cảnh báo phục vụ theo dõi và đánh giá.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Ưu điểm của giải pháp

<div class="divider"></div>

<div style="display:grid;grid-template-columns:repeat(2,1fr);gap:14px;margin-top:14px;">

<GlassBox title="Gọn nhẹ" compact>
Dựa trên thống kê luồng, không cần kiểm tra sâu nội dung gói tin.
</GlassBox>

<GlassBox title="Phù hợp SDN" compact>
Tận dụng điều khiển tập trung và khả năng cài luật từ bộ điều khiển.
</GlassBox>

<GlassBox title="Tự động phản hồi" compact>
Phát hiện bất thường và cài luật loại bỏ mà không cần thao tác thủ công trong mỗi sự kiện.
</GlassBox>

<GlassBox title="Dễ mở rộng" compact>
Triển khai bằng Python trên Ryu, thuận lợi cho bổ sung mô-đun mới và kiểm thử bằng Mininet.
</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Hạn chế của đề tài

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Phạm vi thực nghiệm" compact>

- Kết quả mới được đánh giá trong môi trường mô phỏng.
- Chưa triển khai trên thiết bị SDN vật lý.
- Chưa đánh giá sâu trong mạng quy mô lớn hoặc nhiều bộ điều khiển.

</GlassBox>

<GlassBox title="Giới hạn kỹ thuật" compact>

- Thuật toán dựa trên ngưỡng có thể khó xử lý khi lưu lượng hợp lệ tăng đột biến.
- Chưa bao quát toàn bộ kiểu tấn công trong SDN.
- Hiệu năng thực tế phụ thuộc cấu hình máy mô phỏng và tham số triển khai.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Hướng phát triển

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Nâng cao phát hiện" compact>

- Tối ưu phương pháp chọn ngưỡng theo hướng thích nghi với lưu lượng thực tế.
- Kết hợp Entropy với học máy nhẹ để cải thiện độ chính xác.
- Mở rộng phát hiện sang nhiều kiểu tấn công khác trong SDN.

</GlassBox>

<GlassBox title="Mở rộng triển khai" compact>

- Thử nghiệm trên hệ thống SDN vật lý hoặc môi trường quy mô lớn hơn.
- Xây dựng giao diện giám sát trực quan cho quản trị viên.
- Bổ sung đánh giá định lượng về độ chính xác, độ trễ và cảnh báo nhầm.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Bài học kinh nghiệm

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Kiến thức kỹ thuật" compact>

- Hiểu rõ hơn về kiến trúc SDN và điều khiển bằng OpenFlow.
- Nắm được cách xây dựng môi trường mô phỏng mạng bằng Mininet.
- Biết cách khai thác thống kê luồng để phát hiện bất thường.

</GlassBox>

<GlassBox title="Kinh nghiệm triển khai" compact>

- Rút kinh nghiệm trong lựa chọn ngưỡng và tổ chức dữ liệu theo cửa sổ thời gian.
- Nhận thức rõ vai trò của tự động hóa trong phòng thủ mạng.
- Tăng khả năng tổ chức mô-đun và kiểm thử kịch bản an ninh mạng.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# KẾT LUẬN CHUNG

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Tổng kết đề tài" compact>

- Đề tài đã đề xuất và triển khai mô hình phát hiện, ngăn chặn tấn công mạng trong môi trường SDN.
- Phương pháp thống kê luồng kết hợp Shannon Entropy cho thấy khả năng phát hiện bất thường với chi phí xử lý thấp.
- Cơ chế cài đặt luật loại bỏ qua bộ điều khiển giúp hệ thống phản hồi nhanh trước lưu lượng độc hại.

</GlassBox>

<GlassBox title="Lời kết" compact>

- Kết quả đạt được là nền tảng để mở rộng mô hình trong môi trường mạng thực tế hơn.
- Mô hình có thể tiếp tục cải tiến về ngưỡng phát hiện, giao diện giám sát và đánh giá định lượng.
- Xin chân thành cảm ơn thầy và các bạn đã lắng nghe.

</GlassBox>

</div>
