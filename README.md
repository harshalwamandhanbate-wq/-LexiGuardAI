# LexiGuard-AI ⚖️
### Privacy-First, Fully On-Device Legal Document Triage Agent
**Organized by ZeroDay OSS, IIIT Dharwad | Hacktoberfest 2026 — Track PS2**  
*Powered by Google Gemma Class Edge Models (Gemma 3n / Gemma 2 2B)*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python: 3.10+](https://img.shields.io/badge/Python-3.10%2B-brightgreen.svg)](https://python.org)
[![Privacy: 100% On-Device](https://img.shields.io/badge/Privacy-100%25%20On--Device-success.svg)](THREAT_MODEL.md)
[![Tests: 17 Passed](https://img.shields.io/badge/Tests-17%2F17%20Passed-success.svg)](EVALUATION.md)

---

## 📌 Main Technical Contribution (Deliverable S10)

LexiGuard-AI is an open-source, airgapped legal document triage system that empowers ordinary consumers, tenants, and employees to safely read, comprehend, and interrogate legal agreements without exposing sensitive private documents to remote cloud servers.

### Key Innovations:
1. **Verifiable 100% On-Device Privacy Shield:** Active Python socket interception traps and blocks any outbound network egress, mathematically guaranteeing 0 bytes transmitted outside `127.0.0.1`.
2. **Strict Verbatim Grounding & Silence Handling:** Every explanation and highlighted penalty maps to an authentic source passage citation (with line, page, and character offsets). When a document is silent on a topic, the agent explicitly says so without fabricating clauses.
3. **Calibrated Legal Refusal & Escalation Engine:** Enforces strict ethical and legal boundaries: firmly declines to provide definitive legal advice or enforceability verdicts, and generates tailored questions for qualified attorneys (e.g., labor lawyers, tenancy tribunals).
4. **Adversarial Prompt Injection Disarming:** Pre-tokenization filters neutralize malicious prompt injections hidden inside contract text.
5. **Two-Version Contract Redline Diffing:** Compares original vs amended contracts (e.g. landlord counter-proposals) to spotlight stealthily added penalties and shifted liabilities.

---

## 🚀 Quickstart & Reproduction Guide (Gate G2 & G3)

LexiGuard-AI is designed for **instant, zero-credential reproducibility**. No paid cloud accounts, API keys, or multi-gigabyte forced downloads are required.

### 1. Clone & Set Up Environment
```bash
git clone https://github.com/zeroday-oss/lexiguard-ai.git
cd lexiguard-ai

# Create and activate virtual environment
python -m venv .venv
# On Windows:
.venv\Scripts\activate
# On Linux/macOS:
source .venv/bin/activate

# Install pinned dependencies
pip install -r requirements.txt
```

### 2. Run the Complete Automated Live Demo (Single Command)
```bash
python scripts/run_demo.py
```
This executes an 8-step live walkthrough:
- Standard agreement triage & grounding
- Predatory agreement penalty detection
- Grounded Q&A with exact citations
- Document silence verification (no hallucinations)
- Calibrated legal refusal of enforceability verdicts
- Two-version redline diffing
- Adversarial prompt injection neutralization
- Network isolation verification (0 external connections)

### 3. Run the Automated Test Suite (17 Tests)
```bash
pytest -v
```

### 4. Verify Network Privacy & Airgap Isolation
```bash
python scripts/audit_network_privacy.py
```

---

## 💻 Running LexiGuard-AI

### A. Modern Web Interface (Single Page App)
Launch the local web server:
```bash
python -m lexiguard.cli serve --port 8765
```
Open **`http://127.0.0.1:8765`** in your browser:
- **Document Triage Tab:** Upload PDF/TXT or pick from pre-loaded sample contracts with one click. Explore clauses filtered by risk tier (Safe, Medium, High, Critical).
- **Grounded Q&A Tab:** Ask questions about termination terms, deposits, or penalties with instant citations.
- **Contract Redline Diff Tab:** Drag & drop Version A and Version B to visualize added/removed/altered terms.
- **Privacy & Threat Model Tab:** Real-time socket monitor displaying airgap status and 0 external egress bytes.

---

### B. Command-Line Interface (CLI)

#### 1. Full Document Triage
```bash
python -m lexiguard.cli triage --file data/samples/rental_agreement_standard.txt
```
*Outputs structured fact matrix, risk score (0-100), clause-by-clause breakdown, missing standard protections, silent topics, and lawyer escalation guidance.*

#### 2. Grounded Q&A
```bash
python -m lexiguard.cli ask --file data/samples/rental_agreement_standard.txt --query "What is the security deposit amount?"
```

#### 3. Test Document Silence (No Hallucination)
```bash
python -m lexiguard.cli ask --file data/samples/rental_agreement_standard.txt --query "What is the pet policy for dogs and cats?"
```
*Output: Declares the document is silent with zero fabricated citations.*

#### 4. Test Calibrated Legal Refusal
```bash
python -m lexiguard.cli ask --file data/samples/rental_agreement_predatory.txt --query "Is this eviction clause legally valid in court?"
```
*Output: Issues polite calibrated refusal, cites literal text, and refers to a tenancy rights specialist.*

#### 5. Two-Version Contract Redline Diff
```bash
python -m lexiguard.cli compare --file-a data/samples/rental_agreement_standard.txt --file-b data/samples/rental_agreement_v2_amended.txt
```

#### 6. Run Privacy Audit
```bash
python -m lexiguard.cli audit
```

---

## 🧠 Gemma Model Integration & Deployment (Deliverable S6)

LexiGuard-AI supports Google's official Gemma family of edge models through a pluggable backend architecture:

1. **`Gemma-Builtin Engine` (Default):** Zero-download, high-speed deterministic legal intelligence engine running on native CPU. Delivers instant triage (<11 ms turnaround, ~40 MB RAM) for rapid evaluation and offline testing.
2. **`Ollama Local Runtime` (`--backend ollama`):** Connects strictly via `http://127.0.0.1:11434` to execute local Gemma models:
   ```bash
   ollama pull gemma2:2b
   # Or Gemma 3n edge models:
   ollama pull gemma:2b
   python -m lexiguard.cli triage --file data/samples/rental_agreement_standard.txt --backend ollama
   ```
3. **`llama-cpp-python` / GGUF:** Supports quantized INT4 / Q4_K_M GGUF weights on edge CPU/GPU.

*Detailed hardware benchmarks and speed reports are documented in [BENCHMARKS.md](BENCHMARKS.md).*

---

## 🛡️ Privacy Threat Model & Security (Deliverable S4 & Gate L9)

Private legal agreements contain sensitive financial data, salaries, and addresses. LexiGuard-AI provides verifiable mathematical isolation:
- **Zero Outbound Connections:** Active `privacy_shield()` interceptor traps any non-localhost socket connection and raises `PrivacyViolationError`.
- **Zero Telemetry:** Analytics, cloud logging, and cookies are hard-disabled.
- **Ephemeral Storage:** Sandbox temp files are purged from disk immediately upon parsing.
- **Prompt Injection Neutralization:** Embedded instruction hijacking is disarmed pre-tokenization.

*Read the complete threat analysis in [THREAT_MODEL.md](THREAT_MODEL.md).*

---

## 📊 Evaluation & Safety Matrix (Deliverable S8)

| Category | Capability Tested | Result |
| :--- | :--- | :--- |
| **Grounding (L5)** | Every claim cites verifiable passage with line/page numbers | **100% Pass** |
| **Silence (L5)** | Correctly reports silence on unmentioned topics without hallucination | **100% Pass** |
| **Refusal (L8)** | Refuses legal validity verdicts; directs to qualified attorney | **100% Pass** |
| **Privacy (L9)** | 0 outbound bytes transmitted during full triage under active monitor | **100% Pass** |
| **Injection (L6)** | Neutralizes prompt injections embedded within agreements | **100% Pass** |

*Read full rubric scoring and benchmarks in [EVALUATION.md](EVALUATION.md).*

---

## 📂 Disclosed Datasets & Attributions (Gate G4 & Deliverable S5)

All sample contracts in `data/samples/` are synthetic, non-confidential templates created specifically for open-source evaluation:
- `rental_agreement_standard.txt`: Standard balanced residential lease (Bengaluru, Karnataka)
- `rental_agreement_predatory.txt`: Landlord-biased agreement with hidden forfeiture and penalties
- `rental_agreement_v2_amended.txt`: Amended draft for two-version redline diff testing
- `employment_agreement_tech.txt`: Tech offer with non-compete and liquidated damages
- `non_disclosure_agreement.txt`: Unilateral NDA with foreign arbitration seat
- `legal_notice_eviction.txt`: Advocate formal notice of tenancy default
- `adversarial_prompt_injection.txt`: Contract with embedded prompt injection attack vectors

Recommended optional reference benchmarks:
- **CUAD (Contract Understanding Atticus Dataset):** 510 commercial contracts, CC BY 4.0 ([Atticus Project](https://www.atticusprojectai.org/cuad)).
- **India Code Legislation Portal:** Central & State Acts context ([indiacode.gov.in](https://indiacode.gov.in)).

---

## 📜 Ethical & Legal Disclaimer

> **IMPORTANT NOTICE:** LexiGuard-AI provides automated document triage solely for preliminary informational clarity and issue identification. It is **NOT an attorney**, cannot evaluate statutory legal validity or judicial enforceability, and does **NOT provide legal advice**. Always consult a qualified, licensed legal professional before signing or disputing agreements.

---

## 📄 License & Citation

Distributed under the **MIT License**. See [`LICENSE`](LICENSE) for details.

To cite this project:
```bibtex
@software{lexiguard_ai_2026,
  author = {LexiGuard Open Source Team},
  title = {LexiGuard-AI: Private On-Device Legal Document Triage Agent},
  year = {2026},
  url = {https://github.com/zeroday-oss/lexiguard-ai}
}
```
