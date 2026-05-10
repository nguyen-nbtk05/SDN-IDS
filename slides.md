---
theme: default
title: Phát hiện và Ngăn chặn Tấn công DDoS trên SDN
author: Nhóm Nghiên Cứu
highlighter: shiki
transition: slide-left
lineNumbers: true
colorSchema: light
fonts:
  sans: Inter
  serif: Playfair Display
  mono: JetBrains Mono
  provider: google
defaults:
  transition: fade
---

---
layout: cover-custom
---

::particles::

<NetworkParticles />

::default::

<div class="slide-up">
  <div style="font-size: 0.9rem; text-transform: uppercase; letter-spacing: 4px; color: rgba(232,145,58,0.9); margin-bottom: 1rem; font-family: Inter, sans-serif; font-weight: 600;">
    Đồ án Tốt nghiệp — 2026
  </div>
  <h1 style="font-family: 'Playfair Display', serif; font-size: 2.4rem; font-weight: 700; color: #fff; line-height: 1.2; margin-bottom: 0.8rem;">
    Phát hiện & Ngăn chặn Tấn công Mạng<br/>
    <span style="color: #E8913A;">trên Mô hình SDN</span>
  </h1>
  <p style="font-size: 1rem; color: rgba(255,255,255,0.6); max-width: 600px; margin: 0 auto 1.5rem; line-height: 1.6;">
    Sử dụng Thống kê Luồng & Phân tích Shannon Entropy
  </p>
  <div style="display: flex; gap: 2rem; justify-content: center; font-size: 0.8rem; color: rgba(255,255,255,0.45); font-family: Inter, sans-serif;">
    <span>👨‍🎓 GVHD: TS. Nguyễn Văn A</span>
    <span>📅 Tháng 5, 2026</span>
  </div>
</div>

---

## 📋 Mục lục

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; margin-top: 1.5rem;">

<div v-click style="background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 20px; border-left: 4px solid #1B3A5C;">
  <div style="font-size: 1.3rem; margin-bottom: 6px;">🌐</div>
  <div style="font-weight: 600; color: #1B3A5C; font-size: 0.95rem;">Bối cảnh vấn đề</div>
  <div style="font-size: 0.75rem; color: #6C7A89; margin-top: 4px;">Thực trạng DDoS hiện nay</div>
</div>

<div v-click style="background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 20px; border-left: 4px solid #2A5580;">
  <div style="font-size: 1.3rem; margin-bottom: 6px;">🏗️</div>
  <div style="font-weight: 600; color: #1B3A5C; font-size: 0.95rem;">Kiến trúc SDN</div>
  <div style="font-size: 0.75rem; color: #6C7A89; margin-top: 4px;">3 tầng Application-Control-Data</div>
</div>

<div v-click style="background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 20px; border-left: 4px solid #E8913A;">
  <div style="font-size: 1.3rem; margin-bottom: 6px;">📐</div>
  <div style="font-weight: 600; color: #1B3A5C; font-size: 0.95rem;">Phân tích Entropy</div>
  <div style="font-size: 0.75rem; color: #6C7A89; margin-top: 4px;">Công thức Shannon & ngưỡng</div>
</div>

<div v-click style="background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 20px; border-left: 4px solid #28A745;">
  <div style="font-size: 1.3rem; margin-bottom: 6px;">💻</div>
  <div style="font-weight: 600; color: #1B3A5C; font-size: 0.95rem;">Code Controller</div>
  <div style="font-size: 0.75rem; color: #6C7A89; margin-top: 4px;">Ryu + Python demo</div>
</div>

<div v-click style="background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 20px; border-left: 4px solid #DC3545;">
  <div style="font-size: 1.3rem; margin-bottom: 6px;">🧪</div>
  <div style="font-weight: 600; color: #1B3A5C; font-size: 0.95rem;">Demo Thực nghiệm</div>
  <div style="font-size: 0.75rem; color: #6C7A89; margin-top: 4px;">Mô phỏng tấn công trực tiếp</div>
</div>

<div v-click style="background: #fff; border: 1px solid #E2E8F0; border-radius: 12px; padding: 20px; border-left: 4px solid #6C7A89;">
  <div style="font-size: 1.3rem; margin-bottom: 6px;">📊</div>
  <div style="font-weight: 600; color: #1B3A5C; font-size: 0.95rem;">Kết quả & Hướng phát triển</div>
  <div style="font-size: 0.75rem; color: #6C7A89; margin-top: 4px;">Đánh giá hiệu năng hệ thống</div>
</div>

</div>

---
layout: two-cols
---

## 🌐 Bối cảnh Vấn đề

<v-clicks>

- **DDoS** là mối đe dọa nghiêm trọng nhất với hạ tầng mạng
- Năm 2025: **15.4 triệu** cuộc tấn công DDoS toàn cầu
- Thiệt hại trung bình: **$218,000 / giờ** ngừng hoạt động
- Tấn công ngày càng **tinh vi**, phân tán, đa vector
- Mạng truyền thống **thiếu khả năng** phản ứng tự động

</v-clicks>

<div v-click style="margin-top: 1rem; padding: 14px; background: rgba(220,53,69,0.06); border-radius: 10px; border-left: 3px solid #DC3545;">
  <span style="font-weight: 600; color: #DC3545;">⚡ Thách thức:</span>
  <span style="color: #6C7A89; font-size: 0.85rem;"> Phát hiện nhanh + Phản ứng tự động = Yêu cầu kiến trúc mạng mới</span>
</div>

::right::

<div style="display: flex; flex-direction: column; align-items: center; justify-content: center; height: 100%; padding-left: 1.5rem;">

<div v-click style="width: 100%;">
  <svg viewBox="0 0 300 200" xmlns="http://www.w3.org/2000/svg" style="width: 100%;">
    <rect x="0" y="0" width="300" height="200" rx="12" fill="#1B3A5C" fill-opacity="0.03" stroke="#E2E8F0"/>
    <text x="150" y="25" text-anchor="middle" font-size="11" fill="#1B3A5C" font-weight="600" font-family="Inter,sans-serif">Xu hướng tấn công DDoS (2020–2025)</text>
    <!-- Bars -->
    <rect x="30" y="130" width="30" height="40" rx="3" fill="#2A5580" opacity="0.6"/><text x="45" y="180" text-anchor="middle" font-size="8" fill="#6C7A89">2020</text><text x="45" y="126" text-anchor="middle" font-size="8" fill="#1B3A5C" font-weight="600">5.4M</text>
    <rect x="75" y="115" width="30" height="55" rx="3" fill="#2A5580" opacity="0.7"/><text x="90" y="180" text-anchor="middle" font-size="8" fill="#6C7A89">2021</text><text x="90" y="111" text-anchor="middle" font-size="8" fill="#1B3A5C" font-weight="600">7.9M</text>
    <rect x="120" y="100" width="30" height="70" rx="3" fill="#2A5580" opacity="0.75"/><text x="135" y="180" text-anchor="middle" font-size="8" fill="#6C7A89">2022</text><text x="135" y="96" text-anchor="middle" font-size="8" fill="#1B3A5C" font-weight="600">9.7M</text>
    <rect x="165" y="85" width="30" height="85" rx="3" fill="#E8913A" opacity="0.8"/><text x="180" y="180" text-anchor="middle" font-size="8" fill="#6C7A89">2023</text><text x="180" y="81" text-anchor="middle" font-size="8" fill="#1B3A5C" font-weight="600">11.8M</text>
    <rect x="210" y="70" width="30" height="100" rx="3" fill="#E8913A" opacity="0.85"/><text x="225" y="180" text-anchor="middle" font-size="8" fill="#6C7A89">2024</text><text x="225" y="66" text-anchor="middle" font-size="8" fill="#1B3A5C" font-weight="600">13.1M</text>
    <rect x="255" y="50" width="30" height="120" rx="3" fill="#DC3545" opacity="0.9"/><text x="270" y="180" text-anchor="middle" font-size="8" fill="#6C7A89">2025</text><text x="270" y="46" text-anchor="middle" font-size="8" fill="#DC3545" font-weight="700">15.4M</text>
  </svg>
</div>

</div>

---

## 🏗️ Kiến trúc Mạng SDN

<div v-click style="margin-bottom: 0.5rem;">
  <p style="font-size: 0.85rem; color: #6C7A89; max-width: 700px;">
    <strong>Software-Defined Networking</strong> tách biệt Control Plane & Data Plane, cho phép lập trình logic mạng tập trung tại Controller.
  </p>
</div>

<div v-click>
  <SdnTopology />
</div>

<div v-click style="display: flex; gap: 1rem; justify-content: center; margin-top: 0.5rem;">
  <div style="display: flex; align-items: center; gap: 6px; font-size: 0.75rem; color: #6C7A89;">
    <span style="width: 12px; height: 12px; background: #2A5580; border-radius: 3px; display: inline-block;"></span> Application
  </div>
  <div style="display: flex; align-items: center; gap: 6px; font-size: 0.75rem; color: #6C7A89;">
    <span style="width: 12px; height: 12px; background: #E8913A; border-radius: 3px; display: inline-block;"></span> Control
  </div>
  <div style="display: flex; align-items: center; gap: 6px; font-size: 0.75rem; color: #6C7A89;">
    <span style="width: 12px; height: 12px; background: #1B3A5C; border-radius: 3px; display: inline-block;"></span> Data / Infrastructure
  </div>
</div>

---

## 📐 Phân tích Shannon Entropy

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 2rem; align-items: start;">

<div>

<div v-click>

**Công thức Shannon Entropy:**

$$
H(X) = -\sum_{i=1}^{n} p_i \cdot \log_2(p_i)
$$

Trong đó $p_i$ là xác suất xuất hiện của Source IP thứ $i$ trong cửa sổ quan sát.

</div>

<div v-click style="margin-top: 1rem;">

| Trạng thái | Entropy | Ý nghĩa |
|---|---|---|
| 🟢 Bình thường | $H > 2.5$ | Phân bố đa dạng |
| 🔴 Tấn công | $H < 2.5$ | Tập trung bất thường |

</div>

<div v-click style="margin-top: 1rem; padding: 12px; background: rgba(40,167,69,0.06); border-radius: 8px; border-left: 3px solid #28A745; font-size: 0.8rem; color: #6C7A89;">
  <strong style="color: #28A745;">Cơ chế:</strong> Khi DDoS xảy ra, lưu lượng tập trung từ ít IP → Entropy giảm mạnh dưới ngưỡng θ → Cảnh báo.
</div>

</div>

<div v-click>
  <EntropyChart />
</div>

</div>

---

## 💻 Code Demo — Ryu Controller

<div style="background: #1E1E2E; border-radius: 12px; overflow: hidden; box-shadow: 0 8px 32px rgba(0,0,0,0.2);">
  <div style="background: #181825; padding: 8px 16px; display: flex; align-items: center; gap: 8px;">
    <span style="width: 12px; height: 12px; border-radius: 50%; background: #F38BA8;"></span>
    <span style="width: 12px; height: 12px; border-radius: 50%; background: #FAB387;"></span>
    <span style="width: 12px; height: 12px; border-radius: 50%; background: #A6E3A1;"></span>
    <span style="flex: 1; text-align: center; font-size: 0.7rem; color: #6C7093; font-family: 'JetBrains Mono', monospace;">ids_detector.py — Ryu SDN Controller</span>
  </div>
</div>

```python {3-5|8-14|16-24|26-31|all}{lines:true, maxHeight:'320px'}
import math
from collections import Counter
from ryu.base import app_manager
from ryu.controller import ofp_event
from ryu.controller.handler import MAIN_DISPATCHER, set_ev_cls

ENTROPY_THRESHOLD = 2.5
WINDOW_SIZE = 100  # packets in sliding window

class DDoSDetector(app_manager.RyuApp):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.packet_window = []
        self.blocked_ips = set()

    def calc_shannon_entropy(self, ip_list):
        """Tính Shannon Entropy từ danh sách Source IP."""
        counter = Counter(ip_list)
        total = len(ip_list)
        entropy = 0.0
        for count in counter.values():
            p = count / total
            entropy -= p * math.log2(p)
        return entropy

    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    def packet_in_handler(self, ev):
        src_ip = self._extract_src_ip(ev.msg)
        if not src_ip:
            return
        self.packet_window.append(src_ip)
        if len(self.packet_window) > WINDOW_SIZE:
            self.packet_window.pop(0)

        if len(self.packet_window) >= WINDOW_SIZE:
            entropy = self.calc_shannon_entropy(self.packet_window)
            if entropy < ENTROPY_THRESHOLD:
                top_ip = Counter(self.packet_window).most_common(1)[0][0]
                self.logger.warning(
                    f"⚠ DDoS DETECTED! H={entropy:.2f} "
                    f"< {ENTROPY_THRESHOLD} | Blocking {top_ip}")
                self._install_drop_flow(ev.msg.datapath, top_ip)
                self.blocked_ips.add(top_ip)
```

---

## 🧪 Demo Thực nghiệm

<script setup>
import { ref } from 'vue'
const attackMode = ref(false)
</script>

<div style="text-align: center; margin-bottom: 1rem;">
  <button
    @click="attackMode = !attackMode"
    :style="{
      padding: '10px 28px',
      borderRadius: '8px',
      border: 'none',
      fontFamily: 'Inter, sans-serif',
      fontWeight: 600,
      fontSize: '0.85rem',
      cursor: 'pointer',
      transition: 'all 0.3s ease',
      background: attackMode ? '#DC3545' : '#1B3A5C',
      color: '#fff',
      boxShadow: attackMode ? '0 4px 16px rgba(220,53,69,0.3)' : '0 4px 16px rgba(27,58,92,0.2)',
    }"
  >
    {{ attackMode ? '🛑 Stop Attack' : '▶ Run Attack Demo' }}
  </button>
</div>

<SdnTopology :highlightAttack="attackMode" />

<div v-if="attackMode" style="text-align: center; margin-top: 0.8rem; padding: 10px; background: rgba(220,53,69,0.06); border-radius: 8px;">
  <span style="color: #DC3545; font-weight: 600; font-size: 0.85rem;">🚨 Attacker đang gửi SYN Flood → OVS 1 → Controller phát hiện bất thường Entropy</span>
</div>

---

## 📊 Kết quả Đánh giá

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 24px; margin-top: 2.5rem;">

<div v-click>
  <MetricCard icon="🎯" label="Detection Rate" :value="97.3" suffix="%" color="#28A745" />
</div>

<div v-click>
  <MetricCard icon="⚡" label="False Positive" :value="1.2" suffix="%" color="#E8913A" />
</div>

<div v-click>
  <MetricCard icon="⏱️" label="Response Time" :value="230" suffix="ms" color="#1B3A5C" />
</div>

</div>

<div v-click style="margin-top: 2rem; padding: 16px; background: #fff; border-radius: 12px; border: 1px solid #E2E8F0; display: flex; gap: 2rem; justify-content: center;">
  <div style="text-align: center;">
    <div style="font-size: 0.75rem; color: #6C7A89; text-transform: uppercase; letter-spacing: 0.5px;">Topology</div>
    <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">4 Switch, 8 Host</div>
  </div>
  <div style="text-align: center;">
    <div style="font-size: 0.75rem; color: #6C7A89; text-transform: uppercase; letter-spacing: 0.5px;">Controller</div>
    <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">Ryu + OpenFlow 1.3</div>
  </div>
  <div style="text-align: center;">
    <div style="font-size: 0.75rem; color: #6C7A89; text-transform: uppercase; letter-spacing: 0.5px;">Attack Tool</div>
    <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">hping3 SYN Flood</div>
  </div>
</div>

---

## 🔮 Hướng Phát triển

<div style="position: relative; padding-left: 40px; margin-top: 1.5rem;">

  <!-- Timeline line -->
  <div style="position: absolute; left: 15px; top: 0; bottom: 0; width: 2px; background: linear-gradient(180deg, #1B3A5C, #E8913A, #28A745);"></div>

  <div v-click style="position: relative; margin-bottom: 1.8rem;">
    <div style="position: absolute; left: -33px; top: 2px; width: 12px; height: 12px; border-radius: 50%; background: #1B3A5C; border: 3px solid #FAFAF7; box-shadow: 0 0 0 2px #1B3A5C;"></div>
    <div style="background: #fff; border: 1px solid #E2E8F0; border-radius: 10px; padding: 14px 18px;">
      <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">🤖 Tích hợp Machine Learning</div>
      <div style="font-size: 0.8rem; color: #6C7A89; margin-top: 4px;">Kết hợp Entropy với mô hình Random Forest / LSTM để tăng độ chính xác phân loại tấn công đa vector.</div>
    </div>
  </div>

  <div v-click style="position: relative; margin-bottom: 1.8rem;">
    <div style="position: absolute; left: -33px; top: 2px; width: 12px; height: 12px; border-radius: 50%; background: #2A5580; border: 3px solid #FAFAF7; box-shadow: 0 0 0 2px #2A5580;"></div>
    <div style="background: #fff; border: 1px solid #E2E8F0; border-radius: 10px; padding: 14px 18px;">
      <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">🌐 Multi-Controller Architecture</div>
      <div style="font-size: 0.8rem; color: #6C7A89; margin-top: 4px;">Triển khai hệ thống phân tán với nhiều Controller để tăng khả năng chịu tải và tính sẵn sàng cao.</div>
    </div>
  </div>

  <div v-click style="position: relative; margin-bottom: 1.8rem;">
    <div style="position: absolute; left: -33px; top: 2px; width: 12px; height: 12px; border-radius: 50%; background: #E8913A; border: 3px solid #FAFAF7; box-shadow: 0 0 0 2px #E8913A;"></div>
    <div style="background: #fff; border: 1px solid #E2E8F0; border-radius: 10px; padding: 14px 18px;">
      <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">☁️ Triển khai trên Cloud</div>
      <div style="font-size: 0.8rem; color: #6C7A89; margin-top: 4px;">Đóng gói Docker + Kubernetes, triển khai trên AWS/GCP cho môi trường production thực tế.</div>
    </div>
  </div>

  <div v-click style="position: relative;">
    <div style="position: absolute; left: -33px; top: 2px; width: 12px; height: 12px; border-radius: 50%; background: #28A745; border: 3px solid #FAFAF7; box-shadow: 0 0 0 2px #28A745;"></div>
    <div style="background: #fff; border: 1px solid #E2E8F0; border-radius: 10px; padding: 14px 18px;">
      <div style="font-weight: 600; color: #1B3A5C; font-size: 0.9rem;">📊 Dashboard Real-time</div>
      <div style="font-size: 0.8rem; color: #6C7A89; margin-top: 4px;">Xây dựng giao diện giám sát trực quan với WebSocket, hiển thị Entropy và trạng thái mạng theo thời gian thực.</div>
    </div>
  </div>

</div>

---
layout: center
---

<div style="text-align: center;">
  <div style="font-size: 3rem; margin-bottom: 1rem;">🎓</div>
  <h1 style="font-family: 'Playfair Display', serif; color: #1B3A5C; font-size: 2.5rem; margin-bottom: 0.5rem;">
    Cảm ơn đã lắng nghe!
  </h1>
  <div style="width: 60px; height: 3px; background: #E8913A; border-radius: 2px; margin: 1rem auto;"></div>
  <p style="font-size: 1.1rem; color: #6C7A89; margin-bottom: 2rem;">
    Rất mong nhận được câu hỏi và góp ý từ Hội đồng
  </p>
  <div style="display: flex; gap: 2rem; justify-content: center; align-items: center;">
    <div style="padding: 16px 28px; background: #1B3A5C; color: #fff; border-radius: 10px; font-weight: 600; font-size: 1.2rem;">
      Q & A
    </div>
  </div>
  <div style="margin-top: 2rem; font-size: 0.8rem; color: #A0AEC0;">
    📧 contact@example.edu.vn &nbsp;|&nbsp; 🔗 github.com/sdn-ids
  </div>
</div>
