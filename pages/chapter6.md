---
# FILE: pages/chapter6.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter6.md
# DESC: Chương 6 - Kết luận và Hướng phát triển
layout: chapter
chapterNumber: 6
transition: slide-left
---

# Chương 6

## Kết luận và Hướng phát triển

Tổng kết, đánh giá và hướng phát triển tương lai

---
layout: content-card
transition: slide-left
---

# 6.1 Tóm tắt kết quả đạt được

<div class="divider"></div>

<div style="margin-top: 12px;">

| Mục tiêu | Kết quả | Đánh giá |
|---|---|---|
| **MT-01**: Xây dựng mô hình SDN (Mininet + Ryu + OF 1.3) | Topo 3 SW, 6 hosts hoạt động ổn định | <twemoji-white-heavy-check-mark /> Đạt |
| **MT-02**: Module thu thập Flow Statistics (polling 5s) | Delta packets chính xác, REST API ổn định | <twemoji-white-heavy-check-mark /> Đạt |
| **MT-03**: Phân tích Shannon Entropy (threshold 1.0) | Entropy < 1.0 khi SYN Flood, phát hiện trong ~7s | <twemoji-white-heavy-check-mark /> Đạt |
| **MT-04**: Tự động ngăn chặn (drop flow rule < 10s) | Drop rule installed thành công, ~8s response | <twemoji-white-heavy-check-mark /> Đạt |
| **MT-05**: Accuracy ≥ 95%, FP < 5% | Accuracy 96.5%, FP 3.2% | <twemoji-white-heavy-check-mark /> Đạt |

</div>

<div v-click style="margin-top: 14px; text-align: center;">
  <div style="display:inline-flex;gap:16px;">
    <MetricCard icon="i-twemoji-white-heavy-check-mark" value="5/5" label="Mục tiêu đạt" variant="success" />
    <MetricCard icon="i-twemoji-bar-chart" value="96.5%" label="Accuracy" variant="primary" />
    <MetricCard icon="i-twemoji-stopwatch" value="~7s" label="Detection" variant="accent" />
  </div>
</div>

---
layout: two-cols-custom
transition: slide-left
---

::header::

# 6.2 Ưu điểm & Hạn chế

::default::

## <twemoji-white-heavy-check-mark /> Ưu điểm

<v-clicks>

- **Real-time detection**: Phát hiện DDoS trong vài giây
- **Tự động phản ứng**: Không cần can thiệp thủ công
- **Zero-day capable**: Không phụ thuộc signature database
- **Lightweight**: CPU < 20%, phù hợp embedded
- **Modular**: Dễ mở rộng, thêm module mới
- **Open-source stack**: Ryu + OVS + Python = miễn phí

</v-clicks>

::right::

## <twemoji-cross-mark /> Hạn chế

<v-clicks>

- **Chỉ 1 loại tấn công**: SYN Flood (chưa hỗ trợ UDP, Slowloris)
- **Môi trường mô phỏng**: Chưa thử nghiệm mạng thật
- **Single controller**: Không hỗ trợ HA/clustering
- **Threshold cố định**: Cần manual tuning theo mạng
- **Không có dashboard**: Chỉ output console/CSV
- **Polling-based**: Có thể miss tấn công < 5s

</v-clicks>

---
layout: content-card
transition: slide-left
---

# 6.3 Hướng phát triển

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 14px; margin-top: 12px;">

<div v-click class="glass-card" style="padding:14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge">HP-01</span>
    <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Tích hợp Machine Learning</strong>
  </div>
  <p style="font-size:0.75rem;">Kết hợp Random Forest/SVM với entropy để phân loại multi-class attacks. Cần dataset CIC-IDS2017 để training.</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-accent">HP-02</span>
    <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Dashboard real-time</strong>
  </div>
  <p style="font-size:0.75rem;">Xây dựng web dashboard (React/Vue + WebSocket) hiển thị entropy, flow stats, alerts real-time.</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-success">HP-03</span>
    <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Multi-controller HA</strong>
  </div>
  <p style="font-size:0.75rem;">Triển khai ONOS/ODL cluster để đảm bảo high availability. Cần Kubernetes orchestration.</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-warning">HP-04</span>
    <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Adaptive Threshold</strong>
  </div>
  <p style="font-size:0.75rem;">Tự động điều chỉnh threshold dựa trên baseline traffic pattern (EWMA / Z-score).</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
  <div style="display:flex;align-items:center;gap:8px;margin-bottom:6px;">
    <span class="badge badge-danger">HP-05</span>
    <strong style="font-size:0.82rem;color:var(--sdn-text-primary);">Triển khai thực tế</strong>
  </div>
  <p style="font-size:0.75rem;">Thử nghiệm trên mạng campus thực tế với hardware SDN switches (HP Aruba, Cisco Nexus).</p>
</div>

</div>

---
layout: content-card
transition: slide-left
---

# 6.4 Bài học kinh nghiệm

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 14px;">

<GlassBox title="Kỹ thuật" icon="i-twemoji-wrench">

<v-clicks>

- **SDN programming** đòi hỏi hiểu sâu OpenFlow protocol
- **Entropy-based detection** đơn giản nhưng hiệu quả cho DDoS
- **Sliding window** là kỹ thuật quan trọng cho time-series analysis
- **REST API design** cần xử lý error handling kỹ lưỡng

</v-clicks>

</GlassBox>

<GlassBox title="Quy trình" icon="i-twemoji-clipboard">

<v-clicks>

- **Đặt mục tiêu SMART** giúp đánh giá rõ ràng kết quả
- **Viết test trước** (TDD) giảm bug đáng kể
- **Documentation liên tục** tiết kiệm thời gian viết báo cáo
- **Version control** (Git) là bắt buộc cho mọi dự án

</v-clicks>

</GlassBox>

</div>

