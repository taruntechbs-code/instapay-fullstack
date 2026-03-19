# ⚡ InstaPay — Gasless Stablecoin Transfers on Shardeum

**Send MockUSDC on Shardeum Testnet without holding ETH.**  
*A relayer pays gas, while a confidential risk engine (powered by INCO FHE) blocks malicious destinations.*

---

## ✨ **Key Features**

- 🔌 **MetaMask wallet integration**  
- 🌐 **Automatic Shardeum network enforcement**  
- 🛂 **ERC-20 approval flow** (allowance to relayer)  
- 🧠 **Confidential risk assessment**: LOW / MEDIUM / HIGH  
- ⛽ **Gasless transfers** via relayer (`transferFrom`)  
- 🔁 **Retry-safe backend** + RPC failure handling  
- 🎉 **Modern UI** (animations, overlays & confetti)  
- 🚀 **Deployable** on Vercel (Frontend) + Render (Backend)  

---

## 🧩 **How It Works**

### **Frontend Flow (React + Tailwind)**
1. User connects MetaMask
2. App enforces Shardeum Testnet (Chain ID:8119)
3. User approves MockUSDC spending to relayer address

### **Send Transaction**
recipient + amount → ENCRYPTED via INCO FHE → POST /api/send

**Example Payload:**
```json
{
  "sender": "0xUser",
  "recipient": "0xEncryptedRecipient",
  "amount": "0xEncryptedAmount"
}
```

### **Backend Flow (Express + Ethers v6)**
1. **DECRYPT** fields using INCO
2. **RISK ASSESSMENT** (LOW/MED/High)
3. **BLOCK** HIGH-RISK destinations
4. Relayer executes: `transferFrom(sender, recipient, amount)`
5. Returns **tx hash + explorer link + risk result**

---

## 🌍 **Network Details**

| **Item** | **Value** |
|----------|-----------|
| **Network** | Shardeum EVM Testnet |
| **Chain ID** | `8119` |
| **MockUSDC** | `0x1D782Be54c51c95c60088Ea8f7069b51F8E84142` |
| **Explorer** | [explorer-mezame.shardeum.org](https://explorer-mezame.shardeum.org) |

---

## 🧱 **Project Structure**

```
instapay/
├── backend/
│   ├── package.json
│   └── src/
│       ├── index.js ⭐
│       ├── chains.js
│       ├── config.js
│       ├── fhe.js (INCO)
│       ├── abi/MockUSDC.js
│       └── routes/transfer.js
└── frontend/
    ├── package.json
    ├── tailwind.config.js
    └── src/
        ├── App.js ⭐
        ├── fhe.js
        └── index.js
```

---

## 🔧 **Environment Variables**

### **Backend (.env)**
```env
PORT=3000
RELAYER_PRIVATE_KEY=0xYOUR_RELAYER_PRIVATE_KEY
SHARDEUM_RPC=https://your-shardeum-rpc
```

### **Frontend (.env)**
```env
REACT_APP_API_URL=https://your-backend.onrender.com
```

---

## 📡 **API Reference**

### **`POST /api/send`**

**Request:**
```json
{
  "sender": "0xUserAddress",
  "recipient": "0xEncryptedRecipient",
  "amount": "0xEncryptedAmount"
}
```

**✅ Success Response:**
```json
{
  "success": true,
  "txHash": "0x...",
  "etherscanTx": "https://explorer-mezame.shardeum.org/tx/0x...",
  "riskAssessment": { "level": "LOW", "checked": true }
}
```

**❌ Blocked (High Risk):**
```json
{
  "error": "Transaction blocked by security layer",
  "riskLevel": "HIGH",
  "reason": "Address flagged in confidential blacklist"
}
```

---

## 🧠 **Risk Engine**

| **Risk Level** | **Action** | **Description** |
|----------------|------------|-----------------|
| **HIGH** | ❌ **BLOCKED** | Blacklisted addresses |
| **MEDIUM** | ⚠️ **WARNING** | Allowed with caution |
| **LOW** | ✅ **SAFE** | Proceed normally |

*Fully server-side, protected by FHE boundaries*

---

## 🧪 **Local Setup**

### **Backend**
```bash
cd backend
npm install
npm start
```

### **Frontend**
```bash
cd frontend
npm install
npm start
```

---

## 🚀 **Production Deployment**

### **Backend → Render.com**
```
Root Directory: backend/
Build Command: npm install
Start Command: npm start
Health Check: GET /health
```

### **Frontend → Vercel**
```
Project Root: frontend/
Environment Variable: REACT_APP_API_URL=https://your-backend.onrender.com
Deploy: Click Deploy 🚀
```

---

## 🛡️ **Security Architecture**

| ✅ **Protected** | ❌ **Never Exposed** |
|------------------|---------------------|
| FHE Encryption | User Private Keys |
| Server Risk Check | User Secrets |
| Allowance-only Relayer | Direct Fund Access |

---

## 🧭 **Roadmap**

- [ ] **Multi-chain routing** (cheapest gas optimization)
- [ ] **On-chain encrypted blacklist**
- [ ] **Relayer balance monitoring dashboard**
- [ ] **Transaction history tracking**
- [ ] **Account Abstraction support**

---

## ❤️ **Tech Stack**

```
Frontend: React + Tailwind CSS
Backend: Express.js + Ethers v6
Security: INCO FHE (Fully Homomorphic Encryption)
Network: Shardeum EVM Testnet
Deployment: Vercel + Render
```

---

**⚡ InstaPay — Gasless. Secure. Instant.**

*Send stablecoins without gas, friction, or fear.*
