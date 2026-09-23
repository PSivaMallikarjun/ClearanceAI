# ClearanceAI 🤖🔍

> Fully autonomous, multi-agent QA engineering platform powered by CrewAI.  
> Triggered by GitHub events. Scored by Laya. Gated by one human sign-off.

---

## What ClearanceAI Does

ClearanceAI replaces the repetitive, document-heavy parts of enterprise QA with a 
fully autonomous multi-agent crew. It triggers on a GitHub push or pull request, 
runs three sequential QA stages — each scored by a confidence agent called Laya — 
and only pauses once for a final human sign-off before posting results back to GitHub.

### End-to-End Flow
GitHub Push / PR → Ingest GitHub Event ↓ Stage 1: Generate Test Plan → Laya Score → Auto-approve / Revise / Escalate ↓ Stage 2: Generate Synthetic Data → Laya Score → Auto-approve / Revise / Escalate ↓ Stage 3: Execute Tests & Triage → Laya Score → Auto-approve / Revise / Escalate ↓ ⏸️ One Final Human Sign-Off ↓ Compile QA Report → Post GitHub Commit Status → Create GitHub Issue

--- ## Agents | Agent | Role | Output | |---|---|---| | **GitHub Event Intake Agent** | Reads the triggering PR or push from GitHub | Repo, ref, changed files, QA focus areas | | **QA Strategist** | Analyzes ticket + GitHub change scope | Prioritized test plan with coverage matrix (Markdown) | | **Laya — Plan Scorer** | Evaluates test plan quality | Confidence score 0.0–1.0 + rationale | | **Test Data Engineer** | Generates GDPR/SOC2-compliant synthetic datasets | JSON schema with sample records + compliance annotations | | **Laya — Data Scorer** | Evaluates synthetic dataset quality | Confidence score 0.0–1.0 + rationale | | **Automation & Triage Agent** | Executes test cases, parses logs, triages failures | Pass/fail report with root cause, SLA impact, audit trail | | **Laya — Results Scorer** | Evaluates test execution report quality | Confidence score 0.0–1.0 + rationale | | **Senior QA Lead** | Intervenes after 3 consecutive rejections | Authoritative final version or escalation memo | | **QA Report Compiler** | Assembles final report, posts to GitHub | Full audit report + GitHub commit status + GitHub issue | --- ## Laya Confidence Scoring Every stage output is scored before any routing decision is made. | Laya Score | Decision | |---|---| | **> 0.85** | ✅ Auto-approved — flow continues immediately, no human needed | | **0.50 – 0.85** | 🙋 Ambiguous — HITL triggers, human reviews and approves or rejects | | **< 0.50** | ❌ Auto-rejected — loops back to responsible agent with comments | After **3 consecutive rejections** at any stage, the **Senior QA Lead** automatically intervenes and produces a definitive resolution. --- ## Triggers ClearanceAI runs automatically on: - ✅ **GitHub Push** — any push to the repository - ✅ **GitHub Pull Request** — when a PR is opened or updated - ✅ **Manual Run** — kick off directly from CrewAI Studio with custom inputs --- ## Kickoff Inputs When running manually, provide these five inputs: | Field | Description | Example | |---|---|---| | platform | System under test | Salesforce CRM | | input_type | Type of change | Feature Release | | region | Geographic scope | EMEA | | compliance_requirements | Applicable standards | GDPR, SOC2 | | ticket_detail | Ticket ID and description | JIRA-4821: New lead routing rule for UK and Germany | For GitHub-triggered runs, the following are auto-populated from the event: | Field | Auto-populated From | |---|---| | github_repo | Repository that fired the event | | github_ref | Branch or commit SHA | | github_event_context | PR title, commit message, or workflow name | --- ## Compliance ClearanceAI is built with enterprise compliance requirements in mind: - **GDPR** — all synthetic datasets are fully anonymized; no real PII is ever generated or stored - **SOC2** — compliance annotations are included on every data field in generated datasets - **CCPA** — pseudonymization strategy documented in every dataset output - **Audit Trail** — every agent decision, score, human approval, and timestamp is captured in the final report --- ## Project Structure
ClearanceAI/ ├── src/ │ └── clearanceai/ │ ├── flow.yaml # Main declarative flow definition │ ├── crews/ # Reusable crew definitions │ │ ├── qa_strategist/ │ │ ├── test_data_engineer/ │ │ ├── automation_triage/ │ │ └── report_compiler/ │ ├── tools/ # Custom Python tools │ │ ├── laya_scorer.py # Confidence scoring logic │ │ └── github_utils.py # GitHub event helpers │ └── knowledge/ # Shared knowledge files │ ├── compliance_frameworks.md │ └── sla_thresholds.md ├── .github/ │ └── workflows/ │ └── clearanceai.yml # GitHub Actions workflow (optional CI trigger) ├── .gitignore ├── LICENSE └── README.md

--- ## Install & Run ### Prerequisites - Python 3.10+ - CrewAI CLI installed (pip install crewai) - OpenAI API key stored in CrewAI Studio Secrets - GitHub connection configured in CrewAI Studio ### Install bash crewai install
Run Locally
crewai run
Run with Custom Inputs
crewai run --input platform="Salesforce CRM" \
           --input input_type="Feature Release" \
           --input region="EMEA" \
           --input compliance_requirements="GDPR, SOC2" \
           --input ticket_detail="JIRA-4821: New lead routing rule for UK and Germany"
GitHub Actions Integration (Optional)
To trigger ClearanceAI automatically on every push or PR, add this to .github/workflows/clearanceai.yml:

name: ClearanceAI QA Run

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  qa-run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Trigger ClearanceAI
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          crewai run
⚠️ Never hardcode your API key. Always use ${{ secrets.OPENAI_API_KEY }} in GitHub Actions.

Security
Practice	Status
OpenAI key stored as encrypted secret	✅ CrewAI Studio Secrets
No real PII in synthetic datasets	✅ Enforced by Test Data Engineer agent
GitHub key stored as repo secret	✅ Settings → Secrets → OPENAI_API_KEY
Audit trail on every run	✅ Captured in final report
Human sign-off before report closes	✅ Final gate enforced
What ClearanceAI Cannot Do (Yet)
Limitation	Why
Read live production logs	No runtime observability integration
Execute real NeoLoad performance tests	Requires direct NeoLoad API integration
Autonomous rollback or deployment	Out of scope — requires infrastructure access
Replace client-facing QA leadership	Human judgment and trust cannot be automated
Built With
CrewAI — multi-agent orchestration
OpenAI GPT — agent reasoning
GitHub API — event ingestion and status posting
Laya — custom confidence scoring agent (built in-flow)
Author
PSivaMallikarjun
Senior QA Engineer | Salesforce CRM & Workday QA | AI-Integrated Testing
github.com/PSivaMallikarjun

License
MIT License — see LICENSE for details.

--- ### 📌 How to Push This to Your Repo bash # In your terminal, inside your local ClearanceAI folder echo "# ClearanceAI" >> README.md # Paste the full README content above into README.md, then: git init git add README.md git commit -m "Add detailed README for ClearanceAI" git branch -M main git remote add origin https://github.com/PSivaMallikarjun/ClearanceAI.git git push -u origin main


https://github.com/user-attachments/assets/fc07c44a-a847-4312-8826-de4316605be6



