# TOR-INFINITY 🌌

An asynchronous, post-quantum-resilient 3-hop onion-routing mesh network and anonymous publishing portal built with **Rust** and **Python**.

Designed to run as a zero-dependency, self-contained architecture inside Termux / Android Linux sandboxes as well as standard Unix environments.

---

## 🏛️ System Architecture

```
[ Browser / API Client ]
           │
           │ HTTP POST /api/v1/post/anonymous
           ▼
[ Python API Gateway (Port 8000) ]
           │
           │ (Ephemeral 3-Layer XOR Wrapping)
           ▼
[ Tokio Entry Relay (Port 8081) ] ──> [ Middle Relay (8082) ] ──> [ Exit Node (8083) ]
                                                                            │
                                                                            ▼
                                                                [ Acknowledgment ACK ]
```

---

## ✨ Features

* **3-Hop Onion Routing**: Encrypts payloads in concentric layers with ephemeral 16-byte keys per hop.
* **Asynchronous Tokio Relays**: High-performance Rust backend handling socket handshakes, onion layer peeling, and covert jitter delays.
* **Native Python Gateway**: Integrated HTTP dashboard (`http://127.0.0.1:8000`) and JSON API endpoint using Python's stdlib `http.server`.
* **Zero External Dependencies**: Pure standard library implementation for maximum portability across mobile and desktop environments.

---

## 🚀 Quick Start

### 1. Prerequisites
* **Rust & Cargo** (`cargo 1.70+`)
* **Python 3.10+**

### 2. Build & Run Mesh Relays
In your first terminal, launch the 3-hop Rust mesh node stack:

```bash
cargo run --bin mesh_node
```

### 3. Launch the API Gateway & Official Portal
In a second terminal, start the Python web portal:

```bash
python3 api_gateway.py
```

### 4. Access the Interface
Open your browser and navigate to:
```
http://127.0.0.1:8000
```

---

## 🧪 Testing via CLI

You can send anonymous transmissions directly via `curl`:

```bash
curl -X POST http://127.0.0.1:8000/api/v1/post/anonymous \
  -H "Content-Type: application/json" \
  -d '{"board": "general", "content": "Anonymous broadcast from native CLI!"}'
```

**Response Example:**
```json
{
  "status": "success",
  "message": "Anonymous payload routed successfully through 3-hop mesh.",
  "circuit_acknowledgment": "TOR-INFINITY-ACK: BOARD:general|CONTENT:Anonymous broadcast from native CLI!"
}
```

---

## 📁 Repository Structure

```
tor_infinity/
├── src/
│   ├── main.rs         # Core protocol definitions
│   ├── mesh_relay.rs   # Hop-by-hop relay primitives
│   └── bin/
│       └── mesh_node.rs # Tokio async relay supervisor (Ports 8081-8083)
├── api_gateway.py      # Native HTTP gateway & dark-mode web portal
├── stress_test.py      # Concurrency & payload benchmarking script
├── test_client.py      # End-to-end circuit integration test
├── Cargo.toml          # Rust dependencies & binary targets
├── LICENSE             # MIT License
└── README.md           # Project documentation
```

---

## 📜 License

Distributed under the MIT License. See `LICENSE` for more information.