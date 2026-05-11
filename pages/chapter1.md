---
# FILE: pages/chapter1.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter1.md
# DESC: Chương 1 - Mở đầu và Phạm vi Đề tài
layout: chapter
chapterNumber: 1
transition: slide-left
---

# Chương 1

## Mở đầu và Phạm vi Đề tài

Bối cảnh, mục tiêu, phạm vi và phương pháp nghiên cứu

---
layout: content-card
transition: slide-left
---

# 1.1 LÝ DO CHỌN ĐỀ TÀI

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="BỐI CẢNH THỰC TIỄN" icon="🌐">

<v-clicks>

- Sự bùng nổ của **IoT, Cloud và Big Data** làm tăng lưu lượng mạng.
- **Tấn công mạng** ngày càng tinh vi và quy mô lớn.
- Mạng truyền thống khó mở rộng, quản trị phân tán và phản ứng chậm.

</v-clicks>

</GlassBox>

<GlassBox title="VẤN ĐỀ TỒN TẠI" icon="⚡">

<v-clicks>

- **Các phương pháp truyền thống**: tiêu tốn tài nguyên & phản ứng chậm với mạng băng thông rộng.
- **Thiếu khả năng lập trình** linh hoạt trên thiết bị mạng.

</v-clicks>

</GlassBox>

</div>

<div v-click class="glass-card" style="margin-top: 20px; padding: 12px 16px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-success"></span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);">GIẢI PHÁP BẢO MẬT</strong>
  </div>
  <ul style="font-size:0.75rem; margin: 0; padding-left: 20px; line-height: 1.5;">
    <li>Khắc phục nhược điểm tốn tài nguyên của phương pháp truyền thống.</li>
    <li><strong>Cơ chế:</strong> Thống kê luồng để đo độ "hỗn loạn" của IP/Cổng.</li>
    <li><strong>Lợi ích:</strong> Phát hiện nhanh (DDoS, Port Scan...) mà không cần giải mã gói tin.</li>
  </ul>
</div>

---
layout: content-card
transition: slide-left
---

# 1.2 Mục tiêu đề tài

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-top: 12px;">

<div v-click class="glass-card" style="padding: 14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge">MT-01</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Xây dựng mô hình SDN</strong>
  </div>
  <p style="font-size:0.78rem;">Thiết kế topo mạng SDN bằng Mininet với Ryu Controller, hỗ trợ OpenFlow 1.3</p>
</div>

<div v-click class="glass-card" style="padding: 14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-accent">MT-02</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Module thu thập Flow Statistics</strong>
  </div>
  <p style="font-size:0.78rem;">Polling thống kê luồng mỗi 5 giây qua REST API, tính delta packets chính xác</p>
</div>

<div v-click class="glass-card" style="padding: 14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-success">MT-03</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Phân tích Shannon Entropy</strong>
  </div>
  <p style="font-size:0.78rem;">Phát hiện bất thường khi entropy < 1.0 trên sliding window 20 giây (4 chu kỳ)</p>
</div>

<div v-click class="glass-card" style="padding: 14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-warning">MT-04</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Tự động ngăn chặn</strong>
  </div>
  <p style="font-size:0.78rem;">Drop flow rule tự động khi phát hiện tấn công, thời gian phản ứng < 10 giây</p>
</div>

<div v-click class="glass-card" style="padding: 14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-danger">MT-05</span>
    <strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Đánh giá hiệu năng</strong>
  </div>
  <p style="font-size:0.78rem;">Kiểm thử với SYN Flood (hping3), đạt accuracy ≥ 95%, false positive < 5%</p>
</div>

</div>

---
layout: content-card
transition: slide-left
---

# 1.3 Phạm vi & Giới hạn

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="✅ Phạm vi thực hiện" icon="">

<v-clicks>

- Mô phỏng trên **Mininet** với topo tùy chỉnh (3 switches, 6 hosts)
- Sử dụng **Ryu Controller** (Python) với OpenFlow 1.3
- Phát hiện **SYN Flood DDoS** bằng Shannon Entropy
- Thu thập **Flow Statistics** qua REST API (polling 5s)
- Sliding window **20 giây** (deque 4 samples)
- **Tự động drop** flow rules khi entropy < threshold

</v-clicks>

</GlassBox>

<GlassBox title="❌ Giới hạn đề tài" icon="">

<v-clicks>

- **Không** triển khai trên mạng vật lý thực tế
- **Không** xử lý tấn công phức tạp (Slowloris, DNS Amplification)
- **Không** tích hợp Machine Learning/Deep Learning
- **Không** có giao diện dashboard real-time
- **Chỉ** hỗ trợ single controller (không HA cluster)

</v-clicks>

</GlassBox>

</div>

---
layout: two-cols-custom
transition: slide-left
---

::header::

# 1.4 – 1.5 Đối tượng nghiên cứu & Phương pháp thực hiện

::default::

## 🎯 Đối tượng nghiên cứu

<v-clicks>

- **Quản trị viên mạng** tại doanh nghiệp vừa & nhỏ
- **Sinh viên / Nghiên cứu sinh** chuyên ngành An toàn thông tin
- **Môi trường lab** mô phỏng (Ubuntu 22.04 + Mininet + Ryu)

</v-clicks>

## 🔬 Đối tượng sử dụng

<v-clicks>

- Hệ thống **giám sát an ninh** nội bộ (SOC)
- **Testbed nghiên cứu** tại các trường đại học

</v-clicks>

::right::

## 📋 Phương pháp thực hiện

<TimelineStep title="Nghiên cứu tài liệu" active>
  <p>SDN architecture, OpenFlow protocol, Shannon Entropy</p>
</TimelineStep>

<TimelineStep title="Phân tích yêu cầu" active>
  <p>Xác định functional & non-functional requirements</p>
</TimelineStep>

<TimelineStep title="Thiết kế hệ thống" active>
  <p>Kiến trúc 3-tier, sơ đồ luồng dữ liệu, sequence diagram</p>
</TimelineStep>

<TimelineStep title="Lập trình & Tích hợp" active>
  <p>Python + Ryu Controller + Mininet topology</p>
</TimelineStep>

<TimelineStep title="Kiểm thử & Đánh giá" active :last="true">
  <p>Unit test, integration test, stress test với hping3</p>
</TimelineStep>

