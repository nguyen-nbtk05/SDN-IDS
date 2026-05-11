---
# FILE: pages/chapter2.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter2.md
# DESC: Chương 2 - Cơ sở Lý thuyết và Công nghệ
layout: chapter
chapterNumber: 2
transition: slide-left
---

# CHƯƠNG 2

## CƠ SỞ LÝ THUYẾT & CÔNG NGHỆ

Nền tảng kỹ thuật, công nghệ sử dụng và nghiên cứu liên quan

---
layout: content-card
transition: slide-left
---

# 2.1 TỔNG QUAN CÔNG NGHỆ SỬ DỤNG

<div class="divider"></div>

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 14px; margin-top: 12px;">

<div v-click class="glass-card" style="padding:14px 16px; text-align:center;">
  <div style="font-size:1.8rem;margin-bottom:6px;"><twemoji-snake /></div>
  <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Python 3.10+</strong>
  <p style="font-size:0.72rem;margin-top:4px;">Ngôn ngữ chính, thư viện phong phú cho networking & data analysis</p>
</div>

<div v-click class="glass-card" style="padding:14px 16px; text-align:center;">
  <div style="font-size:1.8rem;margin-bottom:6px;"><twemoji-control-knobs /></div>
  <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Ryu Controller</strong>
  <p style="font-size:0.72rem;margin-top:4px;">SDN Controller Python-based, hỗ trợ OpenFlow 1.0–1.5, REST API tích hợp</p>
</div>

<div v-click class="glass-card" style="padding:14px 16px; text-align:center;">
  <div style="font-size:1.8rem;margin-bottom:6px;"><twemoji-globe-with-meridians /></div>
  <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Mininet</strong>
  <p style="font-size:0.72rem;margin-top:4px;">Emulator mạng ảo, tạo topo SDN linh hoạt trên single machine</p>
</div>

<div v-click class="glass-card" style="padding:14px 16px; text-align:center;">
  <div style="font-size:1.8rem;margin-bottom:6px;"><twemoji-satellite-antenna /></div>
  <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">OpenFlow 1.3</strong>
  <p style="font-size:0.72rem;margin-top:4px;">Giao thức chuẩn SDN, hỗ trợ multi-table, flow statistics</p>
</div>

<div v-click class="glass-card" style="padding:14px 16px; text-align:center;">
  <div style="font-size:1.8rem;margin-bottom:6px;"><twemoji-hammer /></div>
  <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">hping3</strong>
  <p style="font-size:0.72rem;margin-top:4px;">Công cụ tạo gói tin SYN Flood để kiểm thử tấn công DDoS</p>
</div>

<div v-click class="glass-card" style="padding:14px 16px; text-align:center;">
  <div style="font-size:1.8rem;margin-bottom:6px;"><twemoji-penguin /></div>
  <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Ubuntu 22.04 LTS</strong>
  <p style="font-size:0.72rem;margin-top:4px;">Hệ điều hành triển khai, tương thích tốt với Mininet & Ryu</p>
</div>

</div>

---
layout: content-card
transition: slide-left
---

# 2.2 Khảo sát hệ thống tương tự

<div class="divider"></div>

<div style="margin-top: 12px;">

| Tiêu chí | Snort IDS | Suricata | **Đề tài (SDN-IDS)** |
|---|---|---|---|
| **Kiến trúc** | Inline / Passive | Multi-threaded | SDN-based (centralized) |
| **Phương pháp** | Signature-based | Signature + Protocol | **Entropy + Flow Stats** |
| **Zero-day** | <twemoji-cross-mark /> Không phát hiện | <twemoji-cross-mark /> Hạn chế | <twemoji-white-heavy-check-mark /> Phát hiện anomaly |
| **Tự động chặn** | <twemoji-cross-mark /> Cần thêm IPS | <twemoji-warning /> Partial | <twemoji-white-heavy-check-mark /> **Drop rule tự động** |
| **Thời gian PH** | ~Giây | ~Giây | **< 10 giây** |
| **Programmability** | Hạn chế | Hạn chế | <twemoji-white-heavy-check-mark /> **OpenFlow rules** |

</div>

<div v-click style="margin-top: 16px;">

> **Khoảng trống nghiên cứu:** Các hệ thống IDS hiện tại chủ yếu dựa trên signature, không tận dụng được kiến trúc SDN để phản ứng tự động. Đề tài kết hợp **SDN + Entropy** để giải quyết vấn đề này.

</div>

---
layout: content-card
transition: slide-left
---

# 2.3 Các khái niệm và thuật toán chính

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<div>

## SDN Architecture

<GlassBox compact>

<v-clicks>

- **Application Layer**: IDS module, monitoring apps
- **Control Layer**: Ryu Controller — "bộ não" của mạng
- **Data Layer**: OpenFlow switches (OVS)
- **Northbound API**: REST interface (JSON)
- **Southbound API**: OpenFlow 1.3 protocol

</v-clicks>

</GlassBox>

## Shannon Entropy

<GlassBox compact>

$$H(X) = -\sum_{i=1}^{n} p_i \cdot \log_2(p_i)$$

<v-clicks>

- **H cao** (≈ log₂n): phân bố đồng đều → traffic bình thường
- **H thấp** (→ 0): tập trung vào ít source IP → **DDoS attack**
- **Threshold = 1.0**: ngưỡng phát hiện tấn công

</v-clicks>

</GlassBox>

</div>

<div>

## Flow Statistics

<GlassBox compact>

<v-clicks>

- **packet_count**: số lượng packets khớp flow rule
- **byte_count**: tổng bytes truyền qua
- **duration_sec**: thời gian tồn tại flow entry
- **Delta Packets** = count(t) - count(t-1)
- **Sliding Window**: deque(maxlen=4) × 5s = 20s

</v-clicks>

</GlassBox>

## DDoS Attack Pattern

<GlassBox compact>

<v-clicks>

- **SYN Flood**: Gửi hàng ngàn SYN packets giả mạo source IP
- **Đặc điểm**: 1 target IP, nhiều random source IPs
- **Entropy biểu hiện**: Source IP entropy giảm mạnh
- **Phát hiện**: H(src\_ip) < threshold → Alert

</v-clicks>

</GlassBox>

</div>

</div>

---
layout: content-card
transition: slide-left
---

# 2.4 Tiêu chuẩn & Quy chuẩn áp dụng

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 16px;">

<GlassBox title="Chuẩn giao thức" icon="i-twemoji-satellite-antenna">

<v-clicks>

- **ONF OpenFlow Specification 1.3.5** — giao thức SDN chuẩn
- **RFC 793** — TCP (phân tích SYN flag)
- **RFC 5765** — Security Architecture for IP
- **IEEE 802.1Q** — VLAN tagging (tùy chọn)

</v-clicks>

</GlassBox>

<GlassBox title="Chuẩn phát triển" icon="i-twemoji-wrench">

<v-clicks>

- **PEP 8** — Python coding style guide
- **REST API Design** — JSON response format
- **OWASP Top 10** — Bảo mật ứng dụng web
- **NIST SP 800-94** — Guide to IDS/IPS systems

</v-clicks>

</GlassBox>

</div>

