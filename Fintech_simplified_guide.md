# Fintech - Send/Receive Partners & Callbacks (Simplified Guide)

## The Players
- **Sender** → The person sending the money.
- **Send Partner** → The company/app that collects money from the sender (e.g., Western Union, a bank, or a mobile app).
- **Platform (Fintech)** → The middleman tech platform that moves money securely and instantly between partners.
- **Receive Partner** → The company/bank that gives the money to the receiver in their country.
- **Callbacks** → Automatic messages between systems to update on status.

---

## How It Works — Step by Step

**Example:** Alice in the UK sends ₹10,000 to her mom in India.

1. **Sender → Send Partner**  
   Alice goes to her UK remittance app (Send Partner) and pays them £100.

2. **Send Partner → Fintech**  
   The Send Partner sends the transaction details (Alice, amount, recipient bank account) to Fintech’s API.

3. **Fintech → Receive Partner**  
   Fintech checks everything (KYC, fraud checks, FX rate) and sends payout instructions to the Receive Partner in India.

4. **Receive Partner → Recipient**  
   The Receive Partner deposits ₹10,000 into Alice’s mom’s bank account.

---

## Callbacks
- **Receiver Callback:** Receive Partner tells Fintech “Payout Success.”
- **Sender Callback:** Fintech tells Send Partner “Your transaction succeeded.”  
  Send Partner then tells Alice “Money delivered.”

---

## If Things Go Wrong
- If the bank rejects the payout → Receive Partner tells Fintech → Fintech tells Send Partner → Money is refunded.
- If callback fails (e.g., Send Partner’s system is down) → Fintech retries later until the message is delivered.

---

## How Fintech Makes Money
1. **Fee per transaction** (e.g., ₹30 charged to Send Partner or sender).
2. **FX margin** — They give a slightly lower exchange rate than market and keep the difference.
3. **Incentives from partners** for sending volume.
4. **Interest on funds** held temporarily.

---

**In short:**
- Send Partner brings money into the system.
- Receive Partner takes money out of the system.
- Fintech is the smart middleman making sure it’s secure, compliant, and instant — and they make money from fees & FX margin.
