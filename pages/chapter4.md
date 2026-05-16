---
# FILE: pages/chapter4.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter4.md
# DESC: Chương 4 - Triển khai Thực nghiệm và Đánh giá
layout: chapter
chapterNumber: 4
transition: slide-left
---

# Chương 4

## TRIỂN KHAI THỰC NGHIỆM & ĐÁNH GIÁ

Môi trường phát triển, topology, cấu trúc dự án, cài đặt module và kịch bản kiểm thử tự động.

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Cấu hình phần cứng và phần mềm</div>

# MÔI TRƯỜNG PHÁT TRIỂN

<p class="deck-lead">Báo cáo triển khai hệ thống cục bộ, tận dụng ảo hóa của Mininet để mô phỏng toàn bộ mạng SDN.</p>

<div v-click class="card-grid-3">
  <div class="feature-card accent-violet"><div class="icon-bubble"><span class="i-twemoji-penguin"></span></div><h3>Ubuntu 20.04 LTS</h3><p>Môi trường Linux phục vụ ảo hóa mạng và chạy tiến trình thực nghiệm.</p></div>
  <div class="feature-card accent-cyan"><div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div><h3>Mininet 2.3.0</h3><p>Trình giả lập mạng SDN tạo host, switch và link ảo.</p></div>
  <div class="feature-card accent-blue"><div class="icon-bubble"><span class="i-twemoji-antenna-bars"></span></div><h3>Ryu Controller</h3><p>Framework điều khiển mạng dựa trên Python.</p></div>
  <div class="feature-card accent-green"><div class="icon-bubble"><span class="i-twemoji-globe-with-meridians"></span></div><h3>Open vSwitch</h3><p>Switch ảo hỗ trợ OpenFlow 1.3.</p></div>
  <div class="feature-card accent-red"><div class="icon-bubble"><span class="i-twemoji-keyboard"></span></div><h3>hping3 / nmap / arpspoof</h3><p>Bộ công cụ sinh tấn công DDoS, Port Scan và ARP Spoofing.</p></div>
  <div class="feature-card accent-amber"><div class="icon-bubble"><span class="i-twemoji-stopwatch"></span></div><h3>iperf / ping</h3><p>Đo băng thông, độ trễ và kiểm tra kết nối.</p></div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Topology</div>

# THIẾT LẬP SƠ ĐỒ MẠNG THỰC NGHIỆM

<p class="deck-lead">Sơ đồ mạng được thiết kế theo cấu trúc hình sao thông qua script Python sử dụng thư viện API của Mininet.</p>

<div class="split-board topology-board">
  <figure class="report-figure topology-figure">
    <img src="/report-assets/fig-4-1-mininet-topology.png" alt="Sơ đồ kiến trúc mạng thực nghiệm trên Mininet" />
    <figcaption>Sơ đồ kiến trúc mạng thực nghiệm trên Mininet</figcaption>
  </figure>

  <div class="metric-deck">
    <MetricCard icon="i-twemoji-antenna-bars" value="Controller - C0" label="Bộ điều khiển trung tâm" variant="primary" />
    <MetricCard icon="i-twemoji-globe-with-meridians" value="OpenFlow Switch - S1" label="Thiết bị chuyển mạch" variant="accent" />
    <MetricCard icon="i-twemoji-laptop" value="Victim" label="Nhóm Nạn nhân" variant="success" />
    <MetricCard icon="i-twemoji-bust-in-silhouette" value="Benign Hosts" label="Nhóm Người dùng hợp lệ" variant="warning" />
    <MetricCard icon="i-twemoji-high-voltage" value="Attacker" label="Nhóm Kẻ tấn công" variant="danger" />
  </div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Sơ đồ tổ chức tệp tin</div>

# CẤU TRÚC THƯ MỤC DỰ ÁN

<p class="deck-lead">Mã nguồn được tổ chức theo module để tách biệt topology, phát hiện, ngăn chặn, kiểm thử và tiện ích xem topology.</p>

<div class="project-structure-board">
  <div class="project-tree-panel">
    <pre><code>SDN-IDS/
├── README.md
├── alerts.log
├── pyproject.toml
├── src/ <span class="tree-tag">CORE</span>
│   ├── topology.py
│   ├── ids_detector.py
│   ├── arp_monitor.py
│   └── mitigation.py
├── scripts/ <span class="tree-tag">TEST</span>
│   ├── ddos.sh
│   ├── port_scan.sh
│   └── arp_spoofing.sh
├── topology_viewer.py
└── test_ids.py</code></pre>
  </div>

  <div class="module-list">
    <div class="module-card accent-blue"><div class="icon-bubble"><span class="i-mdi-lan"></span></div><div><h3>Topology</h3><p><code>topology.py</code> tạo star topology gồm 1 switch, 1 victim, 5 benign host và 10 attacker.</p></div></div>
    <div class="module-card accent-green"><div class="icon-bubble"><span class="i-mdi-chip"></span></div><div><h3>Core Logic (IDS)</h3><p><code>ids_detector.py</code> lấy Flow Stats, tính Shannon Entropy, phát hiện DDoS và Port Scan.</p></div></div>
    <div class="module-card accent-amber"><div class="icon-bubble"><span class="i-mdi-shield-search"></span></div><div><h3>ARP Monitoring</h3><p><code>arp_monitor.py</code> giám sát Packet-In ARP và đối chiếu bảng TRUSTED MAC-IP.</p></div></div>
    <div class="module-card accent-red"><div class="icon-bubble"><span class="i-mdi-shield-check"></span></div><div><h3>Mitigation Module</h3><p><code>mitigation.py</code> gửi Flow-Mod priority 65535, chặn mặc định trong 300 giây.</p></div></div>
    <div class="module-card accent-violet"><div class="icon-bubble"><span class="i-mdi-chart-line"></span></div><div><h3>Evaluation & Viewer</h3><p><code>test_ids.py</code> tính Precision, Recall, F1; <code>topology_viewer.py</code> xem trạng thái topology.</p></div></div>
  </div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Scripts và dữ liệu</div>

# KỊCH BẢN MÔ PHỎNG TẤN CÔNG VÀ GHI NHẬN KẾT QUẢ

<div class="roadmap-grid">
  <div class="roadmap-item accent-red"><div class="roadmap-code">DDoS</div><div><h3>scripts/ddos.sh</h3><p>Dùng hping3 để thực hiện TCP SYN Flood tốc độ cao.</p></div></div>
  <div class="roadmap-item accent-blue"><div class="roadmap-code">SCAN</div><div><h3>scripts/port_scan.sh</h3><p>Dùng nmap rà quét các cổng từ 1 đến 1000 trên thiết bị mục tiêu.</p></div></div>
  <div class="roadmap-item accent-amber"><div class="roadmap-code">ARP</div><div><h3>scripts/arp_spoofing.sh</h3><p>Dùng arpspoof để đầu độc bảng ARP của máy nạn nhân.</p></div></div>
  <div class="roadmap-item accent-green"><div class="roadmap-code">LOG</div><div><h3>alerts.log</h3><p>Lưu cảnh báo JSON gồm thời gian, loại hình và IP tấn công.</p></div></div>
  <div class="roadmap-item accent-violet"><div class="roadmap-code">TOML</div><div><h3>pyproject.toml</h3><p>Khai báo thư viện phụ thuộc như ryu, requests và eventlet.</p></div></div>
  <div class="roadmap-item accent-cyan"><div class="roadmap-code">JSON</div><div><h3>test_results.json</h3><p>Xuất kết quả đánh giá tự động sau mỗi chu kỳ kiểm thử.</p></div></div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Entropy</div>

# CÔNG THỨC SHANNON ENTROPY

<p class="deck-lead">Hàm tính Entropy chuyển phân bố packet theo IP thành một chỉ số phản ánh mức độ phân tán của lưu lượng.</p>

<CodePanel class="code-panel-large" badge="Entropy" title="calculate_entropy()" tone="blue" caption="Hình 4.2: Hàm tính toán Shannon Entropy trong ids_detector.py">

```python
def calculate_entropy(ip_packet_counts):
    """Tính Shannon Entropy từ phân bố packet theo IP."""
    total = sum(ip_packet_counts.values())
    if total == 0:
        return 0.0

    entropy = 0.0
    for count in ip_packet_counts.values():
        if count <= 0:
            continue
        p = count / total
        entropy -= p * math.log2(p)

    return entropy
```

</CodePanel>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">DDoS Detection</div>

# LOGIC PHÂN LOẠI DoS / DDoS

<p class="deck-lead">Hai hướng Entropy giúp phân biệt DoS tập trung và DDoS phân tán, đồng thời loại trừ bằng chứng Port Scan.</p>

<CodePanel class="code-panel-large" badge="Detection" title="DDoS classification" tone="red" caption="Hình 4.4: Logic Boolean phân loại hình thức tấn công tràn lụt">

```python
# Phân tích Entropy hai chiều từ dữ liệu Cửa sổ trượt
src_entropy = calculate_entropy(aggregated_src)
dst_entropy = calculate_entropy(directional_dst)

# Nhận diện DoS thông thường
is_dos = (
    src_entropy < ENTROPY_THRESHOLD
    and victim_packets >= DDOS_TRAFFIC_VOL
    and not has_scan_evidence
)

# Nhận diện DDoS phân tán
is_distributed_ddos = (
    dst_entropy < DST_ENTROPY_THRESHOLD
    and victim_packets >= DDOS_TRAFFIC_VOL
    and num_attack_sources >= MIN_DDOS_SOURCES
    and not has_scan_evidence
)
```

</CodePanel>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Port Scan</div>

# LOGIC PHÁT HIỆN RÀ QUÉT CỔNG

<p class="deck-lead">Hệ thống kết hợp đếm số cổng đích và phân tích tốc độ gói tin để tránh phụ thuộc hoàn toàn vào khả năng trích xuất tcp_dst của Switch.</p>

<div class="split-board code-explain-board">
  <CodePanel class="code-panel-fit" badge="Scan" title="get_rate_scan_candidates()" tone="blue" caption="Hình 4.6: Khối logic phân tích tốc độ gói tin để phát hiện rà quét mạng">

```python
def get_rate_scan_candidates(
    aggregated_pairs,
    window_seconds,
    protected_ips=None,
):
    """Tìm cặp src -> protected dst có tốc độ giống port scan."""
    if window_seconds is None or window_seconds <= 0:
        return {}
    protected_ips = get_protected_ips(protected_ips)
    candidates = {}
    for (src_ip, dst_ip), packets in aggregated_pairs.items():
        packets = safe_int(packets)
        if src_ip in protected_ips or dst_ip not in protected_ips:
            continue
        pps = packets / window_seconds
        if (
            PORT_SCAN_MIN_PACKETS <= packets
            and PORT_SCAN_PPS_THRESHOLD <= pps <= PORT_SCAN_MAX_PPS
        ):
            candidates[(src_ip, dst_ip)] = {"packets": packets, "pps": pps}
    return candidates
```

  </CodePanel>

  <div class="timeline-panel code-timeline">
    <TimelineStep title="Lớp 1: Port Count" active>Nếu số cổng đích phân biệt vượt PORT_SCAN_THRESHOLD, đánh dấu Port Scan.</TimelineStep>
    <TimelineStep title="Lớp 2: Packet Rate">Dùng cửa sổ trượt để tính PPS từ nguồn đến IP được bảo vệ.</TimelineStep>
    <TimelineStep title="Fallback" last>Nếu Switch không hỗ trợ tcp_dst trong stats, tốc độ gói tin vẫn giúp cảnh báo scan.</TimelineStep>
  </div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">ARP Monitor</div>

# BẢNG MAC-IP TIN CẬY

<p class="deck-lead">Bảng ánh xạ tĩnh là cơ sở để phát hiện một IP hợp lệ bị gắn với MAC không đúng quy hoạch.</p>

<CodePanel class="code-panel-large code-panel-fit" badge="Trusted" title="TRUSTED MAC-IP" tone="amber" caption="Hình 4.7: Bảng MAC-IP tin cậy">

```python
# Bảng MAC-IP tin cậy (từ topology.py)
TRUSTED = {
    "10.0.0.1":  "00:00:00:00:00:01",
    "10.0.0.11": "00:00:00:00:00:11",
    "10.0.0.12": "00:00:00:00:00:12",
    "10.0.0.13": "00:00:00:00:00:13",
    "10.0.0.14": "00:00:00:00:00:14",
    "10.0.0.15": "00:00:00:00:00:15",
    "10.0.0.21": "00:00:00:00:00:21",
    "10.0.0.22": "00:00:00:00:00:22",
    "10.0.0.23": "00:00:00:00:00:23",
    "10.0.0.24": "00:00:00:00:00:24",
    "10.0.0.25": "00:00:00:00:00:25",
    "10.0.0.26": "00:00:00:00:00:26",
    "10.0.0.27": "00:00:00:00:00:27",
    "10.0.0.28": "00:00:00:00:00:28",
    "10.0.0.29": "00:00:00:00:00:29",
    "10.0.0.30": "00:00:00:00:00:30",
}
```

</CodePanel>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">4.3.6. ARP Monitor</div>

# CHỐNG GIẢ MẠO ARP BẰNG CÁCH ĐỐI CHIẾU BẢNG MAC-IP

<p class="deck-lead">Ứng dụng Ryu kiểm tra ARP Packet-In, đối chiếu MAC/IP, rồi phát cảnh báo khi có sai lệch.</p>

<div class="code-slide-grid arp-monitor-grid">
  <CodePanel class="code-panel-fit" badge="Check" title="Đối chiếu MAC-IP" tone="green" caption="Bỏ qua gói ARP hợp lệ đã khớp bảng tin cậy">

```python
def handle_arp(self, pkt):
    arp_pkt = pkt.get_protocol(arp.arp)
    if not arp_pkt:
        return
    src_ip = arp_pkt.src_ip
    src_mac = arp_pkt.src_mac
    # Đối chiếu với danh sách MAC-IP tĩnh đã quy hoạch
    trusted_mac = TRUSTED.get(src_ip)
    attacker_ip = mac_to_ip(src_mac)
    # Nếu khớp hoàn toàn với bảng tin cậy -> Bỏ qua
    if trusted_mac and src_mac.lower() == trusted_mac.lower():
        return
```

  </CodePanel>

  <CodePanel class="code-panel-fit" badge="Alert" title="Tạo cảnh báo" tone="red" caption="Hình 4.8: Logic bảo mật Layer 2 đối chiếu MAC-IP Binding">

```python
    # Trường hợp 1: IP có trong danh sách nhưng sai MAC
    if trusted_mac:
        alert = self.build_alert(
            attack_type="ARP_SPOOFING",
            attacker_ip=attacker_ip,
            claimed_ip=src_ip,
            src_mac=src_mac,
            trusted_mac=trusted_mac,
            message=(
                f"[CẢNH BÁO] Phát hiện ARP Spoofing! "
                f"IP {src_ip} đang bị giả mạo bởi MAC {src_mac}"
            ),
        )
        self.emit_alert(alert)
        return
    # Trường hợp 2: IP lạ, không có trong quy hoạch mạng
    alert = self.build_alert(
        attack_type="ARP_UNKNOWN_BINDING",
        attacker_ip=attacker_ip,
        claimed_ip=src_ip,
        src_mac=src_mac,
        trusted_mac=None,
        message=(
            f"[CẢNH BÁO] Phát hiện ARP Spoofing! "
            f"IP {src_ip} đang bị giả mạo bởi MAC {src_mac}"
        ),
    )
    self.emit_alert(alert)
```

  </CodePanel>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Test-cases</div>

# CÁC KỊCH BẢN KIỂM THỬ

<p class="deck-lead"><code>test_ids.py</code> tự động khởi tạo mạng, kích hoạt mã độc từ máy ảo, thu thập log và đánh giá kết quả.</p>

<div class="roadmap-grid">
  <div class="roadmap-item accent-green"><div class="roadmap-code">TC1</div><div><h3>Benign Traffic</h3><p>Benign host ping Victim Server; kỳ vọng IDS im lặng.</p></div></div>
  <div class="roadmap-item accent-red"><div class="roadmap-code">TC2</div><div><h3>DoS</h3><p>hping3 --flood từ một Attacker; kỳ vọng phát hiện DoS.</p></div></div>
  <div class="roadmap-item accent-red"><div class="roadmap-code">TC3</div><div><h3>Distributed_DoS</h3><p>hping3 --flood từ nhiều Attacker; kỳ vọng phát hiện DDoS.</p></div></div>
  <div class="roadmap-item accent-blue"><div class="roadmap-code">TC4</div><div><h3>Port Scan</h3><p>nmap -p 1-1000 rà nhiều cổng; kỳ vọng cảnh báo Port_Scan.</p></div></div>
  <div class="roadmap-item accent-cyan"><div class="roadmap-code">TC5</div><div><h3>Rate Scan</h3><p>hping3 tạo scan tốc độ cao; kỳ vọng fallback PPS hoạt động.</p></div></div>
  <div class="roadmap-item accent-amber"><div class="roadmap-code">TC6-7</div><div><h3>ARP Spoofing</h3><p>arpspoof với IP giả hoặc IP không tồn tại; kỳ vọng ARP Monitor cảnh báo.</p></div></div>
</div>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Đánh giá hiệu năng</div>

# CƠ CHẾ ĐÁNH GIÁ HIỆU NĂNG

<p class="deck-lead">Sau khi kiểm thử, hệ thống đối chiếu cảnh báo thực tế trong <code>alerts.log</code> với danh sách kỳ vọng và xuất kết quả ra <code>test_results.json</code>.</p>

<div class="card-grid-3">
  <div class="feature-card accent-blue"><div class="icon-bubble"><span class="i-mdi-percent"></span></div><h3>Precision</h3><p>Tỷ lệ cảnh báo đúng trên tổng số cảnh báo mà Controller phát ra.</p></div>
  <div class="feature-card accent-green"><div class="icon-bubble"><span class="i-twemoji-chart-increasing"></span></div><h3>Recall</h3><p>Tỷ lệ các cuộc tấn công bị bắt giữ thành công trên tổng số tấn công thực tế.</p></div>
  <div class="feature-card accent-violet"><div class="icon-bubble"><span class="i-twemoji-bar-chart"></span></div><h3>F1-Score</h3><p>Trung bình điều hòa giữa Precision và Recall, phản ánh độ ổn định thuật toán.</p></div>
</div>

<div class="callout-band accent-green">
  <div class="icon-bubble"><span class="i-twemoji-clipboard"></span></div>
  <p><strong>Kết quả:</strong> Báo cáo nhấn mạnh minh chứng định lượng từ test tự động, không nêu các con số cố định trong phần nội dung trích dẫn.</p>
</div>
