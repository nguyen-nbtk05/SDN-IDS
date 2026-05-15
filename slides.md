---
# FILE: slides.md
# PATH: /home/nora/Projects/slides/SDN-IDS/slides.md
# DESC: Main entry — SDN-IDS Academic Presentation
theme: default
title: 'SDN-IDS'
author: 'SDN-IDS Research Team'
keywords: 'SDN, IDS, DDoS, Entropy, OpenFlow, Ryu, Mininet'
info: |
  ## SDN-IDS Presentation
  Phát hiện và ngăn chặn tấn công mạng dựa trên mô hình mạng SDN
  sử dụng thống kê luồng và phân tích Entropy.
transition: slide-left
colorSchema: light
highlighter: shiki
lineNumbers: true
aspectRatio: 16/9
canvasWidth: 980
mermaid:
  theme: default
fonts:
  sans: Inter
  serif: Merriweather
  mono: JetBrains Mono
  provider: google
drawings:
  enabled: false
defaults:
  transition: slide-left
layout: none
---

<CoverSlide
  badge="CHUYÊN ĐỀ MẠNG MÁY TÍNH 1"
  advisor="Trần Vĩnh Phúc"
  :student="['Nguyễn Bá Thiều Khôi Nguyên — MSSV: 2312702', 'Hồ Quốc Long — MSSV: 2312675']"
/>

---
layout: content-card
transition: slide-left
---

# 📋 NỘI DUNG BÁO CÁO

<div class="divider"></div>

<div v-click style="display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-top: 14px;">

<div class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge">01</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:800;">MỞ ĐẦU & PHẠM VI ĐỀ TÀI</strong>
</div>
<p style="font-size:0.75rem;">Lý do, mục tiêu, phạm vi, đối tượng, phương pháp</p>
</div>

<div class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-accent">02</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:800;">CƠ SỞ LÝ THUYẾT</strong>
</div>
<p style="font-size:0.75rem;">Công nghệ, nghiên cứu liên quan, SDN, Entropy</p>
</div>

<div class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-success">03</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:800;">PHÂN TÍCH & THIẾT KẾ HỆ THỐNG</strong>
</div>
<p style="font-size:0.75rem;">Yêu cầu, use case, kiến trúc, CSDL, sequence diagram</p>
</div>

<div class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-warning">04</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:800;">TRIỂN KHAI & ĐÁNH GIÁ</strong>
</div>
<p style="font-size:0.75rem;">Môi trường, triển khai, kịch bản kiểm thử, nhận xét</p>
</div>

<div class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-danger">05</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);font-weight:800;">KẾT LUẬN & HƯỚNG PHÁT TRIỂN</strong>
</div>
<p style="font-size:0.75rem;">Kết quả đạt được, ưu điểm, hạn chế, hướng mở rộng</p>
</div>

</div>

---
src: ./pages/chapter1.md
---

---
src: ./pages/chapter2.md
---

---
src: ./pages/chapter3.md
---

---
src: ./pages/chapter4.md
---

---
src: ./pages/chapter5.md
---

---
layout: none
transition: slide-left
---

<EndSlide
  email="student@university.edu.vn"
  github="github.com/sdn-ids"
/>
