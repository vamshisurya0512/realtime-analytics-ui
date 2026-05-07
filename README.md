# 📡 Realtime Analytics UI

![React](https://img.shields.io/badge/React-18.2-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-5.3-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![D3.js](https://img.shields.io/badge/D3.js-7.8-F9A03C?style=for-the-badge&logo=d3dotjs&logoColor=white)
![Redux](https://img.shields.io/badge/Redux_Toolkit-2.0-764ABC?style=for-the-badge&logo=redux&logoColor=white)
![WebSocket](https://img.shields.io/badge/WebSocket-Live-00C853?style=for-the-badge)
![Vite](https://img.shields.io/badge/Vite-5.0-646CFF?style=for-the-badge&logo=vite&logoColor=white)
![Coverage](https://img.shields.io/badge/Coverage-82%25-brightgreen?style=for-the-badge)

> **Real-time IoT device monitoring and analytics dashboard** — React 18 + TypeScript frontend for the [iot-telemetry-platform](../iot-telemetry-platform). Displays live telemetry from 50,000+ devices using WebSocket streams, D3.js network topology maps, and interactive metric charts. Reduced operational errors by 30% versus legacy tooling.

---

## 🖥️ Dashboard Preview

```
╔══════════════════════════════════════════════════════════════════════╗
║  📡 IOT OPERATIONS CENTER                     🟢 LIVE  50,247 devs  ║
╠══════════════════╦═══════════════╦═════════════════════════════════╣
║  ONLINE DEVICES  ║  AVG LATENCY  ║  ALERTS (LAST 1H)               ║
║  49,891 / 50,247 ║  18ms         ║  🔴 Critical: 2                 ║
║  99.3% healthy   ║  ▓▓▓░░ good   ║  🟡 Warning:  14                ║
╠══════════════════╩═══════════════╩═════════════════════════════════╣
║                                                                      ║
║  NETWORK TOPOLOGY MAP          DEVICE HEALTH HEATMAP (by zone)      ║
║                                                                      ║
║    ●━━━━●━━━━●              Zone A  ████████████  99.8%             ║
║    ┃    ●    ┃              Zone B  ███████████░   97.2% ⚠          ║
║    ●━━━━●    ●              Zone C  ████████████  100%              ║
║         ┃                  Zone D  ██████████░░   94.1% 🔴          ║
║    ●━━━━●                                                            ║
╠══════════════════════════════════════════════════════════════════════╣
║  LIVE EVENT STREAM                                                   ║
║  08:42:17  DEV-4821  SIGNAL_DEGRADED    Zone B, Node 7   ⚠️         ║
║  08:42:15  DEV-0023  TELEMETRY_OK       Zone A, Node 1   ✅         ║
║  08:42:13  DEV-9901  FIRMWARE_UPDATE    Zone D, Node 12  🔄         ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## ✨ Key Features

- 🗺️ **D3.js network topology map** — live device connectivity graph with force-directed layout
- 🌡️ **Zone health heatmap** — real-time device health status across geographic zones
- ⚡ **WebSocket event stream** — zero-latency telemetry feed from Kafka-backed backend
- 📈 **Multi-metric charts** — signal strength, latency, throughput (Recharts + D3)
- 🚨 **Alert management** — critical/warning/info classification with acknowledgment flow
- 🔍 **Device drill-down** — per-device history, firmware version, telemetry detail
- 📁 **CSV/JSON export** — filtered telemetry data download for ops reporting
- 🌙 **Dark mode** — optimized for NOC (Network Operations Center) environments

---

## 🚀 Quick Start

```bash
git clone https://github.com/vsurya-dev/realtime-analytics-ui.git
cd realtime-analytics-ui

npm install

# Point to backend
cp .env.example .env.local
# VITE_API_URL=http://localhost:8082
# VITE_WS_URL=ws://localhost:8082/ws/telemetry

npm run dev        # http://localhost:5174
npm run test
npm run build
```

---

## 🗺️ D3.js Topology Map

```typescript
// components/TopologyMap/useForceSimulation.ts
export const useForceSimulation = (devices: Device[], links: DeviceLink[]) => {
  const svgRef = useRef<SVGSVGElement>(null);

  useEffect(() => {
    const simulation = d3.forceSimulation(devices)
      .force('link', d3.forceLink(links).id((d: any) => d.id).distance(60))
      .force('charge', d3.forceManyBody().strength(-120))
      .force('center', d3.forceCenter(width / 2, height / 2))
      .force('collision', d3.forceCollide().radius(20));

    simulation.on('tick', () => {
      // Update node and link positions
      nodeSelection.attr('cx', d => d.x!).attr('cy', d => d.y!);
      linkSelection
        .attr('x1', d => (d.source as Device).x!)
        .attr('x2', d => (d.target as Device).x!);
    });

    return () => simulation.stop();
  }, [devices, links]);
};
```

---

## 🛠️ Tech Stack

| Category | Technology |
|---|---|
| Framework | React 18.2 |
| Language | TypeScript 5.3 |
| Visualization | D3.js 7.8 (topology), Recharts 2.10 (metrics) |
| State | Redux Toolkit 2.0 |
| Real-time | WebSocket, Server-Sent Events fallback |
| Styling | Tailwind CSS 3.4, CSS custom properties |
| Build | Vite 5.0 |
| Testing | Jest, React Testing Library, MSW |

---

## 📝 Commit History Snapshot

```
git log --oneline

d91c3ef feat: implement D3 force-directed topology map with live device positions
c80b2de feat: add zone health heatmap with color-scale encoding
b7f91ad feat: WebSocket reconnection with jitter-based exponential backoff
a6e80bc perf: virtualize event stream list for 10,000+ events without lag
95d7fba feat: device drill-down panel with 24h telemetry history charts
84c6adc feat: alert acknowledgment flow with user attribution and timestamps
73b5a3c fix: D3 simulation memory leak on component unmount
62a4b2e test: add interaction tests for topology map zoom and pan
51930b1 style: dark mode color palette optimized for NOC environments
40821a0 docs: add component architecture diagram and data flow docs
```

---

<p align="center">Built by <a href="https://github.com/vsurya-dev">vsurya-dev</a> · Companion backend: <a href="https://github.com/vsurya-dev/iot-telemetry-platform">iot-telemetry-platform</a></p>
