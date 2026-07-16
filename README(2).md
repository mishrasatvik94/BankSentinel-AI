# 🛡️ BankSentinel FraudDNA

> **Detect the cyber-fraud attack chain before money moves.**

**BankSentinel FraudDNA** is an explainable, privacy-aware banking cybersecurity prototype for **FinSpark'26**. It correlates cybersecurity telemetry—such as suspicious logins, new devices, risky IPs, password resets and session activity—with transactional behaviour such as beneficiary creation, payment amount, velocity and customer history.

Instead of treating each alert as an isolated event, BankSentinel builds a **FraudDNA signature** representing the complete cyber-to-payment attack sequence and recommends an action before a suspicious payment is completed.

---

## 🎯 Problem Statement

### AI-Driven Correlation of Cybersecurity Telemetry & Transactional Behaviour

A banking fraud attempt is often an attack chain:

```text
Phishing / Malware
        ↓
Suspicious Login
        ↓
Password Reset or New Device
        ↓
New Beneficiary Added
        ↓
High-Value Payment Attempt
```

Cybersecurity monitoring and transaction-fraud systems are frequently analysed separately. This fragmentation can:

- Miss early warning signals before a payment
- Generate large numbers of disconnected alerts
- Increase false positives
- Delay fraud investigation and containment

BankSentinel correlates these signals into one explainable decision.

---

## 💡 Our Solution

BankSentinel combines four intelligence layers:

1. **Cyber Session Risk**  
   Login failures, new-device activity, browser/device change, risky IP, unusual location and session velocity.

2. **Transaction Anomaly**  
   Amount deviation, unusual transaction time, new beneficiary, payment velocity and customer baseline deviation.

3. **Graph Relationship Risk**  
   Connections among accounts, devices, IPs, beneficiaries and previously flagged entities.

4. **External Threat Risk**  
   Risky IP indicators, mule-account watchlists and threat-intelligence signals where available.

```text
Unified Risk Score
= Cyber Session Risk
+ Transaction Anomaly
+ Graph Relationship Risk
+ External Threat Risk
```

The decision engine recommends:

```text
ALLOW  |  VERIFY  |  HOLD  |  BLOCK  |  ESCALATE
```

---

## 🧬 What is FraudDNA?

**FraudDNA** is a structured and tokenised representation of the sequence, timing and relationships between cyber and payment events.

### Example FraudDNA

```text
PASSWORD_RESET
→ NEW_DEVICE
→ FIRST_TIME_BENEFICIARY
→ ₹90,000 PAYMENT
→ WITHIN_5_MINUTES
```

A FraudDNA signature can include:

- Ordered event sequence
- Time gaps between events
- Device novelty
- IP risk level
- Failed-login velocity
- Transaction deviation
- Beneficiary relationship risk
- Graph proximity to known suspicious entities

The goal is to detect the **connected attack pattern**, not merely a high-value transaction.

---

## ✨ Key Differentiators

- **CyberPayment Kill-Chain** — maps cybersecurity events directly to payment risk
- **FraudDNA Signature** — captures sequence, timing and relationships
- **Privacy-Aware Correlation** — uses tokenised customer/account identifiers
- **Telemetry Trust Score** — weighs signals according to reliability
- **Graph-Based Blast Radius** — discovers linked accounts, devices and beneficiaries
- **Explainable Decisions** — shows the top reasons behind every risk score
- **Pre-Payment Intervention** — recommends verification or containment before money moves
- **Human-in-the-Loop Controls** — high-impact actions remain reviewable and auditable

---

## 🏗️ Proposed Architecture

```mermaid
flowchart LR
    A[Banking Channels] --> D[API / Event Ingestion]
    B[Cybersecurity Telemetry] --> D
    C[Threat Intelligence] --> D

    D --> E[PII Tokenisation]
    E --> F[Feature Store]

    F --> G[FraudDNA Engine]
    F --> H[ML Risk Scoring]
    F --> I[Neo4j Graph Risk]

    G --> J[Decision Engine]
    H --> J
    I --> J

    J --> K[Customer Warning / Verification]
    J --> L[Fraud Analyst Dashboard]
    J --> M[SOC / Security Team]
    J --> N[Bank Admin Controls]

    L --> O[Analyst Feedback]
    O --> G
    O --> H
```

---

## 🔄 Functional Workflow

```mermaid
flowchart TD
    A[Login or Payment Event] --> B[Collect Cyber + Transaction Signals]
    B --> C[Tokenise Sensitive Identifiers]
    C --> D[Extract FraudDNA Pattern]
    D --> E[Calculate ML Anomaly Score]
    D --> F[Calculate Graph Relationship Risk]
    E --> G[Unified Risk Score]
    F --> G
    G --> H{Decision}
    H -->|Low Risk| I[Allow]
    H -->|Medium Risk| J[Step-Up Verification]
    H -->|High Risk| K[Hold / Escalate]
    H -->|Critical Risk| L[Block with Human Approval]
    I --> M[Feedback and Audit Log]
    J --> M
    K --> M
    L --> M
```

---

## 🧪 Illustrative Detection Scenario

A customer normally transfers **₹2,000–₹5,000**.

The system observes:

- Multiple failed login attempts
- Password reset
- Login from a new device
- Unusual IP or location
- First-time beneficiary
- ₹90,000 payment attempt within five minutes
- Beneficiary linked to a suspicious device/IP community

An isolated transaction model may flag only the amount. BankSentinel correlates the entire sequence and produces:

```text
Risk Score: 91 / 100
Recommended Action: HOLD + CUSTOMER VERIFICATION

Top Reasons:
1. New device immediately after password reset
2. Unusual IP and failed-login velocity
3. First-time beneficiary
4. High-value payment within five minutes
5. Graph link to risky device/IP community
```

---

## 🛠️ Technology Stack

| Layer | Technology |
|---|---|
| Dashboard | Streamlit initially; React can be added later |
| Backend APIs | FastAPI, Python |
| Machine Learning | Isolation Forest, XGBoost / rule ensemble |
| Graph Intelligence | Neo4j |
| Event and Case Storage | PostgreSQL |
| Deployment | Docker |
| Security | RBAC, encryption, tokenisation, immutable audit logs |
| Prototype Data | PaySim, IBM AMLSim and synthetic cyber-event data |

---

## 📊 Prototype Modules

| Module | Purpose | Status |
|---|---|---|
| Synthetic Event Generator | Produces login, device, session and transaction events | Planned / In Development |
| FraudDNA Engine | Converts event chains into structured signatures | Planned / In Development |
| Risk Scoring API | Combines cyber, transaction and graph risk | Planned / In Development |
| Neo4j Fraud Graph | Connects account, device, IP and beneficiary entities | Planned / In Development |
| Analyst Dashboard | Shows score, reasons, timeline and graph | Planned / In Development |
| Decision Engine | Recommends allow, verify, hold, block or escalate | Planned / In Development |
| Audit and Feedback | Records analyst decisions for improvement | Planned / In Development |

> Update the status column as modules are completed. Do not label planned features as implemented.

---

## 📁 Recommended Repository Structure

```text
BankSentinel-AI/
├── README.md
├── LICENSE
├── .gitignore
├── docker-compose.yml
├── requirements.txt
├── data/
│   ├── sample/
│   └── synthetic/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── api/
│   │   ├── models/
│   │   ├── services/
│   │   │   ├── frauddna_engine.py
│   │   │   ├── risk_engine.py
│   │   │   └── graph_service.py
│   │   └── security/
│   └── tests/
├── dashboard/
│   ├── app.py
│   ├── pages/
│   └── components/
├── ml/
│   ├── notebooks/
│   ├── training/
│   └── artifacts/
├── graph/
│   ├── schema.cypher
│   └── seed_data.cypher
├── scripts/
│   ├── generate_synthetic_events.py
│   └── seed_demo.py
├── docs/
│   ├── architecture.md
│   ├── data_dictionary.md
│   ├── threat_model.md
│   └── screenshots/
└── demo/
    └── demo-video-link.md
```

---

## 🚀 Local Development Setup

> The commands below assume the project files have been added according to the proposed structure.

### 1. Clone the repository

```bash
git clone https://github.com/mishrasatvik94/BankSentinel-AI.git
cd BankSentinel-AI
```

### 2. Create a virtual environment

```bash
python -m venv .venv
```

Activate it:

```bash
# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure environment variables

Create a `.env` file from `.env.example`:

```env
POSTGRES_URL=postgresql://user:password@localhost:5432/banksentinel
NEO4J_URI=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=change_me
SECRET_KEY=replace_with_a_secure_random_value
```

Never commit real credentials or customer data.

### 5. Start the backend

```bash
uvicorn backend.app.main:app --reload
```

### 6. Start the dashboard

```bash
streamlit run dashboard/app.py
```

### 7. Optional Docker setup

```bash
docker compose up --build
```

---

## 🔐 Security and Privacy Principles

- Customer and account identifiers are tokenised
- Raw PII is not required for prototype model training
- Sensitive data is encrypted in transit and at rest
- Analyst and administrator access is role-based
- Every alert, decision and action is recorded in an audit log
- Payment holds or blocks require policy-based human oversight
- Data minimisation and retention controls are applied
- Models provide explainable reasons instead of blind scores
- Drift, latency and false-positive rates are monitored
- Prototype demonstrations use synthetic or anonymised data only

---

## 📈 Evaluation Plan

The MVP should be evaluated on:

- Detection precision and recall
- False-positive rate
- Risk-scoring latency
- Graph-query latency
- Explainability of alert reasons
- Analyst decision time
- Model drift
- Percentage of fraud chains detected before payment completion

### Pilot Targets

These are **targets**, not achieved production results:

- Risk-scoring response below **500 ms**
- Explainable top risk reasons for every alert
- Shadow-mode operation before any automated payment action
- Measurable reduction in disconnected SOC and fraud alerts
- Improved detection of mule-linked beneficiaries and shared devices

---

## 🗺️ Adoption Roadmap

### Phase 1 — Sandbox MVP
- Synthetic and anonymised data
- FraudDNA extraction
- Unified risk score
- Dashboard and graph visualisation

### Phase 2 — Bank Pilot
- Read-only login and transaction-log integration
- Shadow-mode scoring
- Comparison with analyst labels
- No automated payment blocking

### Phase 3 — Controlled Intervention
- Step-up verification
- Policy-controlled payment holds
- Analyst feedback loop
- Model and rule versioning

### Phase 4 — Network Intelligence
- Privacy-safe FraudDNA pattern exchange
- Consortium governance
- Cross-institution fraud-network detection
- Telecom-supported or verified external signals where available

---

## ⚠️ Current Scope and Limitations

The prototype does **not** claim to provide:

- Telecom-level SIM-swap detection
- Production-grade device fingerprinting unless explicitly implemented
- A trained Digital Arrest scam-specific model
- Live cross-bank customer-data exchange
- Fully autonomous payment blocking
- Production performance or regulatory certification

These capabilities remain part of the future roadmap and require authorised banking, telecom, legal and governance partnerships.

---

## 📚 Research and Design References

The following open-source projects informed the architecture and data strategy. Their code is not claimed as BankSentinel's implementation.

- [Neo4j IEEE-CIS Fraud Graph](https://github.com/neo4j-field/finance-ieee-cis-fraud)
- [Neo4j Fraud Demo](https://github.com/neo4j-product-examples/demo-fraud)
- [IBM AMLSim](https://github.com/IBM/AMLSim)
- [Jube AML and Fraud Transaction Monitoring](https://github.com/jube-home/aml-fraud-transaction-monitoring)
- [NebulaGraph Fraud Detection GNN](https://github.com/wey-gu/NebulaGraph-Fraud-Detection-GNN)
- [UGFraud](https://github.com/safe-graph/UGFraud)

---

## 👥 Team

| Member | Role |
|---|---|
| **Satvik Kumar Mishra** | Team Lead and Solution Architecture |
| **Jatin Kumar** | Backend and Data Pipeline |
| **Add Member** | AI/ML and Cybersecurity |
| **Add Member** | Frontend and Visualisation |

Replace the remaining placeholders before final submission.

---

## 🏆 Hackathon

Developed for **FinSpark'26 — India's Banking Cybersecurity Innovation Hackathon**.

**Selected Problem Statement:**  
AI-Driven Correlation of Cybersecurity Telemetry & Transactional Behaviour

---

## 📄 License

Add an appropriate license before accepting external contributions. For a hackathon prototype, the **MIT License** is a common option, but choose it only after the team agrees on ownership and reuse permissions.

---

## 📬 Contact

- **GitHub:** [mishrasatvik94](https://github.com/mishrasatvik94)
- **Repository:** [BankSentinel-AI](https://github.com/mishrasatvik94/BankSentinel-AI)

---

<p align="center">
  <strong>BankSentinel FraudDNA</strong><br/>
  Detect the attack pattern before it becomes a financial loss.
</p>
