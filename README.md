# PortalBridge Protocol

### Revolutionary Cross-Chain Infrastructure for the Bitcoin–Stacks Ecosystem

---

## 📖 Overview

**PortalBridge** is a decentralized cross-chain gateway that securely connects Bitcoin’s base layer with the **Stacks Layer 2 smart contract ecosystem**.
It enables **trustless asset transfers**, powered by validator-based consensus, advanced cryptographic proof validation, and automated Clarity-based settlement logic.

PortalBridge is designed for:

* **Enterprises** requiring institutional-grade security.
* **Developers** seeking programmable Bitcoin liquidity.
* **Users** demanding fast, seamless asset mobility between Bitcoin and Stacks.

---

## 🚀 Key Features

* **Trustless Cross-Chain Transfers** – Bitcoin deposits and withdrawals managed via validator consensus.
* **Validator Governance** – Multi-validator approval for deposits ensures decentralization and security.
* **Automated Settlement** – Smart contract–driven deposit validation and withdrawal execution.
* **Emergency Controls** – Pause/resume bridge, validator management, and asset recovery mechanisms.
* **Comprehensive Auditability** – On-chain records of deposits, confirmations, and validator signatures.

---

## 🏗️ System Overview

At a high level, PortalBridge operates in three stages:

1. **Deposit Initiation (BTC → Stacks)**

   * User sends BTC on-chain.
   * Validators verify the Bitcoin transaction and register the deposit in PortalBridge.

2. **Deposit Confirmation**

   * Validators provide multi-signature approvals.
   * Once the required confirmations are reached, the equivalent assets are credited to the user’s Stacks account.

3. **Withdrawal (Stacks → BTC)**

   * User initiates a withdrawal by specifying BTC recipient address.
   * Bridge burns/deducts the bridged assets on Stacks.
   * Off-chain relayers (validators) release BTC to the recipient.

---

## 📐 Contract Architecture

The PortalBridge Clarity contract is structured around **security-first primitives**:

### 1. **Traits**

* `bridgeable-token-trait` – defines the interface for supported tokens (transfer & balance retrieval).

### 2. **Error Codes**

* Comprehensive error constants (e.g., unauthorized access, invalid amounts, invalid signatures, paused bridge).

### 3. **Protocol Constants**

* **Deposit thresholds**: `MIN-DEPOSIT-AMOUNT`, `MAX-DEPOSIT-AMOUNT`.
* **Consensus requirement**: `REQUIRED-CONFIRMATIONS`.
* **Contract deployer**: primary administrator.

### 4. **State Variables**

* `bridge-paused` – operational status.
* `total-bridged-amount` – cumulative bridged assets.
* `last-processed-height` – last known processed block.

### 5. **Core Maps**

* **Deposits** – stores deposit details (amount, recipient, confirmations, BTC sender, status).
* **Validators** – tracks authorized validator addresses.
* **Validator Signatures** – records multi-signature confirmations.
* **Bridge Balances** – maintains user bridged asset balances.

### 6. **Administrative Functions**

* Initialize, pause/resume bridge.
* Add/remove validators.
* Emergency withdrawals (fund recovery).

### 7. **Core Bridge Functions**

* `initiate-deposit` – validator records BTC deposit.
* `confirm-deposit` – validators confirm via signatures.
* `withdraw` – users withdraw to BTC.

### 8. **Read-Only Queries**

* Retrieve deposit info, validator status, user balances, and bridge status.

### 9. **Validation Helpers**

* Validate principals, BTC addresses, tx-hash format, cryptographic signatures, and deposit amount ranges.

---

## 🔄 Data Flow

**Deposit Path (BTC → Stacks)**

```
User → Bitcoin Tx → Validator Detection → initiate-deposit → confirm-deposit → 
Bridge Credits Balance → User Receives Bridged Assets
```

**Withdrawal Path (Stacks → BTC)**

```
User → withdraw() → Balance Deduction → Event Log → Validator Relayers Release BTC
```

**Administrative Path**

```
Deployer → add-validator / remove-validator / pause-bridge / emergency-withdraw
```

---

## 🛡️ Security Considerations

* **Validator Set Control** – Only the contract deployer can manage validators.
* **Emergency Pause** – Can halt operations during anomalies or detected attacks.
* **Multi-Signature Requirements** – Prevents single-validator compromise.
* **Deposit & Withdrawal Limits** – Protects against overflow or excessive fund movement.

---

## 📊 Contract Metrics

* **Total Bridged Assets** – `total-bridged-amount`.
* **Deposit Records** – Fully auditable per Bitcoin transaction.
* **Validator Actions** – Each signature logged on-chain.
* **Bridge Status** – Real-time active/paused state.

---

## 🛠️ Usage Examples

### Check bridge status

```clarity
(contract-call? .portal-bridge get-bridge-status)
```

### Get deposit details by Bitcoin tx-hash

```clarity
(contract-call? .portal-bridge get-deposit 0x1234...)
```

### Initiate a deposit (validator only)

```clarity
(contract-call? .portal-bridge initiate-deposit tx-hash amount recipient btc-sender)
```

### Confirm a deposit with signature (validator only)

```clarity
(contract-call? .portal-bridge confirm-deposit tx-hash signature)
```

### Withdraw assets to Bitcoin

```clarity
(contract-call? .portal-bridge withdraw amount btc-recipient)
```

---

## 📌 Future Extensions

* **Decentralized Validator Governance** – DAO-managed validator onboarding/removal.
* **zk-Proof Based Verification** – Replace validator trust with cryptographic proofs.
* **BTC Ordinals / Runestones Support** – Expand bridgeable asset classes.
* **Liquidity Pools** – Enable instant swaps without validator confirmation latency.

---

## 📜 License

MIT License – open source and free to build upon.
