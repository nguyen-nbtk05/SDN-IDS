---
# FILE: pages/chapter5.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter5.md
# DESC: Chương 5 - Kiểm thử và Đánh giá
layout: chapter
chapterNumber: 5
transition: slide-left
---

# Chương 5

## Kiểm thử và Đánh giá

Chiến lược kiểm thử, test cases, hiệu năng và kết quả

---
layout: content-card
transition: slide-left
---

# 5.1 Chiến lược kiểm thử

<div class="divider"></div>

<div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 12px; margin-top: 14px;">

<div v-click class="glass-card" style="padding:14px; text-align:center;">
  <div style="font-size:1.5rem;margin-bottom:6px;">🧪</div>
  <strong style="font-size:0.78rem;color:var(--sdn-primary-light);">Unit Test</strong>
  <p style="font-size:0.7rem;margin-top:4px;">Kiểm thử từng hàm riêng lẻ: entropy calc, delta computation</p>
  <div class="badge" style="margin-top:6px;font-size:0.6rem;">pytest</div>
</div>

<div v-click class="glass-card" style="padding:14px; text-align:center;">
  <div style="font-size:1.5rem;margin-bottom:6px;">🔗</div>
  <strong style="font-size:0.78rem;color:var(--sdn-accent-light);">Integration Test</strong>
  <p style="font-size:0.7rem;margin-top:4px;">Kiểm thử tương tác giữa Collector → Entropy → Mitigation</p>
  <div class="badge badge-accent" style="margin-top:6px;font-size:0.6rem;">pytest + mock</div>
</div>

<div v-click class="glass-card" style="padding:14px; text-align:center;">
  <div style="font-size:1.5rem;margin-bottom:6px;">🖥️</div>
  <strong style="font-size:0.78rem;color:var(--sdn-success);">System Test</strong>
  <p style="font-size:0.7rem;margin-top:4px;">Chạy toàn bộ hệ thống trên Mininet + Ryu + hping3</p>
  <div class="badge badge-success" style="margin-top:6px;font-size:0.6rem;">Mininet + hping3</div>
</div>

<div v-click class="glass-card" style="padding:14px; text-align:center;">
  <div style="font-size:1.5rem;margin-bottom:6px;">👤</div>
  <strong style="font-size:0.78rem;color:var(--sdn-warning);">UAT</strong>
  <p style="font-size:0.7rem;margin-top:4px;">Kiểm thử với kịch bản tấn công thực tế từ nhiều nguồn</p>
  <div class="badge badge-warning" style="margin-top:6px;font-size:0.6rem;">Manual</div>
</div>

</div>

---
layout: content-card
transition: slide-left
---

# 5.2 Bảng Test Case

<div class="divider"></div>

<div style="margin-top: 10px; font-size: 0.78rem;">

| Mã | Chức năng | Dữ liệu đầu vào | Kết quả mong đợi | Thực tế | Status |
|---|---|---|---|---|---|
| TC-01 | Entropy đồng đều | 100 IP khác nhau | H ≈ 6.64 | H = 6.6439 | ✅ Pass |
| TC-02 | Entropy 1 IP | 100 packets cùng IP | H = 0.0 | H = 0.0 | ✅ Pass |
| TC-03 | Entropy threshold | H = 0.95 | is_attack = True | True | ✅ Pass |
| TC-04 | Delta packets | count(t)=100, prev=80 | delta = 20 | delta = 20 | ✅ Pass |
| TC-05 | API timeout | Controller offline | Retry 3x → Error log | Đúng | ✅ Pass |
| TC-06 | SYN Flood detect | hping3 --flood | Alert < 10s | Alert ≈ 7s | ✅ Pass |
| TC-07 | Auto drop rule | Entropy < 1.0 | Flow rule installed | Installed | ✅ Pass |
| TC-08 | Normal traffic | iperf giữa hosts | Không alert | Không alert | ✅ Pass |
| TC-09 | Window overflow | > 4 samples | Oldest dropped | Đúng | ✅ Pass |
| TC-10 | Recovery | Stop attack | Entropy phục hồi | H > 2.5 | ✅ Pass |

</div>

---
layout: content-card
transition: slide-left
---

# 5.3 Đánh giá hiệu năng

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 10px;">

<div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
  <MetricCard v-click icon="⚡" value="~7s" label="Detection Time" description="Từ khi bắt đầu tấn công" variant="primary" />
  <MetricCard v-click icon="🎯" value="96.5%" label="Accuracy" description="True Positive Rate" variant="success" />
  <MetricCard v-click icon="⚠️" value="3.2%" label="False Positive" description="Dưới ngưỡng 5%" variant="warning" />
  <MetricCard v-click icon="🧠" value="18%" label="CPU Usage" description="Controller average" variant="accent" />
</div>

</div>

<div>

<FlowStats :width="400" />

<p v-click style="font-size:0.75rem; color:var(--sdn-text-muted); margin-top:8px; text-align:center;">
  Biểu đồ phân bố flow entries theo protocol trong quá trình kiểm thử
</p>

</div>

</div>

---
layout: content-card
transition: slide-left
---

# 5.4 Nhận xét kết quả kiểm thử

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="✅ Kết quả đạt được" icon="">

<v-clicks>

- **10/10 test cases** PASS thành công
- Phát hiện SYN Flood trong **~7 giây** (< 10s mục tiêu)
- **Accuracy 96.5%**, vượt mục tiêu 95%
- **False Positive 3.2%**, dưới ngưỡng 5%
- Tự động install drop rule **hoạt động ổn định**
- Entropy **phục hồi** sau khi tấn công kết thúc

</v-clicks>

</GlassBox>

<GlassBox title="⚠️ Vấn đề phát sinh" icon="">

<v-clicks>

- **Burst traffic** (iperf bandwidth test) gây false positive nhẹ (3.2%)
- **Polling interval 5s** có thể miss tấn công ngắn (< 5s)
- **Single controller** → bottleneck khi scale nhiều switch
- **Entropy threshold** cần fine-tune theo môi trường thực tế
- **Khắc phục**: Điều chỉnh threshold, thêm whitelist, adaptive polling

</v-clicks>

</GlassBox>

</div>

