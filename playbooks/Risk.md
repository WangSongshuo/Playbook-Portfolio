# Risk Management Playbook

## 1. Objective
Provide a structured approach for identifying, analyzing, evaluating, and managing risks related to information assets.  
Ensure risks are properly documented, assessed, and either mitigated or accepted based on organizational risk appetite.

---

## 2. Risk Identification
- **Asset Classification**  
  - Identify critical assets (systems, data, services).  
  - Assign value based on **CIA triad** (Confidentiality, Integrity, Availability).  
    - High confidentiality data (e.g., customer PII).  
    - High integrity assets (e.g., financial systems).  
    - High availability services (e.g., online portals).  

- **Threat Identification**  
  - Use **frameworks like STRIDE** (Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, Elevation of Privilege).  
  - Map potential risks to assets.  

- **Control Review**  
  - Identify existing technical/organizational controls (e.g., firewalls, backups, access controls).  
  - Estimate **protection level** (low, medium, high).  

---

## 3. Risk Analysis
- **Risk Formula**  
- **Asset Value** → Score based on CIA (1–10).  
- **Risk Factor** → Defined by **Likelihood × Impact**.  
  - Likelihood: Rare (1) → Certain (5).  
  - Impact: Minor (1) → Critical (5).  
- **Protection Factor** → Lower score if strong controls exist.  

- **Scoring**  
- Example: Database with customer data  
  - Asset Value = 9  
  - Likelihood = 4 (high)  
  - Impact = 5 (critical)  
  - Protection Factor = 0.8 (strong access control)  
  - **Risk Score = 9 × (4×5) × 0.8 = 144**  

- **Thresholds**  
- ≤ 20 → Low risk (acceptable).  
- 21–40 → Medium risk (monitor/mitigate).  
- > 40 → High risk (not acceptable, requires treatment).  

---

## 4. Risk Evaluation
- Compare calculated risk scores against **risk appetite & tolerance levels**.  
- Decision points:  
- **Acceptable Risk** → Document, monitor periodically.  
- **Unacceptable Risk** → Escalate for treatment.  

- Evaluation matrix:  
| Risk Score | Category | Decision |
|------------|----------|----------|
| 0–20       | Low      | Accept |
| 21–40      | Medium   | Mitigate if feasible |
| >40        | High     | Not acceptable → Treat immediately |

---

## 5. Risk Treatment
- For unacceptable risks, determine strategy:  
- **Mitigate** → Implement additional controls (patching, network segmentation, MFA).  
- **Transfer** → Insurance, outsourcing.  
- **Avoid** → Stop risky process or service.  
- **Accept** → With management approval, document justification.  

---

## 6. Monitoring & Review
- Regularly update asset inventory, threats, and control effectiveness.  
- Re-assess risks when:  
- New systems are introduced.  
- Major security incidents occur.  
- Regulations change.  

---

## 7. Example Case
- **Asset**: Customer Database  
- **Identified Threat**: SQL Injection (Tampering, Information Disclosure).  
- **Existing Control**: WAF + parameterized queries.  
- **Risk Calculation**:  
- Asset Value = 9  
- Likelihood = 4  
- Impact = 5  
- Protection Factor = 0.8  
- Risk = 144 → High Risk  
- **Evaluation**: Not acceptable.  
- **Treatment**: Additional code review + query logging + SOC alerting.  
