# 📌 Understanding the Architecture – TerraPay FinTech Setup

## 1️⃣ EIG Server
**Purpose:** Hosts APIs and services that partners connect to for exchanging financial data or initiating transactions.

**Directory Setup:**
```
/prd/EIG_OUT/SSL/client_ssls
```
A secure directory for storing SSL certificates, likely used for encrypting communication with partners.

**FinTech Context:**  
EIG servers are backend systems handling partner interactions, such as:
- API calls for fund transfers
- Account validation
- Transaction reporting

---

## 2️⃣ Jump Server
**Purpose:** Acts as a secured gateway for accessing other servers in the architecture.

**Directory Setup:**
```
/prd/utils/setupiptables.sh
```
A script for managing `iptables` firewall rules.

**FinTech Context:**  
- Isolates access to critical servers (EIG, VPN, TXN, RPT)  
- All administrative access is routed through this server for security

---

## 3️⃣ VPN Server
**Purpose:** Handles VPN tunnels, enabling secure communication between the organization's servers and external partners.

**Directory Setup:**
```
/etc/ipsec.d/
```
Contains IPsec-related configurations and secrets.

**Technology:**  
- **Libreswan** → Open-source IPsec VPN implementation for site-to-site or client-to-site VPNs.

**FinTech Context:**  
Ensures secure transmission of sensitive financial data between:
- Partners ↔ Internal systems

---

## 4️⃣ All Tunnels Directory Setup
**Purpose:**
```
/root/vpnsetup/setupiptables.sh
```
Script for configuring `iptables` firewall rules related to VPN tunnels.

**FinTech Context:**  
- Automates firewall rule setup  
- Allows only **whitelisted partner IPs** through VPN  
- Prevents unauthorized access

---

## 5️⃣ Partner Connection to EIG Server
**Purpose:**  
The partner connects to the EIG server to access APIs for payment processing, validation, or other services.

**Security:**
- **IP Whitelisting:** Only specific partner IPs can connect.
- **SSL Authentication:** Mutual SSL/TLS ensures encrypted and verified communication.

**FinTech Context:**  
Partners interact via APIs for:
- Remittances
- Payments
- Transaction queries

---

## 6️⃣ Transaction (TXN) Server
**Purpose:** Processes financial transactions.

**FinTech Context:**  
- Manages secure transaction flows
- Interfaces with payment gateways and banks
- Ensures compliance and reliability

---

## 7️⃣ Reporting (RPT) Server
**Purpose:** Generates reporting and analytics for financial transactions.

**FinTech Context:**  
- Provides transaction reports to partners
- Delivers settlement details
- Supports compliance metrics

---

## 8️⃣ How These Components Work Together
1. **Partner → VPN Server:** Partner connects via secure VPN tunnel.  
2. **VPN Server → EIG Server:** Forwards API traffic to EIG server.  
3. **EIG Server ↔ TXN Server:** EIG requests transaction processing.  
4. **TXN Server → RPT Server:** Transaction logs stored for reporting.  
5. **Jump Server:** Used by admins for secure server management.

---

## 9️⃣ How Send Partner & Receive Partner Connect and Work

### **Send Partner → TerraPay**
- A **Send Partner** (e.g., PayFast in India) initiates a money transfer request through their app or API.  
- The request is sent **via secure VPN + whitelisted IP** to TerraPay’s **EIG Server**.  
- TerraPay verifies:
  - API credentials
  - SSL certificates
  - Transaction details (KYC, compliance)
- TerraPay converts currency if needed and forwards the transaction to the Receive Partner.

---

### **TerraPay → Receive Partner**
- TerraPay sends an **API call** to the Receive Partner’s system (e.g., MobiCash in Kenya) via:
  - Secure VPN tunnel (if applicable)
  - SSL encryption
  - Partner whitelisted IPs
- The Receive Partner:
  - Validates the account/wallet
  - Credits the beneficiary
  - Sends **Receiver Callback** to TerraPay with transaction status

---

### **Callbacks Flow**
- **Receiver Callback:** From Receive Partner → TerraPay with status (SUCCESS/FAILURE)  
- **Sender Callback:** From TerraPay → Send Partner to update sender about transaction status  

---

## 🔗 Example Flow – Combined Architecture + Partner Communication
```
Sender (India) --[Send Partner: PayFast]--> VPN --> EIG Server --> TXN Server
                                                    ↓
                                               RPT Server
                                                    ↓
                                          VPN/SSL --> Receive Partner: MobiCash (Kenya)
                                                    ↓
                                             Beneficiary Wallet Credited
```
```
Receiver Callback (MobiCash → TerraPay) → Sender Callback (TerraPay → PayFast)
```
---

**Security Essentials for Cloud/Network Engineers:**
- Maintain **partner IP whitelists**  
- Ensure **SSL/TLS certificates** are valid and renewed  
- Monitor **VPN tunnel uptime**  
- Use **firewall rules** to block unauthorized traffic  
- Perform **API and packet-level troubleshooting** when needed
