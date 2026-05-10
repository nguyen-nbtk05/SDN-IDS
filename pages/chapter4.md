---
# FILE: pages/chapter4.md
# PATH: /home/nora/Projects/slides/SDN-IDS/pages/chapter4.md
# DESC: Chương 4 - Cài đặt và Hiện thực
layout: chapter
chapterNumber: 4
transition: slide-left
---

# Chương 4

## Cài đặt và Hiện thực

Môi trường, cấu trúc dự án và triển khai các chức năng chính

---
layout: content-card
transition: slide-left
---

# 4.1 Môi trường phát triển

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="Phần cứng" icon="🖥️">

<v-clicks>

- **CPU**: Intel Core i5-12400 (6 cores)
- **RAM**: 16 GB DDR4
- **Storage**: SSD 256 GB
- **Network**: NIC 1 Gbps

</v-clicks>

</GlassBox>

<GlassBox title="Phần mềm" icon="💻">

<v-clicks>

- **OS**: Ubuntu 22.04 LTS (64-bit)
- **Python**: 3.10.12
- **Ryu**: 4.34 (SDN Controller)
- **Mininet**: 2.3.1 (Network Emulator)
- **Open vSwitch**: 2.17.x
- **hping3**: 3.0.0-alpha (Attack Tool)

</v-clicks>

</GlassBox>

</div>

<div v-click style="margin-top: 12px;">

<GlassBox title="Yêu cầu tối thiểu" icon="⚡" compact>

- **RAM ≥ 8GB** (Mininet + Controller chiếm ~2GB), **Python ≥ 3.8**, **Ubuntu ≥ 20.04**

</GlassBox>

</div>

---
layout: content-card
transition: slide-left
---

# 4.2 Cấu trúc thư mục dự án

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 10px;">

<div>

```text
SDN-IDS/
├── controller/
│   ├── simple_switch_13.py    # L2 Switch App
│   └── rest_api.py            # REST API extension
├── ids/
│   ├── ids_detector.py        # Main IDS module
│   ├── entropy_engine.py      # Shannon Entropy
│   ├── flow_collector.py      # Flow Stats polling
│   └── mitigation.py          # Drop rule handler
├── topology/
│   ├── custom_topo.py         # Mininet topology
│   └── topo_config.yaml       # Topo parameters
├── tests/
│   ├── test_entropy.py        # Unit tests
│   ├── test_collector.py      # Integration tests
│   └── test_attack.sh         # Attack scripts
├── logs/
│   └── ids_alerts.csv         # Alert log output
├── requirements.txt
└── README.md
```

</div>

<div>

<GlassBox title="Mô tả module" icon="📦" compact>

<v-clicks>

- **controller/** — Ứng dụng Ryu Controller cơ bản (L2 switch + REST)
- **ids/** — Core IDS: thu thập, phân tích, phản ứng
- **topology/** — Định nghĩa mạng Mininet
- **tests/** — Test scripts (unit + integration + attack)
- **logs/** — Output logs & alerts

</v-clicks>

</GlassBox>

</div>

</div>

---
layout: content-card
transition: slide-left
---

# 4.3 Cài đặt chức năng chính — Entropy Engine

<div class="divider"></div>

```python {1|3-5|7-18|20-24|all}
# FILE: ids/entropy_engine.py — Module tính Shannon Entropy

import math
from collections import Counter
from typing import List, Tuple

def calculate_entropy(ip_list: List[str]) -> float:
    """
    Tính Shannon Entropy cho danh sách source IP.
    H(X) = -Σ p(x) * log2(p(x))
    Returns: entropy value (float)
    """
    if not ip_list:
        return 0.0
    total = len(ip_list)
    counter = Counter(ip_list)
    entropy = -sum(
        (count / total) * math.log2(count / total)
        for count in counter.values()
    )
    return round(entropy, 4)

def is_attack(entropy: float, threshold: float = 1.0) -> bool:
    """Kiểm tra entropy có dưới ngưỡng tấn công không."""
    return entropy < threshold
```

<div v-click style="margin-top: 8px; display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px;">
  <MetricCard icon="📊" value="O(n)" label="Complexity" variant="primary" />
  <MetricCard icon="🎯" value="1.0" label="Threshold" variant="warning" />
  <MetricCard icon="⏱️" value="< 1ms" label="Calc Time" variant="success" />
</div>

---
layout: content-card
transition: slide-left
---

# 4.3 Cài đặt chức năng chính — Flow Collector

<div class="divider"></div>

```python {1|3-6|8-22|all}
# FILE: ids/flow_collector.py — Module polling flow statistics

import requests
import time
from collections import deque
from entropy_engine import calculate_entropy, is_attack

class FlowCollector:
    def __init__(self, controller_url="http://127.0.0.1:8080", dpid=1):
        self.url = f"{controller_url}/stats/flow/{dpid}"
        self.prev_counts = {}          # State persistence
        self.window = deque(maxlen=4)   # Sliding window 20s

    def poll(self):
        """Thu thập flow stats và tính delta packets."""
        resp = requests.get(self.url, timeout=5)
        flows = resp.json().get(str(self.dpid), [])
        current = {}
        for flow in flows:
            key = (flow['match'].get('ipv4_src', 'unknown'),)
            current[key] = flow['packet_count']
        # Tính delta
        deltas = {k: current[k] - self.prev_counts.get(k, 0)
                  for k in current}
        self.prev_counts = current.copy()
        return deltas
```

---
layout: content-card
transition: slide-left
---

# 4.4 Minh họa hệ thống — Entropy Chart

<div class="divider"></div>

<div style="margin-top: 8px;">

<EntropyChart :width="700" />

</div>

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 12px; margin-top: 12px;">

<div v-click class="glass-card" style="padding:10px 14px; text-align:center;">
  <div style="font-size:0.7rem;font-weight:700;color:var(--sdn-success);text-transform:uppercase;">Trạng thái bình thường</div>
  <p style="font-size:0.72rem;margin-top:4px;">H ≈ 3.0 — Traffic phân bố đều, nhiều source IPs</p>
</div>

<div v-click class="glass-card" style="padding:10px 14px; text-align:center;">
  <div style="font-size:0.7rem;font-weight:700;color:var(--sdn-warning);text-transform:uppercase;">Ngưỡng cảnh báo</div>
  <p style="font-size:0.72rem;margin-top:4px;">H = 1.0 — Threshold, bắt đầu phát hiện bất thường</p>
</div>

<div v-click class="glass-card" style="padding:10px 14px; text-align:center;">
  <div style="font-size:0.7rem;font-weight:700;color:var(--sdn-danger);text-transform:uppercase;">Trạng thái tấn công</div>
  <p style="font-size:0.72rem;margin-top:4px;">H → 0 — DDoS SYN Flood, traffic tập trung vào 1 target</p>
</div>

</div>

---
layout: content-card
transition: slide-left
---

# 4.5 Xử lý lỗi & Bảo mật

<div class="divider"></div>

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 12px;">

<GlassBox title="Xử lý ngoại lệ" icon="🛡️">

<v-clicks>

- **Network Timeout**: Retry 3 lần với exponential backoff
- **JSON Parse Error**: Graceful fallback, log warning
- **Division by Zero**: Guard trong entropy calculation
- **Controller Down**: Health check mỗi 30s, auto-reconnect
- **Memory Overflow**: deque(maxlen=4) tự quản lý buffer

</v-clicks>

</GlassBox>

<GlassBox title="Bảo mật hệ thống" icon="🔒">

<v-clicks>

- **Controller Access**: Chỉ cho phép kết nối từ localhost
- **API Authentication**: Token-based cho REST API
- **Flow Rule Validation**: Kiểm tra integrity trước khi install
- **Rate Limiting**: Giới hạn số drop rules/phút
- **Audit Log**: Ghi lại mọi hành động mitigation

</v-clicks>

</GlassBox>

</div>

