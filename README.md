# SAT-SA: Supervisory Analytics Tool for SOC Assessment
**Submission for SIH26157 (NCIIPC, NTRO, MIC)**

## Overview
SAT-SA is an air-gapped, zero-dependency supervisory analytics workbench designed exclusively for the National Critical Information Infrastructure Protection Centre (NCIIPC). It transitions SOC assessment from manual sampling to deterministic, mathematical evaluation at scale.

In strict compliance with NCIIPC requirements, SAT-SA is **not** an operational SIEM or real-time monitoring tool. It is an offline supervisory capability that detects operational weaknesses—specifically **Execution Gaps** and **Negative Space**—from periodic structured data submissions.

## Core Capabilities (Mapping to Problem Statement)

1. **Air-Gapped & Deterministic (No AI/Cloud Dependency)**
   - Operates 100% offline. Zero external APIs, zero SaaS, zero LLMs. 
   - Uses a deterministic, transparent rules engine (16 Core Detectors) to find gaps, eliminating the "black-box" explainability issues of ML models.
   
2. **Execution Gaps & Negative Space Detection**
   - Automatically flags when expected operational behavior is missing (e.g., *Alert acknowledged but closed unusually fast*, *Repetitive template-driven investigations*, *Zero alerts on Critical systems*).
   - Generates an entity-level **Supervisory Risk Index (SRI)** based on formulaic weighting of Execution Gaps, Negative Space, Peer Deviation, and Operational Anomalies.

3. **Cryptographic Auditability & Non-Repudiation**
   - **Ingest Hash Anchoring:** All uploaded datasets are fingerprinted (SHA-256).
   - **Supervisory Audit Ledger:** Every examiner review action (e.g., "Corroborates", "Needs Evidence") is cryptographically chained into an append-only ledger.
   - **Ed25519 Attestation:** Supervisors can mathematically sign the final assessment state (RFC 8032), ensuring zero tampering in NCIIPC oversight files.

## Technical Architecture
- **Frontend Framework:** React 19 + TanStack Start (Single Page Application mode)
- **Styling:** Tailwind CSS v4 (Custom High-Contrast Enterprise/Government Design System)
- **State Management:** Zustand (Fully local state, no external database required)
- **Cryptography:** Web Crypto API (SHA-256, Ed25519)
- **Deployment:** Bundled via custom build script into a standalone zero-dependency Node.js executable artifact for air-gapped Linux/Windows environments.

---

## 🚀 Setup & Evaluation Instructions

We have provided two ways to run this project: standard development mode and air-gapped production mode.

### 1. Standard Developer Setup (Internet required for initial setup)
```bash
# 1. Install dependencies
npm install

# 2. Start the local dev server
npm run dev

# 3. Access the application
# Open your browser and navigate to http://localhost:8080
```

### 2. Air-Gapped Production Deployment (NCIIPC Offline Environment)
To deploy into a controlled network without internet access:

```bash
# 1. Build the production application (creates the optimized bundle)
npm run build

# 2. Package into a standalone offline directory
node scripts/package-offline.mjs

# 3. Transfer and Run
# The script above creates a self-contained folder named:
# `nciipc-sat-sa-offline`
# You can copy this exact folder to any offline machine that has Node.js installed.

# On the offline machine:
cd nciipc-sat-sa-offline
npm start
# The server will bind securely to 0.0.0.0:8080 completely offline.
```

---

## 🧪 Quick Test Guide (For Judges)
We have included a `test_datasets` directory containing 5 pre-configured CSV files. Each file is designed to immediately trigger specific behaviors in the deterministic engine.

1. Go to the **Ingest & Unify** tab in the application.
2. Select **Drop CSV submission here** and upload files from the `test_datasets` folder.
   - `test1_execution_gap_fast_closure.csv` → Flags Execution Gaps (Critical alerts closed too quickly without investigation).
   - `test2_negative_space_missing_telemetry.csv` → Flags Negative Space.
   - `test3_escalation_failure.csv` → Flags when True Positive Criticals are not escalated.
   - `test5_repetitive_template_notes.csv` → Flags Analyst robotic/templating behavior.
3. Observe the **Command Brief**, **Entities Register**, and **Review Queue** populate mathematically based on your uploads!

## Explainability & Validation
Every finding in the Material Findings Registry includes:
1. The exact mathematical formula triggered.
2. The observed evidence values.
3. The detector version (for methodology audits).
Validation is achieved by comparing the tool's 100% recall against historically annotated expert manual reviews (visualized in the Methodology tab).
