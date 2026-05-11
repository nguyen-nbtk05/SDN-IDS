---
# FILE: pages/chapter1.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter1.md
# DESC: Chương 1 - Mở đầu và Phạm vi Đề tài
layout: chapter
chapterNumber: 1
transition: slide-left
---

# CHƯƠNG 1

## MỞ ĐẦU & PHẠM VI ĐỀ TÀI

Bối cảnh, giải pháp & mục tiêu nghiên cứu

---
layout: content-card
transition: slide-left
---

# 1.1 LÝ DO CHỌN ĐỀ TÀI

<div class="divider"></div>
<div v-click style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="BỐI CẢNH THỰC TIỄN" icon="i-twemoji-globe-with-meridians">

- Sự bùng nổ của **IoT, Cloud và Big Data** làm tăng lưu lượng mạng.
- **Tấn công mạng** ngày càng tinh vi và quy mô lớn.
- Mạng truyền thống khó mở rộng, quản trị phân tán và phản ứng chậm.

</GlassBox>

<GlassBox title="VẤN ĐỀ TỒN TẠI" icon="i-twemoji-high-voltage">

- **Các phương pháp truyền thống**: Tiêu tốn tài nguyên & phản ứng chậm với mạng băng thông rộng.
- **Thiếu khả năng lập trình** linh hoạt trên thiết bị mạng.

</GlassBox>

</div>

<div v-click class="glass-card" style="margin-top: 20px; padding: 20px 16px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:bold;"><twemoji-light-bulb /> GIẢI PHÁP BẢO MẬT </strong>
  </div>
  <ul style="font-size:0.75rem; margin: 0; padding-left: 20px; line-height: 1.5;">
    <li>Phân tích <strong>Entropy</strong> giúp khắc phục nhược điểm tiêu tốn tài nguyên của phương pháp truyền thống.</li>
    <li><strong>Cơ chế:</strong> Thống kê luồng để đo độ "hỗn loạn" của IP/Cổng.</li>
    <li><strong>Lợi ích:</strong> Phát hiện nhanh (DDoS, Port Scan...) mà không cần giải mã gói tin.</li>
  </ul>
</div>

---
layout: content-card
transition: slide-left
---

# 1.2 MỤC TIÊU ĐỀ TÀI

<div class="divider"></div>

<div v-click style="display: grid; grid-template-columns: 1fr 1fr; gap: 12px; margin-top: 12px;">

<div class="glass-card" style="padding: 14px 16px;">
  <div style="display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:6px;">
    <span class="badge">MT-01</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:bold;">NGHIÊN CỨU CƠ SỞ LÝ THUYẾT</strong>
  </div>
  <p style="font-size:0.78rem;text-align:center;">Nghiên cứu SDN/OpenFlow và thuật toán Entropy để đo lường độ tập trung luồng dữ liệu.</p>
</div>

<div class="glass-card" style="padding: 14px 16px;">
  <div style="display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-accent">MT-02</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:bold;">XÂY DỰNG MÔI TRƯỜNG THỰC NGHIỆM</strong>
  </div>
  <p style="font-size:0.78rem;text-align:center;">Thiết lập mạng SDN (Mininet) để giả lập kịch bản mạng và các dạng tấn công.</p>
</div>

<div class="glass-card" style="padding: 14px 16px;">
  <div style="display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-success">MT-03</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:bold;">PHÁT TRIỂN MODULE GIÁM SÁT & PHÁT HIỆN</strong>
  </div>
  <p style="font-size:0.78rem;text-align:center;">Tích hợp ứng dụng trên Controller để thu thập luồng và tính Entropy thời gian thực.</p>
</div>

<div class="glass-card" style="padding: 14px 16px;">
  <div style="display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-warning">MT-04</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:bold;">THIẾT KẾ CƠ CHẾ NGĂN CHẶN TỰ ĐỘNG</strong>
  </div>
  <p style="font-size:0.78rem;text-align:center;">Tự động truy vết và cập nhật luật chặn khi Entropy vượt ngưỡng.</p>
</div>

<div class="glass-card" style="padding: 14px 16px; grid-column: 1 / -1; margin: 0 auto; width: calc(50% - 8px);">
  <div style="display:flex;align-items:center;justify-content:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-danger">MT-05</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:bold;">KIỂM THỬ & ĐÁNH GIÁ HIỆU NĂNG</strong>
  </div>
  <p style="font-size:0.78rem;text-align:center;">Đánh giá độ chính xác, tốc độ phát hiện & khả năng duy trì ổn định hệ thống.</p>
</div>

</div>


