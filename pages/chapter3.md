---
# FILE: pages/chapter3.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter3.md
# DESC: Chương 3 - Phân tích và Thiết kế Hệ thống
layout: chapter
chapterNumber: 3
transition: slide-left
---

# Chương 3

## PHÂN TÍCH & THIẾT KẾ HỆ THỐNG

Yêu cầu, Use Case, kiến trúc module, lưu đồ thuật toán và sequence diagram.

---
layout: two-cols-custom
transition: slide-left
class: requirements-slide
---

::header::

# PHÂN TÍCH YÊU CẦU HỆ THỐNG

::default::

## <span class="i-mdi-cogs"></span> Yêu cầu chức năng

<div v-click="1" style="margin-top: 1.5rem;">

- **Thu thập dữ liệu:** Tự động Polling thống kê định kỳ từ Flow Table qua REST API.
- **Nhận diện bất thường:** Sử dụng Shannon Entropy phát hiện DDoS và Port Scanning.
- **Ngăn chặn chủ động:** Tự động đẩy luật Flow-Mod Drop để cách ly IP độc hại ngay lập tức.
- **Phân tích ARP:** Đối chiếu MAC-IP Binding để chặn đứng các hành vi giả mạo ARP Spoofing.
- **Ghi viết & Cảnh báo:** Lưu nhật ký chi tiết (Thời gian, IP, Loại tấn công, Chỉ số Entropy).

</div>

::right::

## <span class="i-mdi-speedometer"></span> Yêu cầu phi chức năng

<div v-click="1" style="margin-top: 1.5rem;">

- **Độ trễ thấp:** Phản hồi nhanh ở mức mili-giây để bảo vệ Controller khỏi nguy cơ quá tải.
- **Tối ưu tài nguyên:** Giám sát nhẹ nhàng dựa trên thống kê, không sử dụng kỹ thuật DPI nặng nề.
- **Tính trong suốt:** Không chiếm dụng băng thông kênh điều khiển (Southbound Channel).
- **Tính sẵn sàng:** Duy trì băng thông và kết nối ổn định cho lưu lượng người dùng hợp lệ.

</div>

---
layout: content-card
transition: slide-left
class: use-case-slide
---

<div class="deck-kicker">Use Case</div>

# TÁC NHÂN VÀ USE CASE CHÍNH

<p class="deck-lead use-case-lead">Mô hình hóa hệ thống khép kín với 2 tác nhân chính và 5 ca sử dụng tạo thành chu trình phòng thủ.</p>

<div class="split-board use-case-board">
  <div class="actor-grid">
    <div v-click class="feature-card actor-card accent-violet"><div class="icon-bubble"><span class="i-mdi-account-cog"></span></div><h3>Quản trị viên mạng</h3><p>Khởi động/kết thúc ứng dụng IDS trên Ryu Controller và theo dõi nhật ký cảnh báo qua màn hình Console.</p></div>
    <div v-click class="feature-card actor-card accent-blue"><div class="icon-bubble"><span class="i-mdi-server"></span></div><h3>Thiết bị chuyển mạch</h3><p>Nằm ở Data Plane. Cung cấp dữ liệu thống kê luồng và thực thi các luật hủy gói tin do IDS đẩy xuống.</p></div>
  </div>

  <div class="timeline-panel">
    <TimelineStep title="KHỞI ĐỘNG HỆ THỐNG" active>Quản trị viên chạy module IDS trên Ryu Controller để kết nối Switch.</TimelineStep>
    <TimelineStep title="THU THẬP THỐNG KÊ LUỒNG">IDS định kỳ truy vấn OpenFlow Switch để lấy thông tin Bảng luồng.</TimelineStep>
    <TimelineStep title="PHÂN TÍCH VÀ TÍNH ENTROPY">Trích xuất IP nguồn, Cổng đích và tính chỉ số nhận diện bất thường.</TimelineStep>
    <TimelineStep title="THỰC THI NGĂN CHẶN">Tự động sinh luật Drop đẩy xuống Switch để cách ly kẻ tấn công.</TimelineStep>
    <TimelineStep title="CẢNH BÁO VÀ LƯU VẾT" last>Xuất loại tấn công, IP vi phạm và thời gian ra màn hình giám sát.</TimelineStep>
  </div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Sơ đồ Use Case tổng quát</div>

# SƠ ĐỒ USE CASE CỦA HỆ THỐNG IDS

<div class="report-center">
  <figure class="report-figure report-figure-wide">
    <img src="/report-assets/fig-3-1-use-case.png" alt="Sơ đồ Use Case tổng quát của Hệ thống IDS" />
    <figcaption>Hình 3.1: Sơ đồ Use Case tổng quát của Hệ thống IDS</figcaption>
  </figure>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Module chức năng</div>

# CẤU TRÚC MODULE CHỨC NĂNG TRONG HỆ THỐNG IDS

<p class="deck-lead">Ứng dụng IDS được mô-đun hóa để tách rõ lắng nghe sự kiện, thu thập dữ liệu, phân tích bất thường và thực thi ngăn chặn.</p>

<div class="split-board">
  <figure class="report-figure">
    <img src="/report-assets/fig-3-2-ids-modules.png" alt="Sơ đồ kiến trúc các Module chức năng" />
    <figcaption>Sơ đồ kiến trúc các Module chức năng</figcaption>
  </figure>

  <div class="timeline-panel">
    <TimelineStep title="Module Giao tiếp và Lắng nghe sự kiện (Event Listener)" active>Bắt EventOFPPacketIn và EventOFPFlowStatsReply.</TimelineStep>
    <TimelineStep title="Module Thu thập và Trích xuất dữ liệu (Traffic Monitor)">Gửi OFPFlowStatsRequest định kỳ và trích eth_src, ipv4_src, ipv4_dst, tcp_dst/udp_dst.</TimelineStep>
    <TimelineStep title="Module Phân tích và Nhận diện bất thường (Anomaly Detection Engine)">Tính Entropy và kiểm tra ràng buộc MAC-IP cho ARP Spoofing.</TimelineStep>
    <TimelineStep title="Module Thực thi và Cảnh báo (Mitigation & Alerting)" last>Đẩy OFPFlowMod + OFPActionDrop priority cao và in log cảnh báo.</TimelineStep>
  </div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Lưu đồ thuật toán và Logic xử lý</div>

# SƠ ĐỒ THUẬT TOÁN

<div class="report-center">
  <figure class="report-figure report-figure-wide">
    <img src="/report-assets/fig-3-3-flowchart.png" alt="Lưu đồ thuật toán phát hiện và ngăn chặn tấn công" />
    <figcaption>Hình 3.3: Lưu đồ thuật toán phát hiện và ngăn chặn tấn công</figcaption>
  </figure>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Sơ đồ tuần tự (Sequence Diagram)</div>

# KỊCH BẢN GIAO TIẾP THEO THỜI GIAN THỰC

<div class="split-board">
  <figure class="report-figure">
    <img src="/report-assets/fig-3-4-sequence-diagram.png" alt="Biểu đồ tuần tự tương tác bản tin OpenFlow" />
    <figcaption>Biểu đồ tuần tự tương tác bản tin OpenFlow trong kịch bản tấn công</figcaption>
  </figure>

  <div class="timeline-panel">
    <TimelineStep title="Giai đoạn 1: Khởi tạo luồng và Báo cáo gói tin lạ" active>Attacker gửi SYN giả mạo; Switch table-miss và gửi OFPT_PACKET_IN lên Controller.</TimelineStep>
    <TimelineStep title="Giai đoạn 2: Thu thập thống kê chủ động">IDS đến ngưỡng ∆t, gửi OFP_MULTIPART_REQUEST và nhận OFP_MULTIPART_REPLY.</TimelineStep>
    <TimelineStep title="Giai đoạn 3: Phân tích, Phát hiện và Ngăn chặn" last>Controller tính Entropy, xác nhận H > Threshold và đẩy OFPT_FLOW_MOD + OFPActionDrop xuống Switch.</TimelineStep>
  </div>
</div>
