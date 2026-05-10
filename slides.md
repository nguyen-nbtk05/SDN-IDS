---
# FILE: slides.md
# PATH: /home/nora/Projects/slides/SDN-IDS/slides.md
# DESC: Main entry — SDN-IDS Academic Presentation
theme: default
title: 'Phát hiện và ngăn chặn tấn công mạng dựa trên SDN sử dụng Entropy'
titleTemplate: '%s — SDN-IDS'
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
  sans: Outfit
  serif: Inter
  mono: JetBrains Mono
  provider: google
drawings:
  enabled: false
defaults:
  transition: slide-left
layout: none
---

<CoverSlide
  badge="Đồ án chuyên ngành"
  advisor="TS. Nguyễn Văn A"
  student="Nguyễn Văn B — MSSV: 2012345"
  year="2025 – 2026"
/>

---
layout: content-card
transition: slide-left
---

# 📋 Nội dung báo cáo

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 16px; margin-top: 14px;">

<div v-click class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge">01</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Mở đầu & Phạm vi</strong>
</div>
<p style="font-size:0.75rem;">Lý do, mục tiêu, phạm vi, đối tượng, phương pháp</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-accent">02</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Cơ sở Lý thuyết</strong>
</div>
<p style="font-size:0.75rem;">Công nghệ, nghiên cứu liên quan, SDN, Entropy</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-success">03</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Phân tích & Thiết kế</strong>
</div>
<p style="font-size:0.75rem;">Yêu cầu, use case, kiến trúc, CSDL, sequence diagram</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-warning">04</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Cài đặt & Hiện thực</strong>
</div>
<p style="font-size:0.75rem;">Môi trường, cấu trúc dự án, code, screenshots</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge badge-danger">05</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Kiểm thử & Đánh giá</strong>
</div>
<p style="font-size:0.75rem;">Test cases, hiệu năng, nhận xét kết quả</p>
</div>

<div v-click class="glass-card" style="padding:14px 18px;">
<div style="display:flex;align-items:center;gap:8px;margin-bottom:4px;">
<span class="badge" style="background:rgba(139,92,246,0.15);color:#a78bfa;border-color:rgba(139,92,246,0.25);">06</span>
<strong style="font-size:0.85rem;color:var(--sdn-text-primary);">Kết luận & Phát triển</strong>
</div>
<p style="font-size:0.75rem;">Tổng kết, ưu/nhược, hướng phát triển, bài học</p>
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
src: ./pages/chapter6.md
---

---
layout: none
transition: slide-left
---

<EndSlide
  email="student@university.edu.vn"
  github="github.com/sdn-ids"
/>
