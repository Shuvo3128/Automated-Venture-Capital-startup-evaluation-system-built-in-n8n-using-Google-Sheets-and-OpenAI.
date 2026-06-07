# 🚀 AI Startup Evaluation System

An automated AI-powered startup evaluation system built with **n8n**, **OpenAI GPT**, and **Google Sheets**. The system automatically analyzes startup pitch data and generates investment decisions, scores, and detailed analysis — 24/7 with zero manual work.

---

## 📸 Workflow Overview

<img width="1222" height="589" alt="image" src="https://github.com/user-attachments/assets/67712eb6-9918-432f-9c65-faebf6148665" />


---

## 📋 Features

- ✅ Automatically evaluates startups from multiple sources
- ✅ AI-generated investment decision (Go / Go with more info / NoGo / Too Early / Reject)
- ✅ Scores startups from 0 to 100
- ✅ Provides reasoning, strengths, risks, and recommended next steps
- ✅ Saves all results to Google Sheets
- ✅ Prevents duplicate processing using a `status` column
- ✅ Runs fully automated on a schedule

---

## 🔄 Workflows

### 1️⃣ Startup Application Evaluation Workflow

**Trigger:** Schedule Trigger (every 1 minute)

**Flow:**
```
Schedule Trigger
      ↓
Get rows from Google Sheet (Pitch Application Form)
      ↓
Filter: status != "done"
      ↓
Loop Over Items (one by one)
      ↓
Edit Fields (map startup data)
      ↓
OpenAI GPT (AI evaluation)
      ↓
Parse JSON response
      ↓
Append result to Evaluation Sheet
      ↓
Mark row as "done"
```

**Description:** Reads startup applications submitted via Google Form. Filters unprocessed entries, evaluates each one with AI, saves results to a separate evaluation sheet, and marks each row as done to avoid reprocessing.

---

### 2️⃣ Crunchbase Startup Evaluation Workflow

**Trigger:** Schedule Trigger (every 1 minute)

**Flow:**
```
Schedule Trigger
      ↓
Get rows from Crunchbase Sheet
      ↓
Filter: status != "done"
      ↓
Loop Over Items (one by one)
      ↓
Edit Fields (map Crunchbase data)
      ↓
OpenAI GPT (AI evaluation)
      ↓
Parse JSON response
      ↓
Append result to Evaluation Sheet
      ↓
Mark row as "done"
```

**Description:** Reads company data from a Crunchbase-style Google Sheet. Evaluates each company using funding stage, investor quality, market category, and other signals. Saves AI-generated investment analysis to a separate sheet.

---

### 3️⃣ Email Startup Evaluation Workflow

**Trigger:** Gmail Trigger (monitors inbox for new emails with PDF attachments)

**Flow:**
```
Gmail Trigger (new email with PDF)
      ↓
Get Message (download attachment)
      ↓
Extract from File (PDF → text)
      ↓
AI Agent (OpenAI GPT analysis)
      ↓
Parse JSON response
      ↓
Append result to Evaluation Sheet
```

**Description:** Monitors a Gmail inbox for incoming emails containing pitch deck PDFs. Automatically extracts text from the PDF, sends it to an AI agent for analysis, and saves the evaluation results to Google Sheets.

---

## 🤖 AI Evaluation Output

Each startup receives the following AI-generated fields:

| Field | Description |
|-------|-------------|
| `decision` | Go / Go with more info / NoGo / Too Early / Reject |
| `score` | 0 to 100 investment score |
| `reasoning` | Concise investment-focused analysis |
| `strengths` | List of positive signals |
| `risks` | List of concerns or missing information |
| `recommended_next_step` | Suggested action for the investor |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| [n8n](https://n8n.io) | Workflow automation |
| [OpenAI GPT-4o mini](https://openai.com) | AI startup evaluation |
| [Google Sheets](https://sheets.google.com) | Data source and results storage |
| [Gmail](https://gmail.com) | Email pitch deck monitoring |

---

## ⚙️ Setup Instructions

### Prerequisites
- n8n instance (cloud or self-hosted)
- OpenAI API key
- Google account with Sheets and Gmail access

### Steps

1. **Clone this repository**
```bash
git clone https://github.com/yourusername/ai-startup-evaluation.git
```

2. **Import workflows into n8n**
   - Open n8n
   - Go to **Workflows → Import**
   - Import each JSON file from the `/workflows` folder

3. **Set up credentials in n8n**
   - Add **OpenAI API** credential
   - Add **Google Sheets OAuth2** credential
   - Add **Gmail OAuth2** credential

4. **Configure Google Sheets**
   - Create an input sheet with a `status` column
   - Create an output sheet with columns: `date`, `startup_name`, `decision`, `score`, `reasoning`, `strengths`, `risks`, `recommended_next_step`

5. **Update Sheet IDs**
   - In each workflow, update the Google Sheets document IDs to point to your own sheets

6. **Activate workflows**
   - Turn on each workflow in n8n

---

## 📁 Repository Structure

```
├── workflows/
│   ├── startup_application_evaluation.json
│   ├── crunchbase_evaluation.json
│   └── email_pdf_evaluation.json
├── workflow_overview.png
└── README.md
```

---

## 📊 Example Output

| date | startup_name | decision | score |
|------|-------------|----------|-------|
| 2026-06-02 | NeuroPalm | Go with more info | 70 |
| 2026-06-02 | GeroMap | Go with more info | 65 |
| 2026-06-02 | SahaSense | Go with more info | 65 |

---

## 📝 License

MIT License — feel free to use and modify.
