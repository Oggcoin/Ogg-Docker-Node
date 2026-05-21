# 🪨 OGG Node — Docker Hub Setup

Ogg make easy path.

No build from source. No cave headache. No compile pain.

Pull image. Run node. Cave connected.

**This setup runs a full Ogg node with:**
- RPC open on port `18545`
- P2P peer connection on port `30666`
- Persistent chain data — survives restarts
- Automatic restart if server reboots

---

## 📋 Requirements

| Requirement | Minimum |
|---|---|
| OS | Ubuntu 20.04 / 22.04 / 24.04 |
| RAM | 8 GB |
| CPU | 4 cores |
| Disk | 100 GB SSD |
| Docker | 20.x or higher |

---

## 1️⃣ Install Docker

First — give server the tools it need.

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable docker
sudo systemctl start docker
```

> Docker not installed? Node not run. Do this first.

---

## 2️⃣ Pull Ogg Node Image

Pull latest Ogg node image from Docker Hub.

```bash
docker pull oggcoin/ogg-node:latest
```

Image downloaded. Cave blueprint ready.

---

## 3️⃣ Create Data Volume

Ogg node needs a safe place to keep chain data.

```bash
docker volume create ogg-node-data
```

> This keeps blockchain data alive even if container is stopped or removed. Chain data survives. Volume is the rock.

---

## 4️⃣ Start Ogg Full Node

Node ready. Time to wake it.

```bash
docker run -d \
  --name ogg-node \
  --restart unless-stopped \
  -p 18545:18545 \
  -p 30666:30666/tcp \
  -p 30666:30666/udp \
  -v ogg-node-data:/data \
  oggcoin/ogg-node:latest
```

Ogg node now starts in background. Cave open. Peers incoming.

---

## 5️⃣ Check Logs

Watch node startup logs.

```bash
docker logs -f ogg-node
```

Healthy startup looks like this:

```
genesis initialized
RPC opened on 18545
peers connected
blocks syncing
```

> Press `CTRL + C` to exit log view. Node keeps running.

---

## 6️⃣ Check RPC

Confirm RPC is alive and responding.

```bash
curl -s http://127.0.0.1:18545 \
-H "Content-Type: application/json" \
--data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

Expected response:

```json
{"jsonrpc":"2.0","id":1,"result":"0xf8"}
```

> JSON response means RPC is working. Node is ready.

---

## 7️⃣ Check Peers and Sync

Open node console:

```bash
docker exec -it ogg-node /app/ogg-node attach http://127.0.0.1:18545
```

Run inside console:

```javascript
net.peerCount
eth.blockNumber
eth.syncing
```

**Healthy node:**

| Check | Expected |
|---|---|
| `net.peerCount` | `> 0` — peers found |
| `eth.blockNumber` | `> 0` — blocks arriving |
| `eth.syncing` | `false` — fully synced |

> If `eth.syncing` still shows progress — node is catching up. Let it finish.

Exit console:

```javascript
exit
```

---

## 8️⃣ Stop Node

Stop node:

```bash
docker stop ogg-node
```

Remove container:

```bash
docker rm ogg-node
```

> Chain data stays safe in Docker volume `ogg-node-data`. Container gone — data not gone. Pull and run again anytime.

---

## 🪨 Useful Commands

| Action | Command |
|---|---|
| Restart node | `docker restart ogg-node` |
| View logs | `docker logs -f ogg-node` |
| Running containers | `docker ps` |
| Check data volume | `docker volume ls` |
| Check RPC | `curl http://127.0.0.1:18545` |

---

## ✅ Node Ready Checklist

```
Docker installed        ✓
Image pulled            ✓
Volume created          ✓
Node running            ✓
RPC alive               ✓
Peers connected         ✓
Chain syncing           ✓
```

Cave connected. Rock is ready.

---

## 🔗 Links

| | |
|---|---|
| 🌐 Website | https://oggcoin.org |
| 📖 Docs | https://oggcoin.gitbook.io/the-book-of-ogg |
| ⛏️ Mining Pool | https://pool.oggcoin.org |
| 💬 Telegram | https://t.me/proveyouogg |
| 🐦 Twitter/X | https://x.com/oggcave |
| 🎮 Discord | https://discord.gg/VrBQz7upZb |

---

🪨 **Node running. Chain syncing. Cave connected.**
