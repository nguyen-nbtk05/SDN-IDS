---
# FILE: pages/chapter1.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter1.md
# DESC: Chương 1 - Mở đầu và Phạm vi Đề tài
layout: chapter
chapterNumber: 1
transition: slide-left
---

# Chương 1

## MỞ ĐẦU VÀ PHẠM VI ĐỀ TÀI


---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Lý do chọn đề tài</div>

# SDN CẦN MỘT CƠ CHẾ BẢO MẬT

<div v-click class="necessity-board">
  <div class="necessity-card necessity-blue">
    <div class="necessity-card-header">
      <div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div>
      <h3>Bối cảnh SDN</h3>
    </div>
    <p>Mạng định nghĩa bằng phần mềm (SDN) tách biệt Control Plane và Data Plane, mang lại sự linh hoạt nhưng tạo ra rủi ro tập trung hóa tại <strong>Bộ điều khiển (Controller)</strong>.</p>
  </div>

  <div class="necessity-card necessity-red">
    <div class="necessity-card-header">
      <div class="icon-bubble"><span class="i-twemoji-locked"></span></div>
      <h3>Vấn đề bảo mật</h3>
    </div>
    <p>Controller trở thành "điểm yếu duy nhất". Các cuộc tấn công DDoS giả mạo IP làm cạn kiệt tài nguyên xử lý của Controller thông qua bản tin Packet-In.</p>
  </div>

  <div class="necessity-card necessity-card-wide necessity-green">
    <div class="necessity-card-header">
      <div class="icon-bubble"><span class="i-mdi-shield-check"></span></div>
      <h3>Giải pháp đề xuất</h3>
    </div>
    <p>Xây dựng hệ thống IDS gọn nhẹ, sử dụng phân tích <strong>Shannon Entropy</strong> để nhận diện bất thường từ thống kê luồng mà không cần giải mã gói tin sâu.</p>
  </div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Mục tiêu đề tài</div>

# MỤC TIÊU NGHIÊN CỨU & TRIỂN KHAI

<p class="deck-lead">Đề tài hướng tới một giải pháp từ lý thuyết đến thực hành để bảo vệ Controller và duy trì khả năng sẵn sàng của mạng SDN.</p>

<div v-click class="process-track">
  <div class="process-step accent-blue"><div class="step-number">01</div><h3>Cơ sở lý thuyết</h3><p>Nghiên cứu SDN, OpenFlow, đặc trưng lưu lượng và toán học Shannon Entropy.</p></div>
  <div class="process-step accent-cyan"><div class="step-number">02</div><h3>Môi trường thực nghiệm</h3><p>Triển khai hạ tầng SDN mô phỏng để tạo lưu lượng thường và tấn công.</p></div>
  <div class="process-step accent-green"><div class="step-number">03</div><h3>Module Giám sát & Phát hiện</h3><p>Thu thập Flow Stats định kỳ và tính Entropy gần thời gian thực.</p></div>
  <div class="process-step accent-red"><div class="step-number">04</div><h3>Cơ chế ngăn chặn tự động</h3><p>Truy vết nguồn tấn công và đẩy luật chặn xuống Data Plane.</p></div>
  <div class="process-step accent-amber"><div class="step-number">05</div><h3>Kiểm thử & Đánh giá hiệu năng</h3><p>Đo thời gian phát hiện, độ chính xác, nhận diện nhầm và băng thông ổn định.</p></div>
</div>
