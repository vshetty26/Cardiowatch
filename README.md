
# ❤️ CardioWatch — AF Detection Wearable System

> A safety-focused wearable ECG system designed to detect patterns suggestive of Atrial Fibrillation (AF), provide local alerts, and securely synchronize results.

---

## 📌 Overview

**CardioWatch** is a wrist-worn wearable software system that records a **single-lead ECG**, checks signal quality, analyzes the ECG for patterns suggestive of **Atrial Fibrillation (AF)**, and provides an alert and appropriate advice to the wearer.

The system is designed with a **safety-first architecture**, where ECG processing, signal-quality checking, AF detection, and alerting remain on the wearable.

> ⚠️ **Disclaimer:** CardioWatch is a project/case-study implementation and does **not provide a medical diagnosis or replace professional clinical judgment.**

---

## 🎯 Objectives

- Record a single-lead ECG.
- Evaluate ECG signal quality before analysis.
- Detect patterns suggestive of AF.
- Generate alerts directly on the wearable.
- Store results locally.
- Synchronize results with a mobile application.
- Provide history and advice.
- Support secure cloud synchronization and clinician review.
- Maintain complete requirements-to-verification traceability.
- Follow a structured software engineering lifecycle.

---

## 🏗️ System Architecture

```text
                    CARDIOWATCH SYSTEM

┌──────────────────────────┐
│      WEARABLE DEVICE     │
│        CardioWatch       │
│                          │
│  ECG Sensor              │
│       ↓                  │
│  Pre-processing          │
│       ↓                  │
│  Signal Quality Check    │
│       ↓                  │
│  AF Detection             │
│       ↓                  │
│  Local Alert             │
│       ↓                  │
│  Local Storage            │
└────────────┬─────────────┘
             │
          Bluetooth LE
             │
             ▼
┌──────────────────────────┐
│       MOBILE APP         │
│                          │
│  User Interface          │
│  Results                 │
│  History                 │
│  Consent                 │
│  Sync Manager            │
└────────────┬─────────────┘
             │
          HTTPS / TLS
             │
             ▼
┌──────────────────────────┐
│       CLOUD SERVER       │
│                          │
│  Data Storage            │
│  User Management         │
│  Audit Logs              │
│  Clinician Review        │
└──────────────────────────┘
````

### Safety-Critical Path

The safety-critical functions remain on the wearable:

```text
ECG Capture
     ↓
Pre-processing
     ↓
Signal Quality
     ↓
AF Detection
     ↓
Local Alert
     ↓
Local Storage
```

The phone and cloud provide additional non-critical functions such as history, synchronization, audit and review.

---

## 🔄 System Workflow

```text
Start ECG Recording
        ↓
Capture ECG
        ↓
Check Signal Quality
        ↓
   ┌───────────────┐
   │ Quality Good? │
   └───────┬───────┘
       No  │  Yes
       ↓   │   ↓
  Show Guidance
       ↓       AF Detection
     Retry          ↓
                AF Detected?
                 /        \
               Yes         No
                ↓           ↓
             Alert       Normal Result
                \           /
                 \         /
                  ↓       ↓
                  Store Result
                       ↓
              Sync When Connected
```

A poor-quality ECG is not allowed to generate a possible-AF alert.

---

# 📋 Requirements

The project contains **19 software requirements** across functional, accuracy, reliability, security and labelling categories.

### Functional Requirements

| ID     | Requirement                          |
| ------ | ------------------------------------ |
| FUN-01 | Record guided single-lead ECG        |
| FUN-02 | Check signal quality before analysis |
| FUN-03 | Display signal-quality status        |
| FUN-04 | Generate AF alert on the wearable    |
| FUN-05 | Cancel and discard partial recording |
| FUN-06 | Store and synchronize results        |

### Accuracy Requirements

| ID     | Requirement                                                |
| ------ | ---------------------------------------------------------- |
| ACC-01 | Sensitivity ≥ 96%                                          |
| ACC-02 | Specificity ≥ 98%                                          |
| ACC-03 | False positives ≤ 2,000 per 100,000 healthy wearers        |
| ACC-04 | Validation data must be separate from training/tuning data |

### Reliability Requirements

| ID     | Requirement                                        |
| ------ | -------------------------------------------------- |
| REL-01 | MTTF ≥ 25,000 hours                                |
| REL-02 | Device failure → FAULT state                       |
| REL-03 | Connectivity failure must not suppress local alert |

### Security

| ID     | Requirement                                             |
| ------ | ------------------------------------------------------- |
| SEC-01 | Only signed firmware from the released baseline can run |

### Labelling

The alert includes:

```text
POSSIBLE IRREGULAR RHYTHM

THIS IS NOT A DIAGNOSIS.
```

---

# 📐 UML Diagrams

The project uses six UML views:

### 1. Use Case Diagram

Shows interactions between:

* User
* Clinician
* CardioWatch System

### 2. Class Diagram

Main classes:

```text
ECGSignal
SignalQuality
AFDetector
DetectionResult
ECGRecord
User
DataSync
```

### 3. Sequence Diagram

```text
User
 ↓
Wearable
 ↓
Signal Quality
 ↓
AF Detector
 ↓
Result
 ↓
Alert / Normal
 ↓
Storage
```

### 4. Activity Diagram

Represents the complete ECG recording and AF detection workflow.

### 5. State Machine Diagram

```text
HOME
  ↓
RECORDING
  ↓
QUALITY CHECK
  ↓
ANALYZING
  ↓
ALERT / NORMAL
  ↓
SAVE
  ↓
SYNC
```

Includes a `FAULT` state for serious device failures.

### 6. Deployment Diagram

```text
Wearable
    │
    │ Bluetooth LE
    ↓
Mobile App
    │
    │ HTTPS/TLS
    ↓
Cloud Server
```

---

# 🔬 Software Engineering Methodology

## V-Model

CardioWatch follows the **V-Model** because development activities are paired with verification and validation activities.

```text
Requirements          → System Testing
      ↓
Architecture          → Integration Testing
      ↓
Detailed Design       → Unit Testing
      ↓
Implementation
```

Short internal iterations can be used within development phases, but patient-facing releases must be controlled, verified and baselined.

---

# 🧪 Verification & Validation

Every requirement has:

* Verification method
* Evidence type
* Acceptance criterion
* Hazard linkage

### Verification Methods

| Code | Method        |
| ---- | ------------- |
| T    | Test          |
| A    | Analysis      |
| I    | Inspection    |
| D    | Demonstration |

### Testing Levels

```text
Unit Verification
       ↓
Integration Testing
       ↓
HIL Testing
       ↓
System Testing
       ↓
ECG Replay Testing
       ↓
Fault Injection
       ↓
Clinical Validation
```

The verification matrix provides **19/19 requirement coverage**.

---

# ⚠️ Risk Management

Major hazards include:

| ID   | Hazard                             |
| ---- | ---------------------------------- |
| H-01 | Missed AF / False Negative         |
| H-02 | False Alarm / False Positive       |
| H-03 | False Reassurance                  |
| H-04 | Silent Device Failure              |
| H-05 | Connectivity-Related Alert Failure |
| H-06 | Misleading or Missing Labelling    |
| H-07 | Unverified or Tampered Firmware    |

---

# 🛡️ Safety Design Decisions

### D-01 — Detection on Wearable

Detection and alerting remain on-device for:

* Offline safety
* Low latency
* Privacy
* Connectivity independence

### D-02 — Signal Quality Gate

Poor-quality signals are prevented from reaching the alert decision.

### D-03 — Locked Thresholds

Detection thresholds are locked before validation.

### D-04 — Explicit FAULT State

Device failures result in:

```text
FAULT
  ↓
Detection Unavailable
```

The device must not display a normal result after a serious failure.

### D-05 — Signed Firmware

Only verified and released firmware is allowed to run.

---

# ⚙️ Configuration Management

Project artifacts are controlled through baselines:

```text
BL-0 → Plan
BL-1 → Requirements
BL-2 → Design
BL-3 → Code + Unit Verification
BL-4 → Release Candidate
BL-5 → Released / Submitted
```

Changes are handled through:

```text
Change Request
      ↓
Impact Analysis
      ↓
Risk Analysis
      ↓
Change Control Board
      ↓
Implementation
      ↓
Regression Testing
      ↓
Documentation
      ↓
New Baseline
```

---

# 🚪 Review Gates

| Gate | Purpose               |
| ---- | --------------------- |
| IUR  | Intended-use review   |
| SRR  | Requirements review   |
| DR   | Design review         |
| TRR  | Test readiness review |
| RR   | Release review        |

The release review verifies that requirements are verified, open defects are acceptable and labelling matches the verification evidence.

---

# 👥 Project Roles

| Role             | Responsibility                        |
| ---------------- | ------------------------------------- |
| Project Manager  | Planning, tracking and change control |
| Software Lead    | Architecture, design and code quality |
| V&V Lead         | Testing, verification and validation  |
| QA / Regulatory  | Quality, standards and submission     |
| Clinical Advisor | Intended use and clinical validation  |
| CM Manager       | Baselines, versions and traceability  |

---

# 📊 Project Estimation

### COCOMO

| Parameter           |               Value |
| ------------------- | ------------------: |
| Estimated Size      |             18 KLOC |
| Model               | Intermediate COCOMO |
| EAF                 |                1.40 |
| Basic Effort        | 115.5 Person-Months |
| Intermediate Effort | 125.8 Person-Months |
| Schedule            |        ~11.7 Months |

The additional effort accounts for reliability, verification, documentation, traceability and controlled processes.

---

# 📚 Standards

The project references:

* **IEC 62304** — Medical Device Software Lifecycle
* **ISO 14971** — Medical Device Risk Management
* **ISO 13485** — Medical Device Quality Management
* **IEC 62366-1** — Usability Engineering

---

# 📦 Project Deliverables

1. Software Development Plan
2. SRS with Verification Matrix
3. Design Pack
4. Verification & Validation Plan
5. Estimation Report
6. Configuration Management Plan

---

# 🔄 Complete Project Lifecycle

```text
Problem Identification
        ↓
Intended Use / BRD
        ↓
Risk & Hazard Analysis
        ↓
SRS
        ↓
Verification Matrix
        ↓
System Architecture
        ↓
UML / Detailed Design
        ↓
Implementation
        ↓
Unit Verification
        ↓
Integration / HIL Testing
        ↓
System / Replay Testing
        ↓
Clinical Validation
        ↓
Configuration Management
        ↓
Release Review
        ↓
Controlled Release
```

---

# ⚠️ Disclaimer

CardioWatch is an academic software engineering case study/project.

It is **not a medical device for real-world clinical diagnosis** and should not be used to make medical decisions.

---

## 👨‍💻 Author

**Vamshi Shetty**

**B.Tech CSE — Software Engineering & Project Management**

---

## ⭐ Key Takeaway

> **CardioWatch follows a safety-first V-Model approach where every requirement is linked to hazards, design decisions, verification evidence and controlled release baselines.**

```
```
