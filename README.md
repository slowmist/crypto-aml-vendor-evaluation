# Crypto AML Vendor Evaluation Checklist & Implementation Guide


*Read this in other languages: [English](README.md), [简体中文](README_CN.md).*

As a threat intelligence company focused on blockchain ecosystem security, SlowMist has found through long-term hacker tracking and anti-money laundering (AML) practices that selecting the right AML vendor is not only critical for meeting compliance requirements, but also directly impacts an institution’s ability to manage illicit fund risks.

Based on SlowMist’s accumulated threat intelligence and AML tracing experience, and with reference to requirements from the FATF, the Wolfsberg Group, and major jurisdictions (such as FinCEN, HKMA, and MAS), this guide aims to provide the industry with a Crypto AML vendor evaluation framework that balances compliance and real-world effectiveness.

## 1. Evaluation Objectives

Ensure that the vendor not only meets basic regulatory reporting requirements, but also possesses high-precision risk identification capabilities in complex adversarial scenarios (e.g., mixing, cross-chain transactions, and laundering attacks), achieving a balance between low false positives and low false negatives.

---

## 2. Core Capability Evaluation

### 2.1 Sanctions & Blacklist Screening

*Key focus: data timeliness, coverage, and the accuracy of fuzzy matching algorithms.*

- [ ] **Coverage Scope**
  - Does it cover major global sanctions lists (OFAC, UN, EU, UK HMT, DFAT, etc.)?
  - Does it include crypto-related blacklists (e.g., OFAC-designated crypto addresses, hacker-related addresses, darknet addresses)?

- [ ] **Update Frequency**
  - Are lists updated in real time or at least daily?
  - How quickly does the vendor respond to emergency sanctions events (e.g., geopolitical conflicts triggering urgent sanctions)?

- [ ] **Matching Algorithms**
  - Does it support fuzzy matching to handle misspellings, aliases, or variations?
  - Can matching thresholds be adjusted to balance false positives?

- [ ] **Clustering Capability**
  - Can it identify associated addresses linked to sanctioned entities?

---

### 2.2 On-chain Transaction Monitoring

*Key focus: accuracy of risk attribution and fund flow tracing capability.*

- [ ] **Asset Coverage**
  - Does it support major public blockchains (BTC, ETH, Solana, Tron, etc.) and Layer 2 networks?
  - Does it support mainstream token standards (ERC-20, TRC-20, etc.)?

- [ ] **Risk Scoring Model**
  - Is it based on multi-dimensional analysis (direct risk vs. indirect/propagated risk)?
  - Does it support hop-based analysis? (e.g., can it detect funds reaching a high-risk entity after multiple hops?)

- [ ] **Entity Identification**
  - What is the scale and quality of labeled entities (exchanges, mixers, darknet markets, ransomware, DeFi protocols, etc.)?
  - How are labels updated? (manual intelligence vs. automated ingestion)

- [ ] **DeFi & NFT Monitoring**
  - Are there dedicated monitoring rules for DEXs, liquidity pools, and cross-chain bridges?
  - Can it identify NFT-related laundering patterns (e.g., wash trading)?

- [ ] **Rule Engine Flexibility**
  - Can users define custom monitoring rules (e.g., single transaction > 1 BTC AND from high-risk jurisdictions)?

---

### 2.3 Source & Destination of Funds Analysis

- [ ] **Fund Tracing**
  - Does it provide visualized fund flow graphs?
  - Can it detect mixing behavior and layering patterns?  
    *(Note: full de-anonymization is difficult, but detection capability is essential.)*

- [ ] **Travel Rule Support**
  - Does it support IVMS 101 standards?
  - Does it integrate with major Travel Rule protocols (e.g., TRISA, Sygna, VerifyVASP)?

---

### 2.4 Continuous Monitoring & Automatic Re-screening

*Key focus: retrospective risk recalculation when underlying data changes.*

- [ ] **Trigger Mechanism**
  - When an address’s associated entity label changes from “low risk” to “high risk/sanctioned,” can the system automatically recalculate historical transaction risk scores?
  - After updates to underlying data sources or sanctions lists (e.g., OFAC), how quickly can the system re-scan affected historical data?

- [ ] **Scope & Propagation Depth**
  - Does re-screening propagate downstream (based on configurable hop levels) and trigger retrospective alerts?

- [ ] **Alert Noise Reduction**
  - Are retrospective alerts clearly distinguished from real-time alerts, or managed under separate case priorities?  
    *(to avoid overwhelming normal alert workflows)*

---

### 2.5 Reporting & Case Management

- [ ] **SAR/STR Assistance**
  - Can it automatically generate transaction summaries required for Suspicious Activity Reports (SAR)?
  - Do report formats comply with local regulators (e.g., FinCEN, JFIU)?

- [ ] **Audit Trail**
  - Are all system actions (alert handling, notes, whitelist changes) immutably logged?

- [ ] **API Integration**
  - Is the API documentation complete and clear?
  - Can throughput (TPS) support peak business demand?

---

### 2.6 Data Privacy & Security

- [ ] **Certifications**
  - Does it hold certifications such as SOC 2 Type II, ISO 27001?

- [ ] **Deployment Options**
  - Does it support on-premise or private cloud deployment (for institutions with strict compliance requirements)?

---

## 3. Evaluation Methodology

To avoid relying solely on vendor demo environments, it is recommended to conduct a **POC (Proof of Concept)** using the following approach:

---

### Phase 1: Static Data Testing

**Objective**: Evaluate database coverage and labeling accuracy.

#### 1. Prepare Datasets

##### (1) Known High-Risk Address Set

*Used to evaluate the system’s ability to identify hackers, sanctioned entities, and illicit addresses.*

**Example data sources:**

- **Sanctions & Risk Entity Databases**
  - OpenSanctions — CryptoWallet dataset  
    https://www.opensanctions.org/search/?schema=CryptoWallet  

- **Public Hack & Theft Address Repositories**
  - Lazarus / Bluenoroff research  
    https://github.com/tayvano/lazarus-bluenoroff-research/tree/main/hacks-and-thefts  
  - ScamSniffer Scam Database  
    https://github.com/scamsniffer/scam-database/tree/main/blacklist  

---

##### (2) Known Clean Address Set

*Used to evaluate false positives (i.e., incorrectly flagged legitimate addresses).*

**Example data sources:**

- **Tagged CEX Addresses**
  - Arkham Intelligence — CEX labels  
    https://intel.arkm.com/tags/cex  

- **Blockchain Explorer Labels**
  - Exchange-tagged addresses (e.g., Etherscan)

---

##### (3) Grey Address Set

*Used to evaluate detection of “risk tendency” rather than confirmed illicit activity.*

**Typical categories:**

- Gambling-related addresses  
  https://etherscan.io/accounts/label/gambling  

- High-risk exchanges or regulatory watchlist entities  

- Mixer interaction addresses  

---

#### 2. Execute Testing

- Import the above datasets into the vendor system.

#### 3. Evaluation Metrics

- **Recall**: How many high-risk addresses are correctly identified?
- **False Positive Rate**: How many clean addresses are incorrectly flagged?

---

### Phase 2: Dynamic Transaction Monitoring Testing

**Objective**: Evaluate rule engine flexibility and real-time performance.

#### 1. Simulated Scenarios

- **Scenario A: Structuring**
  - Generate multiple transactions slightly below reporting thresholds within a short period.

- **Scenario B: Mixer Interaction**
  - Send small amounts to mixer contracts (e.g., Tornado Cash) and withdraw.

- **Scenario C: Multi-hop Risk Propagation**
  - High-risk address → intermediary A → intermediary B → your test address

#### 2. Execute Testing

- Conduct real transactions on testnet or mainnet.

#### 3. Evaluation Metrics

- **Alert Latency**: How long after confirmation is an alert triggered?
- **Risk Propagation**: Can indirect risk in Scenario C be detected?

---

### Phase 3: API & Integration Testing

**Objective**: Evaluate production readiness.

- **Stress Testing**
  - Simulate high-concurrency requests and observe latency and error rates.

- **Failure Recovery**
  - Simulate API interruptions and evaluate retry mechanisms and data consistency.

---

### Phase 4: Due Diligence

- **Industry Reputation & Case Studies**
  - Request 2–3 customer cases from similar business types or scales.

- **Regulatory Engagement**
  - Ask whether the vendor has direct cooperation with regulators  
    *(this often indicates regulatory acceptance of their data)*

---

## 4. Sample Scoring Card

| Evaluation Dimension | Weight | Vendor A Score (1–10) | Vendor B Score (1–10) | Notes |
|---------------------|--------|----------------------|----------------------|-------|
| **Data Quality** | 30% | | | Label accuracy, update speed |
| **Feature Completeness** | 25% | | | Clustering, cross-chain tracing |
| **Usability** | 15% | | | UI/UX, case workflow |
| **Technical Performance** | 15% | | | API latency, stability |
| **Cost** | 10% | | | Setup fees, API pricing |
| **Support & Service** | 5% | | | Response time, training |
| **Total** | **100%** | | | |

---

> **Final Note**  
> While business models, regulatory requirements, and risk exposure vary significantly across institutions—and there is no single “best” solution—the underlying logic of risk control remains consistent.  
>  
> Based on SlowMist’s frontline threat intelligence and AML tracing experience, this evaluation framework is designed to help organizations identify AML vendors that not only meet compliance requirements, but also effectively defend against complex on-chain risks.