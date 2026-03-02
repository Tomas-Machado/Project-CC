# 🚀 Space Mission Network Simulation

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=for-the-badge&logo=python)
![CORE Emulator](https://img.shields.io/badge/Network-CORE_Emulator-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

A robust network simulation project developed for the **Computer Networks** course. This project simulates a mission on a planet involving autonomous **Rovers**, a **Mothership (Gateway)**, and a **Ground Control** station on Earth.

The system implements a custom **Reliable UDP Protocol** (Application Layer) capable of handling high packet loss, duplication, and data fragmentation, featuring a real-time Web Dashboard.

---

## 📸 Screenshots

### 1. CORE Topology
<p align="center">
  <img src=".CC-TP2-main/prints/Topologia.png" alt="Topology" width="600">
  <br>
  <em>Network Topology running in CORE</em>
</p>

---

### 2. Ground Control Dashboard
<p align="center">
  <img src=".CC-TP2-main/prints/GroundControl.png" alt="Dashboard" width="600">
  <br>
  <em>Real-time telemetry and control</em>
</p>

---

### 3. NaveMae Dashboard
<p align="center">
  <img src=".CC-TP2-main/prints/NaveMae.png" alt="NaveMae" width="600">
  <br>
  <em>Mothership real-time orders to the rovers</em>
</p>

## 🌟 Key Features

### 📡 Custom Reliability Protocol (App Layer)
We implemented a **Stop-and-Wait ARQ** mechanism over UDP to simulate challenging space links:
* **ACK System:** Every critical message (Status, Progress, Photos) requires an acknowledgment.
* **Automatic Retransmission:** Retries sending packets up to 5 times before entering a "persistence mode".
* **Duplicate Detection:** Filters out duplicated packets caused by network echoes using Sequence Numbers.
* **Connection Handshake:** Rovers initiate a connection (`STATUS: DESCONECTADO`) and wait for the Mothership's approval before starting operations.

### 📸 Data Fragmentation
* Simulates sending **High-Res Photos** from Mars.
* Splits large binary data into smaller **chunks (Fragments)** (Max 250 bytes).
* Reassembles them at the destination using `MORE_FRAGS` flags and offsets.

### 🎮 Ground Control Dashboard
* **Hybrid Architecture:** Python (Backend) + HTML/JS (Frontend).
* **Real-time Telemetry:** Updates Battery, Position (X,Y), and Status (`IDLE`, `EM_MISSAO`, `CHARGING`).
* **Mission Control:** Send commands (Collect Minerals, Photos, Seismic Analysis) directly from the browser.
* **Visual Status:** Rovers appear "Greyed out" until the handshake is complete.

### 📝 Auto-Logging
* Automatically generates timestamped logs (e.g., `navemae_2025-12-10_15-30.log`) for all nodes in the `logs/` folder.
* Records every ACK, Timeout, and Retransmission for post-mission analysis.

---

## 🛠️ Network Topology

The simulation runs inside the **CORE Network Emulator** with the following nodes:

1.  **Ground Control (Earth):** Runs the Firefox Dashboard.
2.  **Nave-Mãe (Mothership):** Acts as the central gateway/server.
3.  **Rovers (Alpha, Beta, Gamma):** Autonomous clients gathering data.
4.  **Satellite/Switch:** Simulates the link with **25% Packet Loss** and **20% Duplication** to test protocol robustness.

---

## 📂 Project Structure

```bash
TP2/
├── data/                  # Configuration Data
│   ├── missoes.json       # Mission definitions (ID, type, duration)
│   └── rovers_config.json # Static IP/Port config for Rovers
├── logs/                  # Auto-generated logs (timestamped)
├── src/
│   ├── services/          # Network Services (Modular)
│   │   ├── api.py         # REST API logic (GET/POST)
│   │   ├── tcp.py         # TCP Telemetry Service
│   │   └── udp.py         # UDP Reliable Protocol Service
│   ├── database.py        # Central State & Persistence
│   ├── HTTP.py            # HTTP Server Wrapper
│   ├── navemae.py         # Mothership Main Entry Point
│   ├── Pacote.py          # Custom Packet Struct & Serialization
│   └── rover_autonomo.py  # Rover Autonomous Logic
├── web/                   # Frontend Dashboards
│   ├── groundcontrol.html # Main Mission Control Interface
│   └── navemae.html       # Mothership Admin Panel
├── TP2.imn                # CORE Topology Source File
├── fechar.sh              # Panic Script (Cleanup processes)
└── run_core.sh            # 🚀 ALL-IN-ONE LAUNCH SCRIPT
```

## 🚀 How to Run

### Prerequisites
* Linux Environment (Virtual Machine).
* **CORE Network Emulator** installed.
* Python 3.

### Steps

1.  **Open CORE:**
    Open the topology file (`.imn`) in the CORE GUI.

2.  **Start Emulation:**
    Click the **Green Play Button** ▶️ in CORE to start the network.

3.  **Launch the System:**
    Open your terminal, navigate to the project folder, and run the automation script:

    ```bash
    cd ~/Desktop/TP2
    chmod +x run_core.sh
    ./run_core.sh
    ```

    > **What this script does:** It automatically detects the active CORE session, launches the Python scripts inside the specific virtual nodes, creates log files in `logs/`, and opens the Firefox dashboard.

4.  **Stop Simulation:**
    Press `CTRL + C` in the terminal to kill all processes safely.

---

## 🧪 Testing Scenarios

You can verify the system's robustness by applying "Link Effects" in CORE:

* **Scenario A (Perfect Network):** 0% Loss. Immediate ACKs.
* **Scenario B (Mars Storm):** **25% Loss**. Watch the logs show `[TIMEOUT] Retransmitting (1/5)...`. The system will recover automatically.
* **Scenario C (Echoes):** **20% Duplication**. Watch the logs show `[DUPLICADO] Packet ignored`.

---

## 👨‍💻 Authors

* **[Tomás Machado ]**
* **[Hugo Rauber]**
* **[Rui Fernandes]**

---

## 📜 License

This project is licensed under the **MIT License**.

See the [LICENSE](LICENSE) file for more details.

---

<p align="center">
  <i>Developed for Computer Networks - 2025/2026</i>
</p>
