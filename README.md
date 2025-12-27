# Synthetic-Delphi 
Application to set up, save, load and autonomously run 12 types of Delphi Studies using cohorts of synthetic participant personas .

**Creator:** Shane McLoughlin  
**App name:** Synthetic Delphi

**Synthetic Delphi** is a local Streamlit application for running **LLM‑mediated Delphi studies** with a configurable panel of participant agents and a master facilitator (researcher) agent. It supports multiple Delphi templates (pipelines), configurable response‑length and structure policies, audit trails, run health diagnostics, and professionally formatted downloadable reports.


## What it does

Synthetic Delphi helps you design and execute Delphi-style studies where:

- **Participants (LLM agents)** generate responses to prompts/questions under a defined role/persona.
- A **Master facilitator (LLM agent)** consolidates responses, provides controlled feedback, and manages iterations.
- The system computes **quorum**, **consensus/stability** signals (template-dependent), and produces a **synthesis**.
- You can export:
  - a **run audit trail** (stored locally),
  - a **consultancy-style report** (Word `.docx`, downloadable),
  - and optional cohort configuration files for reuse.

---

## Key features

### Study Set‑Up workspace
- **Participants panel** (up to **30** participants)
  - per‑participant role/persona, model selection, and optional per‑participant API key
  - **global API key** option (recommended when using one provider/model)
- **Master facilitator configuration**
  - model, reasoning mode (where supported), response length policy
- **Pipeline template selection** with template‑specific instruments and outputs
- **Project name** and **Report title**
  - used in cohort saves and report generation

### Response length and structure policies
- Separate controls for:
  - **Token limits** (API maximum output tokens)
  - **Word-count policy** (expected response length)
  - **Per-stage overrides** (different word targets per round/stage)
- Master default policy is tuned for larger panels (default target **~2500 words**) to reduce truncation pressure in multi-participant consolidation.

### Model compatibility safeguards
- The app dynamically adjusts requests to match model constraints (e.g., models that only support default temperature).
- “Thinking / reasoning” controls are handled separately from response structuring:
  - reasoning effort influences *internal deliberation* (where supported)
  - word policy controls *presentation length and structure*

### Execution transparency and audit trail
- **Run & Results** provides:
  - agent‑level health (who responded / failed / last error)
  - stage‑level health (quorum achieved, response counts, errors)
  - detailed audit trail of prompts, responses, and artifacts (local)
- A structured **execution summary** is produced for each run.

### Reports
- Generates a **professionally structured Word report** (`.docx`) including:
  - abstract
  - method summary (template, rounds, panel settings)
  - participants (roles and models)
  - findings / synthesis
  - agreements/disagreements (where applicable)
  - dated cover information
- Optional “polish” step can be enabled to perform **spell/grammar checking and layout improvements only** (no substantive rewriting). A guard rejects outputs that deviate materially from the master synthesis; the original text is preserved.

---

## Supported templates (pipelines)

Templates appear under **Study Set‑Up → Study Design → Pipeline template**.

### 1) Item Rating Delphi (`item_rating`)
Consensus-building around items/dimensions.
- optional elicitation or seeded items
- master consolidation
- structured rating rounds with controlled feedback
- consensus and stability diagnostics

### 2) Questionnaire Delphi / AI‑Delphi (`questionnaire_ai_delphi`)
Open-ended multi-round questionnaire with controlled feedback and final synthesis.

### 3) Forecasting (`forecasting`)
Participants provide forecasts (and reasoning) over questions; master summarizes signals and uncertainty.

### 4) Priority Ranking (`priority_ranking`)
Participants rank/prioritize items; master computes convergence (e.g., top‑k overlap) and manages iterative refinement.

### 5) Sensemaking (`sensemaking`)
Problem scoping, issue mapping, and synthesis (useful for exploratory Delphi and research framing).

### 6) Idea Generation (`idea_generation`)
Divergent → convergent ideation with master consolidation and refinement.

### 7) Policy/Guidelines (`policy_guidelines`)
Generates structured guidelines/controls and refines via participant review.

### 8) Criteria/Standards (`criteria_standards`)
Develops evaluative criteria/standards and refines them with panel feedback.

### 9) Risk Register (`risk_register`)
Identifies, consolidates, and iteratively improves risk statements (with mitigations where configured).

### 10) Instrument Development (`instrument_development`)
Creates survey/interview items per construct; master consolidates and improves. Requires constructs as input.

### 11) Scenario Building (`scenario_building`)
Generates scenarios and iteratively refines them via participant critique and master synthesis.

### 12) Recursive Reasoning (`recursive_reasoning`)
A structured interpret → solve → converge pipeline:
- **Round 1: pool participants’ understanding + reasoning only** (no solutions)
- master collates interpretations and tensions
- **Round 2: propose solutions + reasoning**
- master collates solutions and open issues
- **Round 3: final solution + final reasoning**
- master produces synthesis with agreements/disagreements and convergence/divergence

---

## Key terms (plain-language definitions)

- **Quorum fraction**: The minimum fraction of *enabled participants* that must respond for a stage to count as valid.  
  Example: quorum fraction 0.7 with 10 participants → at least 7 responses required.

- **Consensus threshold** (template-dependent): A cutoff that defines “agreement” (e.g., rating dispersion below a threshold, or % agreement above a threshold). Used to label items as converged/diverged.

- **Stability threshold** (template-dependent): A cutoff indicating responses are not changing meaningfully between rounds (e.g., item ranks stabilizing, rating variance stabilizing). Used to stop early when additional rounds yield minimal movement.

- **AI‑Delphi mode**: A configuration that determines how participants are prompted and how feedback is framed between rounds (e.g., independence emphasis vs. deliberative critique).

- **Scope guardrails (Iconic Minds)**: Optional constraints designed to reduce hallucination or overreach in specialist panel modes. They do not change the pipeline logic; they modify instruction framing.

---

## Installation (Windows)

### 1) Install Python (recommended)
In **Command Prompt (Admin)**:

```bat
winget install -e --id Python.Python.3.13
```

Close and reopen Command Prompt afterwards.

### 2) Install dependencies
Navigate to the folder containing `app.py`:

```bat
cd /d C:\path\to\synthetic_delphi_app
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

> Dependencies (from `requirements.txt`):
> - streamlit
> - pandas
> - pydantic
> - requests
> - matplotlib
> - numpy
> - python-docx

### 3) Run the app
```bat
python -m streamlit run app.py
```

Or use the included launchers:
- `Launch_Synthetic_Delphi.ps1`
- `Launch_Synthetic_Delphi.bat`

---

## Using the app (quick start)

1. **Study Set‑Up → Participants**
   - Add participants (up to 30)
   - Set a **Global API key** if using one provider/model
2. **Study Set‑Up → Study Design**
   - Choose a template
   - Configure response length policies (global and per-stage)
   - Set project name and report title
3. **Study Set‑Up → Instruments**
   - Provide template inputs (e.g., questionnaire questions, constructs)
4. **Run & Results**
   - Click **Run study now**
   - Review health diagnostics and artifacts
   - Download the **Word report (.docx)**

---

## Data storage and privacy

- All runs and artifacts are stored locally in a **SQLite** database.
- Cohort files (save/load) store study configuration and optionally API keys.
- If you select **“Include API keys”** when saving, keys are stored in the cohort file in plaintext. Treat cohort files as sensitive.

---

## Troubleshooting

- **`streamlit` not recognized**:
  ```bat
  python -m streamlit run app.py
  ```

- **Multiple Python installs** (common on Windows):
  ```bat
  where python
  python -c "import sys; print(sys.executable)"
  ```

- **Word report not generating**:
  Ensure `python-docx` is installed:
  ```bat
  python -m pip install -U python-docx
  ```

- **API errors (400/401/404)**:
  Confirm:
  - base URL (OpenAI default: `https://api.openai.com/v1`)
  - model name is valid for your account
  - API key is set and active
  - temperature/reasoning controls are compatible with the selected model

---

## License / distribution

This application is provided as a local research tool. 
