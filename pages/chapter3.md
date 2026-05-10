---
# FILE: pages/chapter3.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter3.md
# DESC: Chương 3 - Phân tích & Thiết kế Hệ thống
layout: chapter
chapterNumber: 3
transition: slide-left
---

# Chương 3

## Phân tích & Thiết kế Hệ thống

Yêu cầu, use case, kiến trúc và thiết kế dữ liệu

---
layout: content-card
transition: slide-left
---

# 3.1 Phân tích yêu cầu

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="Yêu cầu chức năng" icon="⚙️">

<v-clicks>

- **FR-01**: Thu thập flow statistics từ OpenFlow switches mỗi 5 giây
- **FR-02**: Tính toán delta packets cho từng flow entry
- **FR-03**: Duy trì sliding window 20s (buffer 4 mẫu)
- **FR-04**: Tính Shannon Entropy cho source IP distribution
- **FR-05**: Phát cảnh báo khi entropy < threshold (1.0)
- **FR-06**: Tự động install drop flow rule chặn attacker IP
- **FR-07**: Ghi log đầy đủ (timestamp, entropy, action)

</v-clicks>

</GlassBox>

<GlassBox title="Yêu cầu phi chức năng" icon="📊">

<v-clicks>

- **NFR-01**: Thời gian phát hiện ≤ 10 giây từ khi bắt đầu tấn công
- **NFR-02**: False Positive Rate < 5%
- **NFR-03**: CPU Controller < 30% khi polling
- **NFR-04**: Hỗ trợ ≥ 500 flow entries đồng thời
- **NFR-05**: Uptime 99.9% (trong môi trường lab)
- **NFR-06**: Code tuân thủ PEP 8, docstring đầy đủ

</v-clicks>

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# 3.2 Sơ đồ Use Case

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 10px;">

<div>

```mermaid {scale: 0.55}
graph LR
  Admin((Admin))
  IDS[IDS Module]
  Admin --> |Khởi động hệ thống| IDS
  Admin --> |Cấu hình threshold| IDS
  Admin --> |Xem log/cảnh báo| IDS
  IDS --> |Thu thập flow stats| Switch[OF Switch]
  IDS --> |Tính entropy| Entropy[Entropy Engine]
  IDS --> |Cài đặt drop rule| Switch
  Attacker((Attacker)) --> |SYN Flood| Switch
```

</div>

<div>

<GlassBox title="UC-01: Thu thập Flow Stats" compact>

| Thuộc tính | Mô tả |
|---|---|
| **Actor** | IDS Module (tự động) |
| **Tiền điều kiện** | Controller kết nối Switch |
| **Luồng chính** | 1. Gửi GET request → REST API |
|  | 2. Parse JSON response |
|  | 3. Tính delta packets |
|  | 4. Lưu vào sliding window |
| **Ngoại lệ** | API timeout → retry 3 lần |

</GlassBox>

</div>

</div>

---
layout: content-card
transition: slide-left
---

# 3.3 Thiết kế kiến trúc hệ thống

<div class="divider"></div>

<div style="display: flex; justify-content: center; margin-top: 10px;">
  <SdnTopology :width="500" />
</div>

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-top: 14px;">

<div v-click class="glass-card" style="padding: 10px 14px; text-align: center;">
  <div style="font-size:0.7rem;font-weight:700;color:var(--sdn-primary-light);text-transform:uppercase;letter-spacing:0.05em;">Application Layer</div>
  <p style="font-size:0.7rem;margin-top:4px;">IDS Module + Entropy Engine</p>
</div>

<div v-click class="glass-card" style="padding: 10px 14px; text-align: center;">
  <div style="font-size:0.7rem;font-weight:700;color:var(--sdn-accent-light);text-transform:uppercase;letter-spacing:0.05em;">Control Layer</div>
  <p style="font-size:0.7rem;margin-top:4px;">Ryu Controller + REST API</p>
</div>

<div v-click class="glass-card" style="padding: 10px 14px; text-align: center;">
  <div style="font-size:0.7rem;font-weight:700;color:var(--sdn-success);text-transform:uppercase;letter-spacing:0.05em;">Data Layer</div>
  <p style="font-size:0.7rem;margin-top:4px;">OVS Switches + Hosts (Mininet)</p>
</div>

</div>

---
layout: content-card
transition: slide-left
---

# 3.4 Thiết kế cơ sở dữ liệu

<div class="divider"></div>

<GlassBox title="Schema — Flow Statistics Buffer" icon="🗄️">

| Trường | Kiểu dữ liệu | Mô tả |
|---|---|---|
| `timestamp` | `datetime` | Thời điểm thu thập |
| `dpid` | `int` | Datapath ID của switch |
| `src_ip` | `str` | Source IP address |
| `dst_ip` | `str` | Destination IP address |
| `packet_count` | `int` | Tổng packet count (cumulative) |
| `delta_packets` | `int` | Packets mới trong chu kỳ |
| `byte_count` | `int` | Tổng byte count |
| `entropy` | `float` | Shannon entropy tại thời điểm |
| `is_attack` | `bool` | Flag cảnh báo tấn công |

</GlassBox>

<div v-click style="margin-top: 12px;">

> **Lưu ý:** Hệ thống sử dụng **in-memory buffer** (`collections.deque`) thay vì database truyền thống để đảm bảo tốc độ xử lý real-time. Dữ liệu log được ghi ra file CSV để phân tích sau.

</div>

---
layout: content-card
transition: slide-left
---

# 3.5 – 3.7 Thiết kế luồng dữ liệu & Sequence Diagram

<div class="divider"></div>

<div style="margin-top: 8px;">

```mermaid {scale: 0.55}
sequenceDiagram
    participant IDS as IDS Module
    participant API as Ryu REST API
    participant SW as OVS Switch
    participant E as Entropy Engine

    loop Mỗi 5 giây
        IDS->>API: GET /stats/flow/{dpid}
        API->>SW: OFPFlowStatsRequest
        SW-->>API: FlowStatsReply
        API-->>IDS: JSON Response
        IDS->>IDS: Tính Delta Packets
        IDS->>E: Gửi source IP distribution
        E->>E: Tính Shannon Entropy H(X)
        E-->>IDS: H = 0.45 (< threshold)

        alt H < Threshold
            IDS->>API: POST drop flow rule
            API->>SW: OFPFlowMod (DROP)
            IDS->>IDS: Log ALERT
        else H >= Threshold
            IDS->>IDS: Log NORMAL
        end
    end
```

</div>

