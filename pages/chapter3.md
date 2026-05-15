---
# FILE: pages/chapter3.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter3.md
# DESC: Chương 3 - Phân tích và Thiết kế Hệ thống
layout: chapter
chapterNumber: 3
transition: slide-left
---

# CHƯƠNG 3: PHÂN TÍCH VÀ THIẾT KẾ HỆ THỐNG

## Yêu cầu, tác nhân, kiến trúc và luồng xử lý

- Trình bày yêu cầu, tác nhân và chức năng chính của hệ thống.
- Mô tả kiến trúc phát hiện và ngăn chặn tấn công mạng trên nền SDN.
- Làm rõ luồng xử lý từ thống kê luồng, Entropy đến luật ngăn chặn.
- Trình bày sơ đồ ca sử dụng, kiến trúc, lưu đồ thuật toán và sơ đồ tuần tự.

---
layout: content-card
transition: slide-left
---

# Mục tiêu thiết kế hệ thống

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Từ cơ sở lý thuyết" compact>

- Xây dựng hệ thống phát hiện xâm nhập hoạt động trên bộ điều khiển Ryu.
- Thu thập thống kê luồng định kỳ từ thiết bị chuyển mạch OpenFlow.
- Phân tích bất thường bằng Shannon Entropy và đối chiếu MAC-IP.

</GlassBox>

<GlassBox title="Đến thiết kế phòng thủ" compact>

- Tự động cài đặt luật loại bỏ khi phát hiện nguồn tấn công.
- Ghi nhận cảnh báo để phục vụ theo dõi và đánh giá.
- Tổ chức các chức năng theo mô-đun để dễ mở rộng và kiểm thử.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Yêu cầu chức năng

<div class="divider"></div>

<div style="display:grid;grid-template-columns:repeat(2,1fr);gap:14px;margin-top:12px;">

<GlassBox title="Thu thập dữ liệu" compact>

- Truy vấn định kỳ bảng luồng.
- Lấy số gói tin, số byte và thông tin đặc trưng.
- Loại bỏ dữ liệu không phục vụ phân tích tấn công.

</GlassBox>

<GlassBox title="Phân tích bất thường" compact>

- Tính Entropy cho IP nguồn và cổng đích.
- Phát hiện DDoS và dò quét cổng.
- Phân tích bản tin ARP để phát hiện giả mạo.

</GlassBox>

<GlassBox title="Ngăn chặn chủ động" compact>

- Xác định nguồn vi phạm.
- Sinh luật loại bỏ tương ứng.
- Cài luật xuống thiết bị chuyển mạch OpenFlow.

</GlassBox>

<GlassBox title="Lưu vết và cảnh báo" compact>

- Ghi nhận thời điểm và loại tấn công.
- Lưu địa chỉ vi phạm và giá trị Entropy.
- Hiển thị bản ghi nhật ký trên thiết bị đầu cuối.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Yêu cầu phi chức năng

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Hiệu năng vận hành" compact>

- Độ trễ từ phát hiện đến cài luật cần ngắn.
- Ưu tiên thống kê luồng, không dùng kiểm tra sâu gói tin.
- Hạn chế tiêu thụ CPU, bộ nhớ và băng thông kênh điều khiển.

</GlassBox>

<GlassBox title="Độ tin cậy" compact>

- Không làm gián đoạn lưu lượng hợp lệ.
- Bộ điều khiển vẫn duy trì khả năng xử lý khi có tấn công.
- Có thể bổ sung thuật toán hoặc kiểu tấn công mới.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Tác nhân trong sơ đồ Use Case

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Quản trị viên mạng" compact>

- Khởi động hoặc dừng ứng dụng phát hiện xâm nhập.
- Theo dõi cảnh báo và bản ghi nhật ký.
- Đánh giá trạng thái an toàn của mạng.
- Không can thiệp vào quá trình tính toán tự động.

</GlassBox>

<GlassBox title="Thiết bị chuyển mạch OpenFlow" compact>

- Cung cấp thống kê từ bảng luồng.
- Gửi bản tin Packet-In khi gặp gói tin chưa có luật xử lý.
- Thực thi luật loại bỏ do bộ điều khiển cài đặt.
- Đóng vai trò nguồn dữ liệu và điểm thực thi phòng thủ.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Các ca sử dụng cốt lõi

<div class="divider"></div>

<div style="margin-top:12px;">

```mermaid {scale: 0.68}
flowchart LR
  A["1. Khởi động<br/>IDS"] --> B["2. Thu thập<br/>thống kê luồng"]
  B --> C["3. Phân tích<br/>Entropy và ARP"]
  C --> D["4. Phát hiện<br/>nguồn tấn công"]
  D --> E["5. Cài đặt<br/>luật ngăn chặn"]
  E --> F["6. Ghi nhận<br/>cảnh báo"]
  F --> B
```

</div>

<div v-click class="glass-card" style="margin-top:16px;padding:12px 16px;">
Chu trình được thiết kế để giám sát liên tục, phản ứng tự động và cung cấp thông tin cho quản trị viên.
</div>

---
layout: content-card
transition: slide-left
---

# Kiến trúc tổng thể hệ thống

<div class="divider"></div>

<div style="display:grid;grid-template-columns:0.9fr 1.1fr;gap:18px;margin-top:10px;align-items:start;">

<div>

- **Tầng ứng dụng**: mô-đun IDS, phát hiện DDoS, dò quét cổng, giả mạo ARP và ghi nhật ký.
- **Tầng điều khiển**: bộ điều khiển Ryu xử lý sự kiện và gửi lệnh OpenFlow.
- **Tầng dữ liệu**: thiết bị chuyển mạch OpenFlow và các máy trạm trong Mininet.
- Các máy trạm được phân vai: kẻ tấn công, nạn nhân và người dùng hợp lệ.

</div>

<div>

```mermaid {scale: 0.58}
flowchart TB
  A["Tầng ứng dụng<br/>IDS, Entropy, ARP, nhật ký"]
  C["Tầng điều khiển<br/>Bộ điều khiển Ryu"]
  D["Tầng dữ liệu<br/>Thiết bị chuyển mạch OpenFlow"]
  H["Máy tấn công<br/>Nạn nhân<br/>Người dùng hợp lệ"]
  A <-->|"Giao diện lập trình"| C
  C <-->|"OpenFlow"| D
  D --- H
```

</div>

</div>

---
layout: content-card
transition: slide-left
---

# Cấu trúc mô-đun chức năng

<div class="divider"></div>

<div style="display:grid;grid-template-columns:repeat(3,1fr);gap:12px;margin-top:12px;">

<GlassBox title="Thu thập thống kê" compact>
Lấy dữ liệu từ bảng luồng theo chu kỳ.
</GlassBox>

<GlassBox title="Cửa sổ thời gian trượt" compact>
Tổ chức dữ liệu theo từng khoảng quan sát.
</GlassBox>

<GlassBox title="Phân tích Entropy" compact>
Tính mức phân tán của IP nguồn và cổng đích.
</GlassBox>

<GlassBox title="Phát hiện và phân loại" compact>
Nhận diện DDoS, dò quét cổng và giả mạo ARP.
</GlassBox>

<GlassBox title="Ngăn chặn" compact>
Sinh luật loại bỏ và gửi xuống thiết bị chuyển mạch.
</GlassBox>

<GlassBox title="Ghi nhật ký" compact>
Lưu cảnh báo và dữ liệu phục vụ đánh giá.
</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Luồng hoạt động liên mô-đun

<div class="divider"></div>

<div style="margin-top:12px;">

```mermaid {scale: 0.62}
flowchart LR
  S["Thiết bị chuyển mạch<br/>cập nhật bộ đếm"]
  C["Bộ điều khiển<br/>truy vấn thống kê"]
  W["Cửa sổ<br/>thời gian trượt"]
  E["Tính toán<br/>Entropy"]
  D["Phát hiện<br/>loại tấn công"]
  M["Cài đặt<br/>luật loại bỏ"]
  L["Ghi nhận<br/>cảnh báo"]
  S --> C --> W --> E --> D --> M --> L
```

</div>

<div v-click class="glass-card" style="margin-top:16px;padding:12px 16px;">
Luồng dữ liệu khép kín giúp hệ thống vừa quan sát, vừa phản ứng mà không cần can thiệp thủ công trong mỗi chu kỳ.
</div>

---
layout: content-card
transition: slide-left
---

# Lưu đồ thuật toán phát hiện

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-top:10px;align-items:start;">

<div>

- Thu thập thống kê luồng từ thiết bị chuyển mạch.
- Trích xuất IP nguồn, IP đích, cổng đích và số gói tin.
- Tính Entropy theo cửa sổ thời gian.
- So sánh với ngưỡng an toàn.
- Nếu bất thường, xác định nguồn nghi vấn, ngăn chặn và ghi nhật ký.

</div>

<div>

```mermaid {scale: 0.52}
flowchart TB
  A["Thu thập thống kê"] --> B["Trích xuất đặc trưng"]
  B --> C["Tính Entropy"]
  C --> D{"Vượt ngưỡng?"}
  D -- "Không" --> E["Tiếp tục giám sát"]
  D -- "Có" --> F["Xác định nguồn nghi vấn"]
  F --> G["Cài luật loại bỏ"]
  G --> H["Ghi nhật ký"]
  E --> A
  H --> A
```

</div>

</div>

---
layout: content-card
transition: slide-left
---

# Logic phát hiện DDoS

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Dấu hiệu quan sát" compact>

- DDoS làm xuất hiện nhiều IP nguồn hoặc lưu lượng bất thường hướng đến nạn nhân.
- Hệ thống theo dõi Entropy của IP nguồn và tốc độ gói tin.
- Dữ liệu được đánh giá theo từng cửa sổ thời gian.

</GlassBox>

<GlassBox title="Phản ứng phòng thủ" compact>

- Khi vượt ngưỡng, nguồn nghi vấn được đưa vào danh sách xử lý.
- Bộ điều khiển sinh luật loại bỏ cho nguồn hoặc luồng độc hại.
- Luật được cài xuống thiết bị chuyển mạch để giảm tải cho nạn nhân và bộ điều khiển.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Logic phát hiện dò quét cổng

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Đặc trưng hành vi" compact>

- Dò quét cổng tạo nhiều kết nối ngắn đến nhiều cổng đích.
- Hệ thống theo dõi sự phân tán cổng đích theo từng IP nguồn.
- Entropy của cổng đích tăng khi một nguồn truy cập nhiều cổng.

</GlassBox>

<GlassBox title="Điều kiện xử lý" compact>

- Số lượng cổng đích hoặc Entropy được so sánh với ngưỡng an toàn.
- Khi vượt ngưỡng, nguồn đó được đánh dấu là nghi vấn.
- Bộ điều khiển áp dụng luật chặn phù hợp trên thiết bị chuyển mạch.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Logic phát hiện giả mạo ARP

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Cơ sở phát hiện" compact>

- ARP thiếu cơ chế xác thực nên dễ bị giả mạo.
- Hệ thống xây dựng bảng ràng buộc MAC-IP tin cậy.
- Bản tin ARP mới được đối chiếu với bảng này.

</GlassBox>

<GlassBox title="Phản ứng" compact>

- Nếu MAC và IP sai lệch, hệ thống cảnh báo nguy cơ giả mạo ARP.
- Nguồn phát tán bản tin giả mạo có thể bị cô lập.
- Cơ chế này bổ sung cho Entropy vì giả mạo ARP không nhất thiết tạo biến động thống kê lớn.

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# Sơ đồ tuần tự xử lý tấn công

<div class="divider"></div>

<div style="margin-top:8px;">

```mermaid {scale: 0.52}
sequenceDiagram
    participant A as Kẻ tấn công
    participant S as Thiết bị chuyển mạch OpenFlow
    participant C as Bộ điều khiển Ryu

    A->>S: Gửi lưu lượng độc hại
    S->>C: Bản tin Packet-In hoặc cập nhật thống kê
    C->>S: Truy vấn thống kê định kỳ
    S-->>C: Trả về thống kê bảng luồng
    C->>C: Phân tích Entropy hoặc đối chiếu MAC-IP
    alt Phát hiện tấn công
        C->>S: Bản tin Flow-Mod chứa luật loại bỏ
        S->>S: Loại bỏ lưu lượng độc hại về sau
    else Bình thường
        C->>C: Tiếp tục giám sát
    end
```

</div>

---
layout: content-card
transition: slide-left
---

# Tổng kết Chương 3

<div class="divider"></div>

<div style="display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px;">

<GlassBox title="Kết quả thiết kế" compact>

- Xác định yêu cầu chức năng và phi chức năng của hệ thống.
- Kiến trúc được tổ chức theo mô hình phân tầng của SDN.
- Các mô-đun chính gồm thu thập, xử lý, phát hiện, ngăn chặn và ghi nhật ký.

</GlassBox>

<GlassBox title="Cơ sở triển khai" compact>

- Cơ chế phòng thủ dựa trên thống kê luồng, Entropy và đối chiếu MAC-IP.
- Luồng xử lý hỗ trợ phản ứng tự động bằng luật loại bỏ.
- Thiết kế này là nền tảng để triển khai thực nghiệm trong Chương 4.

</GlassBox>

</div>
