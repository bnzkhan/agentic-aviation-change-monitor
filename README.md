# Agentic Aviation Change Monitor

## Overview

**Agentic Aviation Change Monitor** is an internal-demo–ready Agentic AI system that detects, analyzes, and governs changes in global airport infrastructure using a small team of collaborating agents.

The system ingests (synthetic) airport data snapshots, identifies meaningful operational changes (e.g., runways, taxiways, gates, construction), applies configurable approval policies, and routes higher-risk items for human review via Jupyter Notebook.

This project is designed to demonstrate **agent orchestration, human-in-the-loop governance, and policy-driven autonomy**—with a clean path to future productionization.

---

## Why Agentic AI?

Airport infrastructure data is:

* Heterogeneous and noisy
* Operationally sensitive
* Subject to governance and audit requirements

A single monolithic model is not appropriate. Instead, this project uses **specialized agents** with clear responsibilities and shared state, coordinated using **LangGraph**.

---

## Agent Architecture

### 1. AirportIngestionAgent

**Responsibility:** Build versioned airport state

* Loads raw (synthetic) airport snapshots
* Normalizes data into a canonical schema
* Persists versioned state for comparison

---

### 2. ChangeAnalysisAgent

**Responsibility:** Detect and explain changes

* Diffs airport state versions
* Identifies semantic changes (runways, gates, construction)
* Produces:

  * Human-readable summaries
  * Impact classification
  * Confidence score

---

### 3. PolicyReviewAgent

**Responsibility:** Governance and approval

* Applies configurable approval rules (YAML)
* Auto-approves low-risk changes
* Routes higher-risk items for human review
* Records audit trail

---

## Workflow

1. Load airport snapshot at T₀
2. Load airport snapshot at T₁
3. Ingestion agent normalizes and versions data
4. Change analysis agent detects and explains differences
5. Policy agent applies approval rules
6. Human reviewer approves or rejects (Jupyter)

---

## Project Structure

```text
agentic-aviation-change-monitor/
│
├── agents/
│   ├── ingestion_agent.py
│   ├── change_analysis_agent.py
│   └── policy_review_agent.py
│
├── workflow/
│   └── agentic_pipeline.py
│
├── schemas/
│   ├── airport_state.py
│   └── change_event.py
│
├── policies/
│   └── approval_rules.yaml
│
├── data/
│   ├── synthetic_airports_v1.json
│   └── synthetic_airports_v2.json
│
├── notebooks/
│   └── demo_review.ipynb
│
├── architecture.md
├── README.md
└── requirements.txt
```

---

## Policies (Configurable Autonomy)

Approval behavior is defined in YAML.

Example:

```yaml
auto_approve:
  - change_type: "Gate Reassignment"
    max_impact: "LOW"

require_review:
  - change_type: "Runway Modification"
  - confidence_below: 0.85
```

This enables safe autonomy with explicit governance.

---

## LLM Usage

The system supports:

* Mock LLM (default for demos)
* OpenAI API (pluggable)

LLMs are used only where reasoning or summarization is required—not for deterministic logic.

---

## Demo Instructions

1. Install dependencies
2. Open `notebooks/demo_review.ipynb`
3. Run the agentic pipeline
4. Review detected changes
5. Approve or reject via notebook cells

---

## Future Extensions

* Real FAA / ICAO data sources
* FastAPI review UI
* Vector memory for historical precedent
* Auto-notifications to downstream systems

---

## Disclaimer

All data used in this project is synthetic and for demonstration purposes only.
