---
# FILE: pages/chapter2.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter2.md
# DESC: Chương 2 - Cơ sở Lý thuyết và Công nghệ
layout: chapter
chapterNumber: 2
transition: slide-left
---

# Chương 2

## CƠ SỞ LÝ THUYẾT VÀ CÔNG NGHỆ

Các nền tảng công nghệ, mô hình IDS, SDN, OpenFlow, Ryu, Mininet và Entropy.

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Tổng quan công nghệ</div>

# STACK CÔNG NGHỆ SỬ DỤNG

<div class="card-grid-2 mt-16">
  <div v-click class="feature-card signal-card accent-blue"><div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div><div><h3>SDN và OpenFlow</h3><p>SDN tách Data Plane và Control Plane; OpenFlow là Southbound API cho phép Controller thao tác Flow Table.</p></div></div>
  <div v-click class="feature-card signal-card accent-green"><div class="icon-bubble"><span class="i-twemoji-antenna-bars"></span></div><div><h3>Ryu Controller</h3><p>Framework SDN bằng Python, hỗ trợ RESTful API để truy vấn thống kê luồng định kỳ.</p></div></div>
  <div v-click class="feature-card signal-card accent-cyan"><div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div><div><h3>Mininet</h3><p>Tạo mạng SDN ảo gồm host, switch OpenFlow và Controller trên một máy vật lý.</p></div></div>
  <div v-click class="feature-card signal-card accent-red"><div class="icon-bubble"><span class="i-twemoji-keyboard"></span></div><div><h3>Công cụ tấn công</h3><p>hping3 tạo DDoS flood, nmap dò quét cổng, arpspoof đầu độc bảng ARP.</p></div></div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Ryu SDN Controller</div>

# VÌ SAO CHỌN RYU CHO HỆ THỐNG IDS?

<p class="deck-lead ryu-lead">Ryu cung cấp khả năng lập trình toàn diện bằng Python, giúp tối ưu hóa thu thập dữ liệu thống kê và can thiệp bảo mật tầng mạng.</p>

<div class="card-grid-3 ryu-card-grid">
  <div v-click class="feature-card ryu-card accent-blue"><div class="icon-bubble"><span class="i-mdi-lightning-bolt"></span></div><h3>Cơ chế hướng sự kiện</h3><p>Xử lý tức thời các bản tin <strong>Packet-In</strong> để phát hiện hành vi giả mạo ARP và lưu lượng lạ ngay khi chúng vừa chạm tới Switch.</p></div>
  <div v-click class="feature-card ryu-card accent-green"><div class="icon-bubble"><span class="i-mdi-chip"></span></div><h3>Khai thác OpenFlow 1.3</h3><p>Truy cập trực tiếp vào các <strong>Bộ đếm</strong> của Bảng luồng để lấy dữ liệu thô phục vụ thuật toán tính toán <strong>Shannon Entropy</strong>.</p></div>
  <div v-click class="feature-card ryu-card accent-violet"><div class="icon-bubble"><span class="i-mdi-cogs"></span></div><h3>RESTful API linh hoạt</h3><p>Cho phép IDS thực hiện Polling thống kê định kỳ và đẩy lệnh <strong>Flow-Mod Drop</strong> để cô lập kẻ tấn công một cách tự động.</p></div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Phương thức tấn công</div>

# BA KỸ THUẬT TẤN CÔNG TIÊU BIỂU

<p class="deck-lead attack-lead">Báo cáo tập trung vào các kỹ thuật tấn công tiêu biểu gây cạn kiệt tài nguyên và đe dọa an ninh mạng SDN.</p>

<div class="attack-strip">
  <div v-click class="attack-card accent-red"><div class="attack-card-header"><span class="badge badge-danger">DoS / DDoS</span><div class="icon-bubble"><span class="i-twemoji-high-voltage"></span></div></div><div class="attack-card-body"><h3 style="font-size: 1rem;">TÊ LIỆT BỘ ĐIỀU KHIỂN</h3><ul><li>Gây lỗi <strong>không khớp bảng (Table-miss)</strong> liên tục bằng lưu lượng IP giả mạo.</li><li>Switch đẩy dồn dập <em>Packet-In</em>, vắt kiệt tài nguyên CPU/RAM của Controller.</li></ul></div></div>
  <div v-click class="attack-card accent-blue"><div class="attack-card-header"><span class="badge">Port Scanning</span><div class="icon-bubble"><span class="i-twemoji-magnifying-glass-tilted-left"></span></div></div><div class="attack-card-body"><h3 style="font-size: 1rem;">DÒ QUÉT CỔNG</h3><ul><li>Sinh ra vô số luồng mạng (flows) ngắn hạn.</li><li>Buộc Controller đẩy <em>Flow-Mod</em> rác, làm lấp đầy Bảng luồng (Flow Table).</li></ul></div></div>
  <div v-click class="attack-card accent-amber"><div class="attack-card-header"><span class="badge badge-warning">ARP Spoofing</span><div class="icon-bubble"><span class="i-mdi-incognito"></span></div></div><div class="attack-card-body"><h3 style="font-size: 1rem;">GIẢ MẠO GIAO THỨC PHÂN GIẢI ĐỊA CHỈ</h3><ul><li>Lừa Controller học sai cấu trúc định tuyến ARP.</li><li>Tiếp tay thiết lập mô hình nghe lén <strong>Man-in-the-Middle</strong>.</li></ul></div></div>
</div>

<!--
* DDoS: kẻ tấn công huy động một mạng lưới khổng lồ các thiết bị đã bị chiếm quyền điều khiển (Botnet) nằm rải rác trên toàn cầu để đồng loạt nhắm vào một nạn nhân. Mục đích cuối cùng không phải là đánh cắp dữ liệu, mà là làm cạn kiệt tài nguyên hệ thống, khiến dịch vụ bị ngưng trệ và từ chối phục vụ người dùng hợp lệ.
* Port: Kẻ tấn công gửi các gói tin thăm dò đến một dải rộng các cổng từ 1 đến 65535 trên thiết bị của nạn nhân. Dựa vào các gói tin phản hồi, kẻ tấn công có thể ánh xạ được sơ đồ mạng, xác định trạng thái cổng và phát hiện các dịch vụ đang vận hành để tìm kiếm lỗ hổng khai thác.
* Lỗ hổng của giao thức ARP: Giao thức phân giải địa chỉ (ARP) được dùng để ánh
xạ địa chỉ IP sang địa chỉ MAC (tầng 2) trong mạng LAN nội bộ. Lỗ hổng chí
mạng của ARP là tính chất phi trạng thái và hoàn toàn không có cơ chế xác thực. Một thiết bị trạm sẵn sàng chấp nhận và cập nhật vào bộ đệm của nó bất kỳ gói tin ARP Reply nào nhận được, kể cả khi trước đó nó không hề gửi yêu cầu.
Lợi dụng lỗ hổng trên, kẻ tấn công liên tục phát tán các gói tin
ARP Reply giả mạo vào mạng. Chúng đánh lừa nạn nhân (Victim) rằng địa chỉ MAC
của kẻ tấn công là địa chỉ của Gateway, đồng thời lừa Gateway rằng MAC của kẻ tấn
công là của Victim.
Đòn tấn công này thiết lập thành công mô hình Tấn công xen giữa (Man-in-the-Middle). Toàn bộ lưu lượng thay vì đi thẳng sẽ bị bẻ lái đi qua máy kẻ tấn công, cho phép chúng nghe lén hoặc thay đổi nội dung dữ liệu.
-->

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Thuật toán Shannon Entropy</div>

# ENTROPY ĐO MỨC ĐỘ HỖN LOẠN CỦA LƯU LƯỢNG

<p class="deck-lead">Thuật toán không đo nội dung dữ liệu mà đo mức phân tán/hỗn loạn của đặc trưng mạng trong một cửa sổ quan sát.</p>

<div class="split-board">
  <div v-click class="focus-panel accent-blue">
    <div>
      <span class="badge">Công thức</span>
      <MathFormula expression="H(X) = -\sum_{i=1}^{n} p(x_i)\log_2 p(x_i)" />
      <p class="formula-source">
        Nguồn: Claude E. Shannon, <a href="https://people.math.harvard.edu/~ctm/home/text/others/shannon/entropy/entropy.pdf" target="_blank">A Mathematical Theory of Communication</a>, Bell System Technical Journal, 1948.
      </p>
      <p>Shannon Entropy 𝐻(𝑋) được tính bằng tổng của các xác suất xuất hiện nhân với logarit cơ số 2 của chính xác suất đó.</p>
    </div>
  </div>

  <div class="timeline-panel">
    <div class="formula-explain">
      <h3>Trong đó:</h3>
      <ul>
        <li><strong>𝑋</strong>: Biến ngẫu nhiên cần phân tích.</li>
        <li><strong>𝑛</strong>: Tổng số lượng các giá trị phân biệt của 𝑋 xuất hiện trong mẫu.</li>
        <li>
          <strong>𝑝(𝑥ᵢ)</strong>: Xác suất xuất hiện của phần tử 𝑥ᵢ. Xác suất này được tính toán thực tế bằng công thức
          <span class="inline-formula">𝑝(𝑥ᵢ) = 𝑐ᵢ / 𝑆</span>, với 𝑐ᵢ là số lượng gói tin thuộc về phần tử 𝑥ᵢ, và 𝑆 là tổng số gói tin bắt được trong toàn bộ mẫu.
        </li>
      </ul>
    </div>
  </div>
</div>
