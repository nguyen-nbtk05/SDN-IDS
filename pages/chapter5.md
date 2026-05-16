---
# FILE: pages/chapter5.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter5.md
# DESC: Chương 5 - Kết luận và Hướng phát triển
layout: chapter
chapterNumber: 5
transition: slide-left
---

# Chương 5

## KẾT LUẬN & HƯỚNG PHÁT TRIỂN

Kết quả đạt được, ưu điểm, hạn chế, hướng phát triển và bài học kinh nghiệm.

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Tóm tắt kết quả đạt được</div>

# HỆ THỐNG HOÀN THÀNH MỤC TIÊU BAN ĐẦU

<p class="deck-lead">Báo cáo kết luận đề tài đã hoàn thành mục tiêu ban đầu, xây dựng được hệ thống phát hiện và ngăn chặn tấn công mạng trên nền tảng SDN.</p>

<div class="card-grid-2 results-summary-grid">
  <div class="feature-card signal-card accent-cyan"><div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div><div><h3>Môi trường thực nghiệm</h3><p>Thiết lập Mininet với quy mô nhiều máy trạm, gồm Victim, Benign và Attacker.</p></div></div>
  <div class="feature-card signal-card accent-blue"><div class="icon-bubble"><span class="i-twemoji-antenna-bars"></span></div><div><h3>Controller thông minh</h3><p>Triển khai ứng dụng Ryu Controller tích hợp đa module bảo mật.</p></div></div>
  <div class="feature-card signal-card accent-green"><div class="icon-bubble"><span class="i-twemoji-bar-chart"></span></div><div><h3>Entropy + Sliding Window</h3><p>Nhận diện DDoS và DoS cường độ cao bằng Shannon Entropy trên cửa sổ trượt.</p></div></div>
  <div class="feature-card signal-card accent-red"><div class="icon-bubble"><span class="i-mdi-shield-check"></span></div><div><h3>Phòng thủ đa lớp</h3><p>Chặn DDoS/DoS, Port Scan và ARP Spoofing bằng Entropy, phân tích tốc độ gói tin và MAC-IP Binding.</p></div></div>
  <div class="feature-card signal-card accent-violet"><div class="icon-bubble"><span class="i-twemoji-test-tube"></span></div><div><h3>Đánh giá tự động</h3><p><code>test_ids.py</code> đo Precision, Recall và F1-Score, xuất kết quả ra <code>test_results.json</code>.</p></div></div>
  <div class="feature-card signal-card accent-amber"><div class="icon-bubble"><span class="i-twemoji-clipboard"></span></div><div><h3>Log cảnh báo</h3><p>Các sự kiện tấn công được ghi nhận trong <code>alerts.log</code> để phục vụ đối chiếu và đánh giá.</p></div></div>
</div>

---
layout: two-cols-custom
transition: slide-left
---

::header::

# ƯƯ ĐIỂM & HẠN CHẾ CỦA HỆ THỐNG

::default::

## ƯU ĐIỂM

<div v-click>

- Kiến trúc tối ưu: tách logic tính toán khỏi lõi Controller, giữ băng thông điều khiển ổn định.
- Độ chính xác cao hơn cho Port Scan nhờ cơ chế phát hiện kép: cổng đích và tốc độ gói tin.
- Có tính trực quan nhờ `topology_viewer.py` giúp quản trị viên theo dõi topology và trạng thái.
- Quy trình phát hiện, ghi log và rào chắn được tự động hóa, giảm can thiệp thủ công.

</div>

::right::

## HẠN CHẾ

<div v-click>

- Ngưỡng Entropy và PPS hiện còn tĩnh, có thể cần điều chỉnh khi quy mô lưu lượng thay đổi.
- Hệ thống mới kiểm thử trên Mininet, chưa đánh giá trên Switch vật lý hỗ trợ OpenFlow.

</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Hướng phát triển</div>

# HƯỚNG PHÁT TRIỂN TRONG TƯƠNG LAI

<p class="deck-lead">Báo cáo đề xuất nâng cấp hệ thống theo hướng thích nghi hơn, trực quan hơn và gần môi trường doanh nghiệp hơn.</p>

<div class="roadmap-grid">
  <div class="roadmap-item accent-green"><div class="roadmap-code">AI</div><div><h3>Tích hợp học máy</h3><p>Tự động học và điều chỉnh ngưỡng phát hiện, thích ứng với biến thể tấn công mới.</p></div></div>
  <div class="roadmap-item accent-red"><div class="roadmap-code">L7</div><div><h3>Mở rộng phạm vi bảo mật</h3><p>Phát triển module phát hiện tấn công tầng ứng dụng như SQL Injection hoặc DNS Amplification.</p></div></div>
  <div class="roadmap-item accent-blue"><div class="roadmap-code">UI</div><div><h3>Dashboard Web</h3><p>Quản lý luật chặn và xem biểu đồ thống kê lưu lượng theo thời gian thực.</p></div></div>
  <div class="roadmap-item accent-amber"><div class="roadmap-code">HW</div><div><h3>Triển khai thực tế</h3><p>Thử nghiệm trên hạ tầng doanh nghiệp với Switch hỗ trợ SDN.</p></div></div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Bài học kinh nghiệm</div>

# BÀI HỌC KINH NGHIỆM TỪ DỰ ÁN

<p class="deck-lead">Đề tài mang lại kinh nghiệm cả về kiến thức mạng, tư duy lập trình thời gian thực và phương pháp tổ chức dự án.</p>

<div class="card-grid-3">
  <div class="feature-card accent-blue"><div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div><h3>Kiến thức chuyên môn</h3><p>Hiểu sâu kiến trúc SDN, giao thức OpenFlow và vận hành Controller bằng Python.</p></div>
  <div class="feature-card accent-green"><div class="icon-bubble"><span class="i-mdi-code-tags"></span></div><h3>Tư duy lập trình</h3><p>Làm quen xử lý dữ liệu lớn theo thời gian thực và tối ưu cấu trúc dữ liệu.</p></div>
  <div class="feature-card accent-amber"><div class="icon-bubble"><span class="i-twemoji-white-heavy-check-mark"></span></div><h3>Phương pháp làm việc</h3><p>Tổ chức dự án chuyên nghiệp và xây dựng kịch bản kiểm thử tự động để đảm bảo khách quan.</p></div>
</div>
