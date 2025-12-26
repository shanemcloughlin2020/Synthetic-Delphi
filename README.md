# Synthetic-Delphi 
Application to set up, save, load and autonomously run 12 types of Delphi Studies using cohorts of synthetic participant personas 

The local Streamlit application to run **LLM-based Delphi studies** with up to 30 participant agents and a master “researcher” (facilitator) agent.

## Supported study templates

The application supports multiple Delphi “pipelines” (templates), each with an appropriate orchestration pattern and outputs:

1. **Item-rating Delphi (consensus-building)** (`item_rating`)
   - Optional Round 1 elicitation (or seed items)
   - Master consolidation
   - Structured rating rounds with controlled feedback
   - Consensus and stability diagnostics, plus “converged vs diverged” mapping

2. **Questionnaire Delphi (AI-Delphi style)** (`questionnaire_ai_delphi`)
   - Open-ended questionnaire rounds
   - Master-controlled feedback between rounds
   - Final synthesis report

3. **Forecasting** (`forecasting`)
   - Probability elicitation per question
   - Optional confidence-weighted aggregation
   - Bayesian pooling (Beta) with Monte-Carlo credible intervals

4. **Priority setting** (`priority_ranking`)
   - Rank/order a consolidated list
   - Borda aggregation + Kendall’s W (agreement)
   - Stability check via top-k overlap

5. **Problem scoping / sensemaking** (`sensemaking`)
   - Glossary, assumptions, boundaries, uncertainties
   - Master consolidation and (optional) validation round

6. **Idea generation** (`idea_generation`)
   - Divergent idea elicitation
   - Master clustering and consolidation

7. **Criteria / standards development** (`criteria_standards`)
   - Elicit statements (or seed)
   - Rate acceptability/feasibility

8. **Policy / guideline formation** (`policy_guidelines`)
   - Elicit statements (or seed)
   - Rate acceptability/feasibility

9. **Risk identification & mitigation planning** (`risk_register`)
   - Elicit risks + mitigations
   - Rate likelihood/impact and produce a prioritized risk register

10. **Measurement & instrument development** (`instrument_development`)
   - Generate items per construct
   - Rate relevance/clarity and capture edit suggestions

11. **Scenario building** (`scenario_building`)
   - Elicit key drivers/uncertainties
   - Master generates scenarios
   - Panel critiques scenarios

## 1) Install dependencies (no virtual environment)

Open **Command Prompt** (cmd.exe) or **PowerShell**.

```bash
python -m pip install --upgrade pip
pip install streamlit pandas pydantic requests matplotlib numpy
```

Optional (recommended for development/testing):

```bash
pip install pytest
```

## 2) Run the app

### Open in a new browser window (recommended)

Use the included launcher scripts (they start the server and attempt to open a **new browser window**):

- PowerShell: `Launch_Synthetic_Delphi.ps1`
- Double-click: `Launch_Synthetic_Delphi.bat`

### PowerShell

```powershell
cd C:\synthetic_delphi_app
python -m streamlit run app.py
```

### Command Prompt (cmd.exe)

```bat
cd /d C:\synthetic_delphi_app
python -m streamlit run app.py
```

## 3) API keys

- The UI uses a **common API key** by default (Step 1). Each participant is still treated as a **separate agent with separate calls and task tracking**.
- You may also set an environment variable:

### PowerShell

```powershell
setx OPENAI_API_KEY "YOUR_KEY_HERE"
```

Open a **new** terminal after setting it.

### Command Prompt

```bat
setx OPENAI_API_KEY "YOUR_KEY_HERE"
```



## 3.5) Project manager (save/load)

The sidebar includes a **Project** manager which can **save** and **load** a full study setup:
- Participants (including per-agent model/provider settings)
- Master facilitator settings
- Study design + instruments
- Response length/structure policies

You can choose whether to include API keys in the saved file.
Saved files are written to the `cohorts/` folder next to `app.py`.

## 4) Storage

Runs are stored in a local SQLite database (default: `synthetic_delphi.sqlite3` in the app folder).
You can set a different DB path in **Run & Results**.



## 4.5) Response length policies (separate from max_tokens)

The app distinguishes between:
- `max_tokens`: a hard upper bound at the provider level
- **Response length policies**: word-count guidance and (optionally) strict enforcement per stage

You can configure:
- A global policy for **participants** and for the **master facilitator**
- Optional **per-stage overrides** (participants and master can differ)

This is independent of model "thinking" settings (reasoning effort), which affects internal computation rather than output structure.

## 4.6) Report download

In **Run & Results**, you can download a consultancy-style **RTF report** (Microsoft Word compatible) that includes an abstract, method summary, participant list, and template-specific findings.

## 5) Notes on “Iconic Minds” personas

If you select **Iconic Minds** mode, consider enabling scope guardrails to reduce misrepresentation risk.

## 6) Troubleshooting

- If `streamlit` is not recognized, run via Python:

```bash
python -m streamlit run app.py
```

- If you see API errors, confirm:
  - base URL is correct (OpenAI default: `https://api.openai.com/v1`)
  - model name is valid for your provider
  - API key is set

