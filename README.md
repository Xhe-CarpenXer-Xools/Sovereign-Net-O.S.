# SOVEREIGN NET OS

# Sovereign NET OS v2.0

**You are not the product. You are the root node.**

Sovereign NET OS is a browser-native, peer-to-peer operating system that redefines how identity, data, and applications work on the internet.

It does not replace the internet.
It changes who is in control within it.

---

## 🌐 What This Is

Sovereign NET OS is a **user-owned execution environment** that runs entirely in the browser and connects directly to other peers—without requiring centralized servers for identity, coordination, or data ownership.

It provides:

* **Self-sovereign identity** (cryptographic, user-controlled)
* **Local-first data storage** (you hold your data)
* **Peer-to-peer networking** (direct communication between users)
* **Append-only logs** (verifiable, tamper-resistant history)
* **Composable applications** (services that plug into *your* environment, not the other way around)

---

## ⚡ Core Principle

> Companies can exist. Platforms can exist.
> They just don’t get to own you.

Sovereign NET OS separates **users from platforms at the root layer**:

| Today’s Internet       | Sovereign NET OS    |
| ---------------------- | ------------------- |
| You log into apps      | Apps connect to you |
| Platforms own identity | You own identity    |
| Data is platform-bound | Data is portable    |
| Lock-in is default     | Exit is trivial     |

---

## 🧠 Architecture Overview

### 1. Identity Layer

* Ed25519 keypairs generated client-side
* No accounts, no email/passwords
* Identity = cryptographic proof, not platform registration

### 2. Data Layer

* Append-only logs (hash-chained)
* Local-first storage
* Sync via peer gossip (eventual consistency)

### 3. Network Layer

* WebRTC-based peer-to-peer mesh
* No required central servers
* NAT traversal + peer discovery mechanisms

### 4. Application Layer

* Apps run in-browser
* Apps request access to your data—not store it
* Stateless relative to user identity

---

## 🔐 Security Model

* **Zero-trust by default**
* All data signed and verifiable
* No implicit authority layers
* Users control key custody

> If you control the keys, you control the system.

---

## 🧩 What You Can Build

* Messaging systems without centralized servers
* Social networks without platform lock-in
* Marketplaces without custodial intermediaries
* Identity systems that persist across applications
* Offline-first collaborative tools

---

## 🚧 What This Is Not

* ❌ Not a blockchain (no global consensus required)
* ❌ Not anti-company or anti-business
* ❌ Not a replacement for all infrastructure
* ❌ Not magic—tradeoffs still exist

---

## ⚠️ Current Challenges

This system pushes against real-world constraints:

* Browser limitations (storage, background execution)
* P2P scalability and discovery
* Key management for non-technical users
* Data persistence across unreliable peers

These are active areas of development—not ignored problems.

---

## 🔄 Why This Matters

The current internet centralizes power by default:

* Identity is rented
* Data is trapped
* Access is conditional

Sovereign NET OS introduces a different default:

> You exist independently of any platform.

That single shift changes everything:

* Competition increases
* Lock-in weakens
* Users regain leverage

---

## 🚀 Philosophy

Sovereign NET OS is not about eliminating systems.
It’s about removing **structural dependency**.

You can still use:

* Social platforms
* Cloud services
* Online marketplaces

But you use them **without becoming them**.

---

## 🛠️ Status

Experimental.
Evolving.
Provocative by design.

This is a blueprint and a working direction—not a finished endpoint.

---

## 🤝 Contributing

We are building toward:

* Better peer discovery
* Improved local-first data models
* Usable key management
* Interoperable application interfaces

If you care about:

* Decentralized systems
* User sovereignty
* Rethinking the internet stack

You’re in the right place.

---



## Contents

- [Quick Start — Browser](#quick-start--browser)
- [Quick Start — Desktop (Electron + Real IPFS)](#quick-start--desktop-electron--real-ipfs)
- [Features](#features)
- [Architecture](#architecture)
- [Electron / IPFS Integration](#electron--ipfs-integration)
- [Building for Distribution](#building-for-distribution)
- [Roadmap](#roadmap)
- [License](#license)

---

## Quick Start — Browser

No install. No build step.

1. Download `index.html`
2. Open it in Chrome, Edge, or Firefox
3. Open 2–4 tabs side by side — they auto-discover each other via `BroadcastChannel` and form a live mesh

**That's it.** Peer discovery, signed commits, encrypted channels, and real DAG storage all run locally in your browser.

---

## Quick Start — Desktop (Electron + Real IPFS)

Wraps `index.html` in an Electron shell with a real [Kubo](https://github.com/ipfs/kubo) daemon underneath. Simulated data is replaced with live IPFS peers, real CIDs, genuine pubsub, and actual bandwidth stats.

### Prerequisites

| Tool | Install |
|---|---|
| Node.js 18+ | https://nodejs.org |
| Kubo (go-ipfs) | `brew install ipfs` · [Linux/Windows](https://docs.ipfs.tech/install/command-line/) |

### Setup

```bash
# 1. Clone / download the repo so index.html and src/ are in the same directory
# 2. Run setup — installs npm deps, checks Kubo, injects adapter into index.html
bash scripts/setup.sh

# 3. Launch
npm start

# Dev mode (opens DevTools)
npm run dev
```

The setup script handles everything: dependency install, Kubo detection, and injecting `src/ipfsAdapter.js` into `index.html` before `</body>`. You don't edit `index.html` manually.

---

## Features

| View | Features |
|---|---|
| 🏠 **Home** | Live stats, event stream, activity feed, peer health |
| 💻 **Code Editor** | Merkle-DAG versioning, Ed25519 signed commits, real-time collab cursors, sandboxed execution |
| 💬 **Messenger** | Public channels, AES-256-GCM encrypted rooms, DMs, invite keys, file sharing |
| 📡 **Social Feed** | Decentralized posts, likes, boosts, trending topics |
| 📁 **Files** | CID-addressed storage, drag & drop upload, encryption toggle |
| 🎬 **Video** | Peer-streamed video, live stream simulation |
| 🕸️ **Network** | Animated live mesh canvas, peer list, gossip log, routing table |
| 🛒 **Marketplace** | Buy/sell apps, earnings dashboard, asset valuation |
| 🔧 **App Builder** | Drag-and-drop component builder with live preview |
| ⌨️ **Console** | 30+ real commands: `cid`, `dag`, `state-set`, `run`, `blocks`, `sig-log`, `help` |
| 🪪 **Identity** | DID management, Ed25519 keys, badges, reputation |
| ⚔️ **Security** | RRTK auditor — reentrancy, flash-loan, access control, Sybil, Byzantine detection |

---

## Architecture

Everything you see is **actually happening** in the browser — not mocked.

```
Identity      Ed25519 keypair → DID (did:svn:0x...)  persisted in IndexedDB
Networking    BroadcastChannel mesh (real multi-tab P2P) + simulated WebRTC
Storage       IndexedDB stores: blocks · dag · channels · messages · files
Versioning    Merkle-DAG with signed commits, vector clocks, CRDT merge
Crypto        Web Crypto API — Ed25519 signatures, AES-256-GCM channel keys
State sync    LWW-CRDT gossip over BroadcastChannel
Execution     Sandboxed new Function() with op/time metering
Security      RRTK — pattern-based vulnerability scanner with severity grading
```

### Directory layout

```
Sovereign-Net-OS/
├── index.html                        ← The entire OS (single file, 287 KB)
├── package.json                      ← Electron + electron-builder config
├── src/
│   ├── main.js                       ← Electron main process
│   ├── preload.js                    ← Context bridge (window.ipfs API)
│   └── ipfsAdapter.js                ← Live IPFS wiring for the renderer
├── scripts/
│   └── setup.sh                      ← One-shot setup script
├── docs/
│   └── sovereign-net-os-documentation.pdf
├── LICENSE-COMMUNITY
└── LICENSE-COMMERCIAL
```

---

## Electron / IPFS Integration

### How it works

```
Renderer (index.html)
  └── window.ipfs.swarmPeers()
        └── IPC → main.js
              └── http.request → localhost:5001 (Kubo)
                    └── returns real peer list
```

All Kubo API calls go through `ipcMain` in the main process, bypassing CORS entirely. The renderer never touches `localhost:5001` directly.

### window.ipfs — available in Electron

```js
// Live peer list from the IPFS swarm
const { body } = await window.ipfs.swarmPeers();
// → { Peers: [{ Peer, Addr, Latency }, ...] }

// Add a file — get a real CID back
const { body } = await window.ipfs.add('hello.txt', new TextEncoder().encode('hi'));
// → { Hash: 'QmXxx...', Name: 'hello.txt', Size: '3' }

// Read any CID via the local gateway
const buf = await window.ipfs.cat('QmXxx...');

// Real pubsub
await window.ipfs.pubsubPub('sovereign-net/general', btoa('hello network'));

// Node identity
const { body } = await window.ipfs.id();
// → { ID: '12D3KooW...', Addresses: [...], PublicKey: '...' }

// Bandwidth + repo stats
await window.ipfs.statsBw();   // { TotalIn, TotalOut, RateIn, RateOut }
await window.ipfs.repoStat();  // { NumObjects, RepoSize }
```

### What ipfsAdapter.js replaces

When running in Electron, `ipfsAdapter.js` is injected at runtime and automatically patches the simulated subsystems:

| Simulated | Real (Kubo) |
|---|---|
| `generatePeers(24)` | `swarm/peers` — live IPFS swarm nodes |
| `STATE.did` | `ipfs id` — your actual peer ID as `did:ipfs:12D3...` |
| File upload handler | `ipfs add` — real CID returned and stored |
| `meshSendPublic()` | `pubsub/pub` on `sovereign-net/<channelId>` |
| Home stats panel | `stats/bw` + `repo/stat` — real bytes transferred |
| Console log | Kubo daemon stdout/stderr streamed via IPC |

The adapter wraps — it doesn't replace. BroadcastChannel mesh stays active as a local fallback.

### Daemon lifecycle

`main.js` handles the full Kubo lifecycle automatically:
- Detects if a daemon is already running on `localhost:5001`
- If not, finds the `ipfs` binary (bundled `bin/ipfs` → common install paths → `$PATH`)
- Initialises the IPFS repo in `userData/ipfs-repo` if needed
- Enables pubsub and configures CORS headers before starting
- Streams daemon logs to the Console view via IPC
- Kills the daemon cleanly on app quit (only if we spawned it)

---

## Building for Distribution

```bash
# macOS (.dmg + .zip)
npm run build:mac

# Windows (.exe NSIS installer)
npm run build:win

# Linux (.AppImage + .deb)
npm run build:linux

# All platforms
npm run build:all
```

Output lands in `dist/`.

### Bundling Kubo (zero-dependency install)

1. Download the Kubo binary for your target platform from  
   https://github.com/ipfs/kubo/releases

2. Place it at `bin/ipfs` (macOS/Linux) or `bin/ipfs.exe` (Windows)

3. Add `"bin/**/*"` to the `files` array in `package.json` → `build`

`main.js` checks `resources/bin/ipfs` first and uses it automatically.

---

## Roadmap

- [ ] IPNS — publish handle/profile to the DHT
- [ ] DAG-versioned editor commits stored as real IPFS objects (not just IndexedDB)
- [ ] OrbitDB for persistent, replicated message history across peers
- [ ] libp2p WebRTC for direct browser ↔ Electron peer connections
- [ ] Bundled Kubo binary for zero-dependency app install
- [ ] WASM Kubo — run the IPFS node directly in the browser tab
- [ ] Mobile PWA with service worker mesh

---

## License

- **Community use**: see `LICENSE-COMMUNITY`
- **Commercial use**: see `LICENSE-COMMERCIAL`

---

> *You are not using the internet. You* are *the internet.*
>
> Built as a single `index.html` to prove it can be done.


## 🧭 Final Thought

The internet doesn’t need to be destroyed to be fixed.

It just needs to stop owning the people who use it.

Sovereign NET OS is one path toward that reality.


---
