# Terrapay — Send/Receive Partners, Callbacks, and How the Business Makes Money

**Purpose:** This guide explains how *send partners* and *receive partners* interact with a payment/remittance platform (like TerraPay), how sender/receiver callbacks work in real-time, and the technical and operational patterns used in production. It also covers revenue models, settlement, security, monitoring, and troubleshooting. This is written for cloud/network engineers and integration teams.

---

## Table of Contents
1. Overview — actors & terminology
2. Real-time end-to-end scenarios (happy path + failure modes)
3. Technical architecture & network flows
4. API & callback designs (examples + idempotency)
5. Security, whitelisting, and network setup
6. Settlement, reconciliation & liquidity
7. How the platform (e.g., TerraPay) earns money
8. Monitoring, observability, and runbook snippets
9. Appendix: JSON examples, headers, sample Tomcat/nginx config, acronyms

---

## 1) Overview — actors & terminology

- **Sender**: The end customer initiating the payment (person/app). 
- **Send Partner**: The external entity (app, bank, agent) that collects funds from the Sender and calls the platform to originate the transfer.
- **Platform (TerraPay)**: The central clearing/processing layer that validates, routes, converts currency, and instructs payout.
- **Receive Partner**: The external entity that executes the payout (bank, wallet provider, cash pay-out agent).
- **Sender Callback**: HTTP(S) endpoint on Send Partner that the platform calls to notify of final status or updates for the sender side.
- **Receiver Callback**: HTTP(S) endpoint on Receive Partner called to instruct payout and to notify status updates.
- **EIG_IN / EIG_OUT**: Common naming in integration systems — *EIG_IN* receives partner-originated transactions; *EIG_OUT* sends callbacks/notifications back to partner.

... (Content continues as in our earlier markdown document) ...
