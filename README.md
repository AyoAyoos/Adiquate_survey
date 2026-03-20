# AQ Assessment — Adversity Quotient Research

This project is a single-page, multi-step survey UI for the **AQ Assessment (Adversity Quotient Research Study)**.

It collects personal details, questionnaire responses, a technical subject score, and up to 3 career role preferences, then computes an AQ score and shows a success screen.

## Quick Start

1. Open `Index.html` in your browser (double-click it or use your browser's "Open File").
2. Fill out all 7 steps.
3. Click **Submit** on the final step.

## Project Files

- `Index.html` - The survey UI (7 steps), including the consent banner and success overlay.
- `style.css` - Styling (Material Design 3-inspired tokens + responsive layout).
- `script.js` - Form logic: validation, step navigation, job role limit (max 3), AQ scoring, and submission.

## Survey Structure (7 Steps)

1. **Personal Info**: `fullName`, `email`, `universityName`, `universityId`
2. **Resilience & Mindset**: Q1 - Q5
3. **Adaptability & Response**: Q6 - Q11
4. **Adversity Reach**: Q12 - Q16
5. **Situation Ownership**: Q17 - Q21
6. **Recovery & Technical Profile**: Q22 - Q23 + `dsaMarks`
7. **Career Preferences**: Select **1 to 3** job roles

## Validation Rules (Frontend)

- Step 1 fields must be non-empty; `email` uses a basic email regex.
- Steps 2-5: all MCQ answers must be selected (Q1-Q21).
- Step 6: Q22-Q23 must be selected and `dsaMarks` must be provided.
- `dsaMarks` must be in the format `marks/total` (example: `75/100`) and obtained marks cannot exceed total marks.
- Job preferences:
  - User must select at least **1** role.
  - User can select at most **3** roles (the UI prevents choosing a 4th).

## Scoring

The script computes:

- `aqScore`: sum of answers for `q1` through `q23` (each answer is 1-5)
- `aqMax`: `23 * 5 = 115`
- `aqPercentage`: `aqScore / aqMax * 100` (rounded)
- `aqCategory`:
  - `score >= 100`: High AQ — Climber
  - `score >= 75`: Above Average AQ
  - `score >= 50`: Average AQ — Camper
  - otherwise: Low AQ — Quitter

## Submitting Responses (Backend Wiring)

`script.js` sends the collected payload via `fetch()` to an API endpoint:

- `ENDPOINT` is currently set to: `/api/submit`
- The POST body is JSON.

If the request fails (for example, no backend is running), the UI falls back to a **demo success** screen and still shows the computed AQ results.

To enable real submission:

1. Update `ENDPOINT` in `script.js` to your backend URL.
2. Implement `POST /api/submit` (or change it to match your route).
3. Make your backend return a success response (`response.ok` should be true).

## Data Collected (Frontend Payload)

The submission payload includes:

- `personalInfo` (`fullName`, `email`, `universityName`, `universityId`)
- `responses` for `q1`..`q23`
- `dsaMarks`
- `jobPreferences` (array of selected roles)
- `submittedAt` (ISO timestamp)
- computed scoring fields (`aqScore`, `aqMax`, `aqPercentage`, `aqCategory`)

## Notes

- This repository is frontend-only; it does not store data by itself.
- The consent banner states responses are collected for academic research and personal data is kept confidential; ensure your backend handling matches your privacy requirements.

