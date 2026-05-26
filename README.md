# 🪨 OGG Node — Docker Hub Setup

Ogg make easy path.
No build from source. No cave headache. No compile pain.
Pull image. Run node. Cave connected.

**This setup runs a full Ogg node with:**
- RPC open on port `18545`
- P2P peer connection on port `30303`
- Persistent chain data — survives restarts
- Automatic restart if server reboots
- Bootnodes hardcoded — peers connect automatically

---

## 📋 Requirements

| Requirement | Minimum |
|---|---|
| OS | Ubuntu 20.04 / 22.04 / 24.04 |
| RAM | 4 GB |
| CPU | 2 cores |
| Disk | 100 GB SSD |
| Docker | 20.x or higher |
| Ports | 18545 (RPC), 30303 (P2P) |

---

## 1️⃣ Install Docker

First — give server the tools it needs.

```bash
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
```

Verify Docker is running:

```bash
docker --version
```

---

## 2️⃣ Pull Ogg Node Image

Pull latest Ogg node image from Docker Hub.

```bash
docker pull oggcoin/ogg-node:latest
```

Image downloaded. Cave blueprint ready.

---

## 3️⃣ Start Node

### Full Node / Exchange (No Mining)

```bash
docker run -d \
  --name ogg-node \
  --restart unless-stopped \
  -p 18545:18545 \
  -p 30303:30303/tcp \
  -p 30303:30303/udp \
  -v $(pwd)/egem-data:/root/egem-data \
  oggcoin/ogg-node:latest
```

Node starts in background. Chain data saved to `egem-data` folder. Cave open.

### Solo Miner

Add your wallet address with `-e WALLET=`:

```bash
docker run -d \
  --name ogg-node \
  --restart unless-stopped \
  -e WALLET=0xYourWalletAddress \
  -p 18545:18545 \
  -p 30303:30303/tcp \
  -p 30303:30303/udp \
  -v $(pwd)/egem-data:/root/egem-data \
  oggcoin/ogg-node:latest
```

Replace `0xYourWalletAddress` with your OGG wallet address. Mining rewards go directly to that wallet.

---

## 4️⃣ Check Logs

Watch node startup logs:

```bash
docker logs -f ogg-node
```

Healthy startup looks like this:

```
Initializing chain data...
ChainID: 70088
RPC opened on 18545
peers connected
blocks syncing
```

> Press `CTRL + C` to exit log view. Node keeps running.

---

## 5️⃣ Check RPC

Confirm RPC is alive and responding:

```bash
curl -s -X POST http://127.0.0.1:18545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

Expected response:

```json
{"jsonrpc":"2.0","id":1,"result":"0x81"}
```

JSON response means RPC is working. Node is ready.

---

## 6️⃣ Check Peers and Sync

Check connected peers:

```bash
curl -s -X POST http://127.0.0.1:18545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"net_peerCount","params":[],"id":1}'
```

Check sync status:

```bash
curl -s -X POST http://127.0.0.1:18545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'
```

Check current block:

```bash
curl -s -X POST http://127.0.0.1:18545 \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'
```

**Healthy node:**

| Check | Expected |
|---|---|
| `net_peerCount` | `0x2` or higher — peers found |
| `eth_blockNumber` | Increasing — blocks arriving |
| `eth_syncing` | `false` — fully synced |

> If `eth_syncing` still shows progress — node is catching up. Let it finish.

---

## 7️⃣ Stop / Start / Restart

Stop node:
```bash
docker stop ogg-node
```

Start node:
```bash
docker start ogg-node
```

Restart node:
```bash
docker restart ogg-node
```

Remove container (chain data stays safe):
```bash
docker stop ogg-node && docker rm ogg-node
```

> Chain data stays safe in `egem-data` folder. Container gone — data not gone. Run again anytime.

---

## 🪨 Useful Commands

| Action | Command |
|---|---|
| View logs | `docker logs -f ogg-node` |
| Running containers | `docker ps` |
| Check RPC | `curl -s -X POST http://127.0.0.1:18545 -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}'` |
| Stop node | `docker stop ogg-node` |
| Restart node | `docker restart ogg-node` |
| Remove container | `docker stop ogg-node && docker rm ogg-node` |

---

## ✅ Node Ready Checklist

```
Docker installed        ✓
Image pulled            ✓
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
