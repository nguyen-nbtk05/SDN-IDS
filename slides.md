---
# FILE: slides.md
# PATH: /home/nora/Projects/slides/SDN-IDS/slides.md
# DESC: Main entry - SDN-IDS Academic Presentation
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
  sans: 'Inter'
  mono: 'Fira Code'
  provider: google
unoCSS:
  presets:
    - icons
  icons:
    collections:
      mdi: true
      twemoji: true
drawings:
  enabled: false
defaults:
  transition: slide-left
layout: cover-custom
---

<CoverSlide
  badge="CHUYÊN ĐỀ MẠNG MÁY TÍNH 1"
  advisor="Trần Vĩnh Phúc"
  placeDate="Đà Lạt - 05/2026"
  :student="['2312702 - Nguyễn Bá Thiều Khôi Nguyên', '2312675 - Hồ Quốc Long', '2312613 - Lê Nguyễn Đức Hiếu']"
/>

---
layout: content-card
transition: slide-left
---

<div class="deck-kicker">Theo mục lục báo cáo</div>

# NỘI DUNG BÁO CÁO

<div class="agenda-rail">
  <div v-click class="agenda-hero">
    <div>
      <span class="badge badge-network">INSTRUSION DETECTION SYSTEM</span>
      <h2 style="text-align: justify;">PHÁT HIỆN VÀ NGĂN CHẶN TẤN CÔNG MẠNG DỰA TRÊN MÔ HÌNH MẠNG SDN</h2>
    </div>
    <div class="metric-row">
      <MetricCard icon="i-twemoji-clipboard" value="50" label="Trang báo cáo" variant="primary" />
      <MetricCard icon="i-twemoji-open-book" value="5" label="CHƯƠNG" variant="accent" />
    </div>
  </div>

  <div class="agenda-list">
    <div v-click class="agenda-item accent-blue">
      <div class="agenda-number">01</div>
      <div><div class="agenda-title">Mở đầu và phạm vi đề tài</div><div class="agenda-desc">Lý do chọn đề tài, mục tiêu, phạm vi, đối tượng, phương pháp</div></div>
    </div>
    <div v-click class="agenda-item accent-cyan">
      <div class="agenda-number">02</div>
      <div><div class="agenda-title">Cơ sở lý thuyết và công nghệ</div><div class="agenda-desc">IDS/IPS, SDN, OpenFlow, Ryu, Mininet, Entropy</div></div>
    </div>
    <div v-click class="agenda-item accent-green">
      <div class="agenda-number">03</div>
      <div><div class="agenda-title">Phân tích và thiết kế hệ thống</div><div class="agenda-desc">Yêu cầu, use case, module, flowchart, sequence diagram</div></div>
    </div>
    <div v-click class="agenda-item accent-amber">
      <div class="agenda-number">04</div>
      <div><div class="agenda-title">Triển khai thực nghiệm và đánh giá</div><div class="agenda-desc">Topology, mã nguồn, kịch bản kiểm thử, đánh giá hiệu suất</div></div>
    </div>
    <div v-click class="agenda-item accent-red">
      <div class="agenda-number">05</div>
      <div><div class="agenda-title">Kết luận và hướng phát triển</div><div class="agenda-desc">Kết quả đạt được, ưu điểm, hạn chế, bài học</div></div>
    </div>
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
  github-label="github.com/hoquoclong/SDN-IDS"
  github-url="https://github.com/hoquoclong/SDN-IDS"
/>
