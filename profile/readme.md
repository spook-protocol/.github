# SPOOK Wallet / Protocol

**Rebuilding the Phantom source with full-chain privacy architecture**

---

## 🕶️ Overview

**SPOOK** is a next-generation **privacy wallet and protocol stack** designed for EVM, Solana, TON, and XMR ecosystems.  
It provides **complete anonymity**, **multi-chain privacy transfers**, and **automated proxy rotation** — all without relying on centralized tracking or custodial intermediaries.

---

## 🔑 Core Features

### 1. Fully Private Wallet Architecture
- Supports **EVM / Solana / TON / XMR**.
- Import existing wallets or create new HD wallets (BIP-39 / BIP-32).
- All asset inflows are routed **through privacy mixers only**.
- Local-only key management, AES-256-GCM encryption.

### 2. Dual Interaction Modes
| Mode | Description |
|------|--------------|
| **Normal Interaction** | Direct on-chain transactions via connected RPC nodes. |
| **Privacy Transfer** | Multi-step ZK-based mixing workflow through temporary wallets and proof generation. |

### 3. Temporary “Mask” Wallets
- Automatically derive a new wallet for every transaction.
- Supports **batch creation** of temporary wallets.
- Temporary wallets self-destruct or encrypt-archive after use.

### 4. Cross-Chain Privacy Mixing
- Optional **centralized bridge integration** for XMR cross-chain routing.
- Allows asset anonymization via **XMR jump chain**, then return to any supported chain.
- Each bridge uses MPC escrow + proof-based verification.

### 5. Advanced Proxy & Network Privacy
- Built-in **SOCKS5 proxy rotation**.
- Optional **multi-hop proxy chain**.
- Every RPC request can use a **unique proxy channel** to reduce fingerprint correlation.

---

## 🧩 Protocol Components

| Component | Description |
|------------|-------------|
| **Wallet Core** | Key derivation, encryption, storage, hardware wallet support. |
| **Mixer Adapter Layer** | Unified interface for EVM, Solana, TON, XMR privacy pools. |
| **Proof Engine** | ZK-SNARK / Halo2-based proof generation (local or remote). |
| **Proxy Manager** | Manages SOCKS5 / HTTP proxies and connection rotation. |
| **Relay Service (Optional)** | Encrypted transaction relay layer for broadcast and obfuscation. |

---

## 🧠 Architecture Overview

```

┌──────────────────────────┐
│        SPOOK APP         │
│  (Plugin / Mobile / PC)  │
└──────────┬───────────────┘
│
▼
┌──────────────────────────┐
│ Wallet Core (HD / Temp)  │
│ ├─ Key Derivation (BIP32)│
│ ├─ Secure Storage (AES)  │
│ └─ Hardware Wallet API   │
└──────────┬───────────────┘
│
▼
┌──────────────────────────┐
│     Mixer Adapter Layer  │
│ (Tornado / Railgun / ... ) │
└──────────┬───────────────┘
│
▼
┌──────────────────────────┐
│   Proxy & Relay Manager  │
│   (SOCKS5 / Multi-hop)   │
└──────────────────────────┘

````

---

## ⚙️ Mixer Adapter API (Draft)

```ts
interface MixerAdapter {
  listSupportedPools(chain: string, asset: string): Promise<PoolInfo[]>;
  deposit(poolId: string, from: string, amount: number): Promise<{ txHash: string; note: string }>;
  prove(note: string, to: string): Promise<ProofData>;
  withdraw(proof: ProofData, to: string): Promise<string>;
  getNoteStatus(note: string): Promise<{ spent: boolean; confirmations: number }>;
}
````

---

## 🧾 Privacy Transaction Flow

1. User selects **“Privacy Transfer”**.
2. SPOOK derives a **temporary wallet A**.
3. Transfer from main wallet → A.
4. Deposit into **Mixer Protocol**, generating ZK note.
5. Derive new **temporary wallet B**.
6. Withdraw from mixer → B → target wallet.
7. Securely delete temporary wallets and proof traces.

---

## 🔒 Security Design

* Local-only private keys (never transmitted).
* Optional hardware wallet (Ledger/Trezor) integration.
* All storage AES-256-GCM encrypted.
* Optional TEE-based secure enclave for proof generation.
* Proxy-based request rotation to mitigate network fingerprinting.

---

## ⚖️ Compliance Disclaimer

> **Note:** Privacy and mixing protocols may be restricted or regulated in some jurisdictions.
> Users are solely responsible for compliance with local laws.
> SPOOK provides open-source privacy tools **without custodial control or data storage**.

---

## 🧱 Tech Stack

| Layer      | Technology                                      |
| ---------- | ----------------------------------------------- |
| Core       | TypeScript / Node.js / Rust (WASM Proof Engine) |
| UI         | React / React Native / Electron                 |
| Storage    | SQLite / LevelDB + AES Encryption               |
| ZK Proofs  | Halo2 / Groth16 via Rust + WASM                 |
| Proxy      | SOCKS5 / HTTP Multi-hop                         |
| Deployment | Docker / K8s (Relay & Proof Services)           |

---

## 🚀 Roadmap

### Phase 1 (MVP)

* EVM + Solana base wallet.
* Temporary wallet generation.
* Mock privacy adapter (local mixer simulation).
* Basic proxy manager.

### Phase 2 (Beta)

* Full EVM Tornado adapter.
* Solana privacy pool integration.
* XMR centralized bridge.
* Proof generation optimization (WASM).

### Phase 3 (1.0 Release)

* TON privacy integration.
* Multi-hop relay and proxy.
* Decentralized proof-verifier network.
* Open governance + security audit.

---

## 🧩 Repository Structure

```
spook-wallet/
├─ packages/
│  ├─ core/              # Wallet core (key, storage, tx)
│  ├─ mixer-adapters/    # Privacy protocol adapters
│  ├─ proxy-manager/     # Proxy routing & rotation
│  ├─ proof-engine/      # ZK proof generator
│  └─ ui/                # React frontend / Electron app
├─ scripts/
│  ├─ deploy/            # Deployment helpers
│  └─ test/              # Automated test scripts
├─ docs/
│  ├─ ARCHITECTURE.md
│  ├─ PROTOCOLS.md
│  └─ SECURITY.md
└─ README.md
```

---

## 📄 License

**MIT License**
© 2025 Spook Protocol Technology Co., Ltd.
All rights reserved.

---

## 🤝 Contributing

Pull requests are welcome!
Before contributing, please:

* Run code linting and unit tests.
* Follow the privacy and security contribution guidelines in `docs/SECURITY.md`.
* Avoid committing sensitive keys or proxy credentials.

---

## 🌐 Links

* [Website (Coming Soon)](https://spookwallet.io)
* [Documentation](https://docs.spookwallet.io)
* [Telegram Community](https://t.me/spookwallet)
* [Twitter / X](https://x.com/spookwallet)

---

> “Privacy is not a crime — it’s a human right.”

