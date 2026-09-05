# Candidate Screening & Interview Scheduling Automation

An AI-powered candidate screening and interview workflow built using **n8n, Gmail, Google Sheets, and Gemini**.

This project automates the initial candidate screening process by extracting information from resumes, storing candidate details in Google Sheets, and triggering appropriate email notifications based on the HR-selected candidate status.

---

## 🚀 Project Overview

The automation consists of two n8n workflows.

### Workflow 1 — Resume Intake & Candidate Screening

When a candidate sends a resume as an email attachment:

1. Gmail receives the resume.
2. The resume attachment is downloaded.
3. The resume PDF is converted into text.
4. Gemini AI extracts structured candidate information.
5. The candidate details are appended to Google Sheets.
6. The candidate's initial status is set to `Pending`.

### Workflow 2 — Candidate Status Processing

HR manually updates the candidate's status in Google Sheets.

The automation then processes the status:

- `Selected` → Candidate receives a selection/next-round email and HR receives candidate details.
- `Rejected` → Candidate receives a polite rejection email.
- `On Hold` → No email is sent.
- `Pending` → No email is sent.

---

## 🏗️ Architecture

```text
                    ┌─────────────────────┐
                    │    Candidate Email  │
                    │   Resume Attachment │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Gmail Trigger    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Get a Message    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Extract from File  │
                    │     PDF → Text      │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │  Gemini AI /        │
                    │ Information         │
                    │ Extractor           │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Google Sheets     │
                    │ Candidate Database  │
                    │ Status = Pending    │
                    └──────────┬──────────┘
                               │
                         HR updates status
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Google Sheets       │
                    │ Trigger             │
                    └──────────┬──────────┘
                               │
                               ▼
                         ┌─────────────┐
                         │ Status IF   │
                         └──────┬──────┘
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
          Selected           Rejected         Pending/On Hold
             │                  │                  │
             ▼                  ▼                  ▼
      Candidate Email     Rejection Email       No Action
             │
             ▼
         HR Email
