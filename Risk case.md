## Example Risk Assessment – SQL Database

###  To caculate the risk value for an asset, we will need 4 parameters: Asset, Likelihood, Impact, Control Effectiveness. 

# Risk – SQL Injection (Detailed Calculation)

## 1) Asset & Context
- **Asset:** SQL Database (Customer Orders & Payment Info)  
- **CIA Value (Asset Value):** **5** (High confidentiality, integrity, and availability)

> Assumption: The database stores sensitive customer and payment data and supports the online ordering system. Therefore, it is classified at the highest criticality level.

---

## 2) Threat Identification (STRIDE)
- **Threat Type:** Tampering, Information Disclosure  
- **Attack Vector Example:** Malicious input injected via web forms or APIs, allowing attackers to read, modify, or exfiltrate database content.

---


## 3) Likelihood 
We decompose likelihood into three measurable factors (0–1 scale), then map the composite score to a 1–5 rating.

**Factors (example values):**
- **Exploitability = 0.90**  
  SQLi exploits and automated tools are widely available(like sqlmap, even a script kiddie could use it).
- **Exposure = 0.80**  
  The front-end application is internet-facing with input fields(depends on how many web apps are connected to the database); input validation is incomplete.  
- **Threat Activity = 0.70**  
  SQL injection remains a frequent attack method (OWASP Top 10, DBIR reports).

**Composite Probability Calculation:**

 0.90 × 0.80 × 0.70= 0.504 

- **Composite Likelihood = 0.504**

**Mapping 0–1 to 1–5 scale (example thresholds):**
- 0.00–0.10 → 1 (Rare)  
- 0.10–0.30 → 2 (Unlikely)  
- 0.30–0.50 → 3 (Possible)  
- 0.50–0.70 → 4 (Likely)  
- 0.70–1.00 → 5 (Almost Certain)  

> Result: 0.504 falls into the **0.50–0.70 range → Likelihood = 4 (Likely)**

---

## 4) Impact
Impact is broken down into sub-factors (0–1), averaged, and then mapped to a 1–5 scale.

**Sub-factors (example values):**
- **Data Sensitivity = 1.00** (admin passwd)  
- **Business Criticality = 1.00** (system critical for operations)  
- **Regulatory Exposure = 0.90** (GDPR and financial compliance)  
- **Recovery Complexity = 0.80** (difficult to restore quickly)  

**Calculation:**

Sum = 1.00 + 1.00 + 0.90 + 0.80 = 3.70
Average = 3.70 / 4 = 0.925


**Mapping 0–1 to 1–5 scale:**
- 0.00–0.20 → 1 (Minor)  
- 0.20–0.40 → 2  
- 0.40–0.60 → 3  
- 0.60–0.80 → 4  
- 0.80–1.00 → 5 (Critical)  

> Result: 0.925 falls into the **0.80–1.00 range → Impact = 5 (Critical)**

---

## 5) Control Effectiveness – Combined Probability
We model multiple controls as independent probabilities. For each control with effectiveness `e_i`, the combined failure probability is Π(1 − e_i).  
Thus:  
`Overall Effectiveness = 1 − Π(1 − e_i)`

**Controls (example effectiveness values):**
- WAF rules = 0.25  
- Parameterized Queries (partially implemented) = 0.20  
- Vulnerability Scanning / Pentests = 0.15  
- Developer Training (limited) = 0.05  

**Step-by-step:**

(1 − 0.25) = 0.75
(1 − 0.20) = 0.80
(1 − 0.15) = 0.85
(1 − 0.05) = 0.95

Step A: 0.75 × 0.80 = 0.6000
Step B: 0.6000 × 0.85 = 0.5100
Step C: 0.5100 × 0.95 = 0.4845

Combined failure probability = 0.4845
Overall Control Effectiveness = 1 − 0.4845 = 0.5155


- **Control Effectiveness ≈ 0.52**

---

## 6) Final Risk Score Calculation
Formula:  
`Risk = Asset × Likelihood × Impact × (1 − ControlEffectiveness)`

**Step-by-step:**

Asset × Likelihood = 5 × 4 = 20
20 × Impact = 20 × 5 = 100

1 − ControlEffectiveness = 1 − 0.5155 = 0.4845

Risk = 100 × 0.4845 = 48.45


- **Final Risk Score = 48.45**

---

## 7) Risk Evaluation
**Thresholds (example):**
- 0–20 → Low (acceptable)  
- 21–40 → Medium (mitigate if feasible)  
- 41–75 → High (not acceptable, requires treatment)  
- >75 → Critical (immediate action)  

> Result: **48.45 → High**  
This risk exceeds tolerance and must be treated.

---

## 8) Recommendations
- **Short-term:**  
  1. Enforce parameterized queries across all applications.  
  2. Improve WAF rules and monitoring (reduce false negatives).  
  3. Integrate SAST/DAST into CI/CD pipelines.  
  4. Conduct targeted penetration testing on high-risk endpoints.  

- **Follow-up:**  
  Recalculate the risk score after improving controls (expected Control Effectiveness ≥ 0.7).
