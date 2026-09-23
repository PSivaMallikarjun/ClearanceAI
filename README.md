# ClearanceAI 🤖🔍
# ClearanceAI 🤖🔍

### Autonomous Multi-Agent QA Engineering & Release-Clearance Platform

> **GitHub event → AI QA planning → synthetic test data → test execution & triage → confidence scoring → human approval → automated QA clearance**

**ClearanceAI** is an AI-powered, multi-agent QA engineering platform built with **CrewAI** and integrated with **GitHub Actions**.

It automates the repetitive and document-heavy parts of enterprise QA while preserving a controlled **Human-in-the-Loop (HITL)** approval gate before final clearance.

The platform analyzes software changes, generates a risk-aware test plan, creates compliant synthetic test data, executes and triages tests, evaluates every stage using **Laya**, and produces an auditable QA report that can be posted back to GitHub.

---

## 🚀 Why ClearanceAI?

Traditional enterprise QA workflows often require engineers to manually:

* Understand change requests and ticket scope
* Identify impacted functionality
* Build test plans
* Prepare test data
* Execute regression scenarios
* Analyze failures
* Document evidence
* Prepare QA sign-off reports
* Update development teams through GitHub/Jira-style workflows

ClearanceAI turns this into an **agentic QA pipeline**.

### The core idea

```text
                 ┌─────────────────────┐
                 │     GitHub Event    │
                 │  Push / Pull Request│
                 └──────────┬──────────┘
                            ↓
                 ┌─────────────────────┐
                 │ GitHub Event Intake │
                 │       Agent         │
                 └──────────┬──────────┘
                            ↓
              ┌────────────────────────────┐
              │     STAGE 1 — PLANNING     │
              │       QA Strategist        │
              └─────────────┬──────────────┘
                            ↓
                    ┌──────────────┐
                    │ Laya Scoring │
                    └──────┬───────┘
                           ↓
                  Auto-Approve / Revise
                    / Human Review
                           ↓
              ┌────────────────────────────┐
              │  STAGE 2 — TEST DATA       │
              │    Test Data Engineer       │
              └─────────────┬──────────────┘
                            ↓
                    ┌──────────────┐
                    │ Laya Scoring │
                    └──────┬───────┘
                           ↓
                  Auto-Approve / Revise
                    / Human Review
                           ↓
              ┌────────────────────────────┐
              │ STAGE 3 — EXECUTION        │
              │ Automation & Triage Agent   │
              └─────────────┬──────────────┘
                            ↓
                    ┌──────────────┐
                    │ Laya Scoring │
                    └──────┬───────┘
                           ↓
                  Auto-Approve / Revise
                    / Human Review
                           ↓
                 ┌────────────────────┐
                 │  FINAL HITL GATE   │
                 │ Senior QA Approval │
                 └─────────┬──────────┘
                           ↓
                 ┌────────────────────┐
                 │ QA Report Compiler │
                 └─────────┬──────────┘
                           ↓
             ┌────────────────────────────┐
             │ GitHub Status + Issue      │
             │ Audit Report + Evidence    │
             └────────────────────────────┘
```

---

# 🧠 Multi-Agent Architecture

ClearanceAI uses specialized agents rather than relying on one general-purpose AI agent.

| Agent                         | Responsibility                                        | Primary Output                                |
| ----------------------------- | ----------------------------------------------------- | --------------------------------------------- |
| **GitHub Event Intake Agent** | Reads repository, commit/PR context and changed files | Structured change context                     |
| **QA Strategist**             | Analyzes ticket and code-change scope                 | Prioritized test plan + coverage matrix       |
| **Laya — Plan Scorer**        | Evaluates test-plan completeness and confidence       | Score + rationale                             |
| **Test Data Engineer**        | Creates synthetic QA datasets                         | Schema + sample records + compliance metadata |
| **Laya — Data Scorer**        | Evaluates data quality and suitability                | Score + rationale                             |
| **Automation & Triage Agent** | Executes tests and analyzes failures                  | Test results + root-cause analysis            |
| **Laya — Results Scorer**     | Evaluates execution evidence                          | Score + rationale                             |
| **Senior QA Lead**            | Handles repeated low-confidence outcomes              | Resolution / escalation memo                  |
| **QA Report Compiler**        | Produces final evidence package                       | QA report + GitHub status + issue             |

---

# 🎯 Laya Confidence Engine

Every major stage passes through **Laya**, ClearanceAI's confidence-scoring agent.

Laya evaluates the quality of the current artifact before the workflow decides whether to continue.

### Decision model

|      Confidence | Decision                             |
| --------------: | ------------------------------------ |
|      **> 0.85** | ✅ Auto-approved                      |
| **0.50 – 0.85** | 🙋 Human review / HITL               |
|      **< 0.50** | ❌ Rejected and returned for revision |

### Example

```text
QA Strategist
     ↓
Test Plan
     ↓
Laya
     ↓
Confidence = 0.91
     ↓
AUTO-APPROVED
     ↓
Synthetic Data Generation
```

If the score falls below the acceptance threshold:

```text
Agent Output
     ↓
Laya
     ↓
Confidence = 0.42
     ↓
REJECT
     ↓
Feedback
     ↓
Responsible Agent
     ↓
Regenerate
```

---

# 🔁 Controlled Retry & Escalation

ClearanceAI prevents agents from entering uncontrolled loops.

If an agent receives **three consecutive rejections**, the workflow escalates to the **Senior QA Lead**.

```text
Attempt 1 → Reject
     ↓
Attempt 2 → Reject
     ↓
Attempt 3 → Reject
     ↓
Senior QA Lead
     ↓
Authoritative Resolution
```

This creates a controlled balance between:

* Autonomous execution
* AI self-correction
* Human oversight
* Auditability

---

# 👤 Human-in-the-Loop

ClearanceAI is **not designed to remove human QA accountability**.

Instead, it automates repetitive work while retaining a deliberate human approval point.

### Human intervention can occur when:

* Laya confidence is ambiguous
* Compliance requirements require review
* Critical defects are detected
* Risk exceeds configured thresholds
* Repeated agent revisions fail

### Final clearance

```text
AI-generated evidence
        ↓
Confidence evaluation
        ↓
Human QA approval
        ↓
Final QA clearance
```

---

# ⚡ Trigger Sources

ClearanceAI supports three execution modes.

### 1. GitHub Push

A push to configured branches can initiate a QA run.

### 2. GitHub Pull Request

A PR can trigger QA analysis when opened or updated.

GitHub Actions supports `push`, `pull_request`, and manual `workflow_dispatch` triggers for workflow automation.

### 3. Manual Execution

A QA engineer can launch the workflow with custom parameters.

---

# 📥 Manual Run Inputs

| Input                     | Description             | Example                            |
| ------------------------- | ----------------------- | ---------------------------------- |
| `platform`                | System under test       | `Salesforce CRM`                   |
| `input_type`              | Type of change          | `Feature Release`                  |
| `region`                  | Geographic scope        | `EMEA`                             |
| `compliance_requirements` | Applicable requirements | `GDPR, SOC2`                       |
| `ticket_detail`           | Ticket ID + description | `JIRA-4821: New lead routing rule` |

### Example

```bash
crewai run \
  --input platform="Salesforce CRM" \
  --input input_type="Feature Release" \
  --input region="EMEA" \
  --input compliance_requirements="GDPR, SOC2" \
  --input ticket_detail="JIRA-4821: New lead routing rule for UK and Germany"
```

---

# 🧪 Stage 1 — AI Test Planning

The **QA Strategist** receives:

* Ticket information
* PR description
* Changed files
* Repository context
* Business scope
* Compliance requirements

It produces a structured test plan.

### Example output

```markdown
## QA Test Plan

### Functional Coverage
- Lead creation
- Lead routing
- Regional assignment
- Duplicate detection

### Negative Coverage
- Invalid country
- Missing routing rule
- Duplicate lead
- Unauthorized user

### Regression Coverage
- Existing lead workflows
- Existing assignment rules
- CRM integration

### Risk
HIGH

### Coverage Matrix
| Requirement | Test Cases | Priority |
|---|---|---|
| UK routing | TC-001–TC-004 | P0 |
| Germany routing | TC-005–TC-008 | P0 |
| Invalid region | TC-009 | P1 |
```

Laya then evaluates the generated plan.

---

# 🧬 Stage 2 — Synthetic Test Data

The **Test Data Engineer** creates synthetic datasets based on the approved test plan.

Example:

```json
{
  "customer": {
    "customer_id": "SYN-10001",
    "country": "DE",
    "email": "synthetic.user@example.test",
    "segment": "Enterprise"
  },
  "compliance": {
    "contains_real_pii": false,
    "synthetic": true,
    "classification": "NON_PRODUCTION"
  }
}
```

The system is designed so that test data generation does not require real customer PII.

> Compliance requirements must still be validated against the organization's actual legal, regulatory, and security controls. ClearanceAI's generated annotations are not themselves legal certification.

---

# 🔬 Stage 3 — Test Execution & Triage

The **Automation & Triage Agent**:

1. Executes configured test cases
2. Collects test output
3. Parses logs
4. Identifies failures
5. Groups related failures
6. Determines probable root causes
7. Assesses SLA impact
8. Generates an audit trail

Example:

```text
Test Suite
    │
    ├── 42 Passed
    ├── 3 Failed
    └── 2 Blocked

Failure Cluster
    ↓
Authentication Service
    ↓
HTTP 401
    ↓
Probable Cause:
Expired test credential
    ↓
Severity: High
```

Laya then evaluates the quality and completeness of the execution report.

---

# 📊 Final QA Report

ClearanceAI compiles all artifacts into a single report.

### Example structure

```markdown
# ClearanceAI QA Report

## Build
Repository: example/customer-platform
Commit: a82f9d1

## Change Scope
Feature Release

## Test Coverage
Functional: 94%
Regression: 87%
Negative: 91%

## Execution
Passed: 42
Failed: 3
Blocked: 2

## Critical Findings
- Authentication regression
- Missing regional routing case

## Laya Scores
Planning: 0.93
Test Data: 0.89
Execution: 0.91

## Human Approval
Status: APPROVED
Reviewer: Senior QA Lead
Timestamp: 2026-09-23T10:30:00Z

## Final Clearance
APPROVED
```

---

# 🔗 GitHub Integration

ClearanceAI can integrate with GitHub Actions to initiate the workflow from repository events.

A production implementation should use the minimum required GitHub token permissions and repository/environment secrets rather than hardcoding credentials. GitHub documents `GITHUB_TOKEN` authentication and recommends limiting its permissions to the minimum required scope.

### Example workflow

```yaml
name: ClearanceAI QA

on:
  push:
    branches:
      - main
      - develop

  pull_request:
    branches:
      - main

  workflow_dispatch:

permissions:
  contents: read
  checks: write
  issues: write

jobs:
  clearanceai:
    name: Run ClearanceAI
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run ClearanceAI
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          crewai run
```

GitHub repository secrets are encrypted and can be exposed to a workflow only when explicitly referenced.

---

# 🔐 Security Architecture

| Security Control      | Implementation                                 |
| --------------------- | ---------------------------------------------- |
| OpenAI credentials    | GitHub/CrewAI secrets                          |
| GitHub authentication | `GITHUB_TOKEN` / configured GitHub credentials |
| Synthetic test data   | No intentional production PII                  |
| Audit trail           | Agent decisions + timestamps                   |
| Human approval        | Final clearance gate                           |
| Retry control         | Maximum 3 consecutive rejections               |
| Access control        | Minimum required permissions                   |
| Production deployment | Explicitly outside current scope               |

> **Security note:** ClearanceAI should be deployed with least-privilege credentials and isolated execution environments. Never place API keys directly in source code.

---

# 🏗️ Project Structure

```text
ClearanceAI/
│
├── src/
│   └── clearanceai/
│       │
│       ├── flow.yaml
│       │
│       ├── crews/
│       │   ├── qa_strategist/
│       │   ├── test_data_engineer/
│       │   ├── automation_triage/
│       │   └── report_compiler/
│       │
│       ├── tools/
│       │   ├── laya_scorer.py
│       │   └── github_utils.py
│       │
│       └── knowledge/
│           ├── compliance_frameworks.md
│           └── sla_thresholds.md
│
├── .github/
│   └── workflows/
│       └── clearanceai.yml
│
├── tests/
│
├── .env.example
├── .gitignore
├── LICENSE
├── README.md
└── requirements.txt
```

---

# 🛠️ Technology Stack

| Technology                    | Purpose                              |
| ----------------------------- | ------------------------------------ |
| **Python**                    | Core application and agent tooling   |
| **CrewAI**                    | Multi-agent orchestration            |
| **OpenAI GPT**                | Agent reasoning and generation       |
| **GitHub API**                | Repository and QA result integration |
| **GitHub Actions**            | CI/CD event trigger                  |
| **JSON / Markdown**           | Structured QA artifacts              |
| **Synthetic Data Generation** | Non-production test datasets         |

---

# ⚙️ Installation

## Prerequisites

* Python 3.10+
* CrewAI CLI
* Git
* GitHub repository
* OpenAI API key
* Configured GitHub credentials

Install CrewAI:

```bash
pip install crewai
```

Install project dependencies:

```bash
crewai install
```

---

# ▶️ Run Locally

```bash
crewai run
```

Or run with custom inputs:

```bash
crewai run \
  --input platform="Salesforce CRM" \
  --input input_type="Feature Release" \
  --input region="EMEA" \
  --input compliance_requirements="GDPR, SOC2" \
  --input ticket_detail="JIRA-4821: New lead routing rule for UK and Germany"
```

---

# 📈 Current Capabilities

### ✅ Implemented / Designed

* Multi-agent QA orchestration
* GitHub event-driven execution
* AI-generated test plans
* Confidence-based routing
* Synthetic test-data generation
* Automated test triage
* Retry and escalation logic
* Human-in-the-loop approval
* Audit report generation
* GitHub status integration
* GitHub issue generation
* Compliance metadata
* Manual execution mode

---

# 🚧 Roadmap

ClearanceAI is intentionally designed as an extensible QA automation platform.

### Phase 1 — Foundation

* [x] CrewAI orchestration
* [x] Agent specialization
* [x] Laya confidence scoring
* [x] GitHub trigger design
* [x] HITL approval model

### Phase 2 — Enterprise Integrations

* [ ] Jira integration
* [ ] Salesforce integration
* [ ] Workday integration
* [ ] ServiceNow integration
* [ ] Slack / Teams notifications

### Phase 3 — Test Automation

* [ ] Playwright integration
* [ ] Selenium integration
* [ ] API testing
* [ ] Postman/Newman integration
* [ ] Pytest integration
* [ ] JMeter integration

### Phase 4 — Observability

* [ ] Application Insights
* [ ] CloudWatch
* [ ] Datadog
* [ ] OpenTelemetry
* [ ] Production-safe log ingestion

### Phase 5 — Enterprise Intelligence

* [ ] Historical defect learning
* [ ] Risk-based regression selection
* [ ] Flaky-test detection
* [ ] Test-maintenance agents
* [ ] Release-risk prediction
* [ ] QA knowledge graph

---

# 🚫 Current Limitations

ClearanceAI currently does **not**:

| Limitation                              | Reason                                            |
| --------------------------------------- | ------------------------------------------------- |
| Read live production logs               | Observability integration required                |
| Execute real NeoLoad workloads          | NeoLoad API integration required                  |
| Autonomously deploy production releases | Infrastructure access intentionally out of scope  |
| Autonomously roll back production       | Requires deployment/platform controls             |
| Replace QA leadership                   | Human accountability remains part of the workflow |

These limitations are intentional. ClearanceAI focuses on **AI-assisted QA engineering and controlled release clearance**, not unrestricted autonomous production operations.

---

# 🧩 Design Principles

ClearanceAI follows five principles:

### 1. Autonomous by Default

Agents perform repetitive QA activities without requiring manual intervention at every step.

### 2. Confidence Before Progression

Every major artifact is evaluated before the workflow advances.

### 3. Human Accountability

Final clearance remains subject to human approval.

### 4. Auditable Decisions

Agent outputs, confidence scores, revisions, approvals and timestamps are captured.

### 5. Least-Privilege Automation

Automation should only receive the permissions and credentials required for its specific task.

---

# 📊 Example End-to-End Run

```text
GitHub PR #4821
      │
      ▼
Event Intake
      │
      ▼
Changed Files + PR Context
      │
      ▼
QA Strategist
      │
      ▼
Test Plan
      │
      ▼
Laya = 0.93
      │
      ├── > 0.85 → AUTO APPROVE
      │
      ▼
Synthetic Data Engineer
      │
      ▼
Dataset
      │
      ▼
Laya = 0.89
      │
      ▼
Automation & Triage
      │
      ▼
42 PASS / 3 FAIL / 2 BLOCKED
      │
      ▼
Laya = 0.91
      │
      ▼
FINAL HUMAN SIGN-OFF
      │
      ▼
QA Report
      │
      ├── GitHub Commit Status
      └── GitHub Issue
```

---

# 🎯 Enterprise Use Cases

ClearanceAI can be adapted for:

* Salesforce QA
* CRM releases
* Workday integrations
* Financial applications
* Enterprise APIs
* SaaS platforms
* Regression testing
* Compliance-sensitive applications
* Release validation
* Synthetic test-data generation
* AI-assisted defect triage

---

# 🌟 What Makes ClearanceAI Different?

ClearanceAI is not simply an LLM generating test cases.

It combines:

```text
Multi-Agent Orchestration
          +
Confidence-Based Routing
          +
Self-Correction
          +
Synthetic Test Data
          +
Automated Test Execution
          +
Failure Triage
          +
Human Approval
          +
Auditability
```

The result is a **controlled agentic QA pipeline** designed to operate alongside enterprise engineering teams.

---

# 👨‍💻 Author

**PSivaMallikarjun**

Senior QA Engineer | Salesforce CRM & Workday QA | AI-Integrated Testing

GitHub: `github.com/PSivaMallikarjun`

---

# 📄 License

This project is licensed under the **MIT License**.

See [`LICENSE`](LICENSE) for details.

---

# 🚀 Getting Started

Clone the repository:

```bash
git clone https://github.com/PSivaMallikarjun/ClearanceAI.git
cd ClearanceAI
```

Initialize the environment:

```bash
python -m venv .venv
```

Activate it:

### Windows

```bash
.venv\Scripts\activate
```

### macOS / Linux

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Configure your secrets using your local environment or GitHub repository secrets.

Then run:

```bash
crewai run
```

---

## 🔗 GitHub

**Repository:** `PSivaMallikarjun/ClearanceAI`

The GitHub integration is designed around standard GitHub Actions events and secure repository secrets rather than embedding credentials in the repository.

---

> **ClearanceAI — From Code Change to QA Clearance, with AI in the loop and humans in control.**



https://github.com/user-attachments/assets/fc07c44a-a847-4312-8826-de4316605be6



