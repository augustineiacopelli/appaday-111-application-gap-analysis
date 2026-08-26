# AppADay 111 - Application Gap Analysis

Part of [AppADay](https://augustineiacopelli.github.io/appaday/), a daily discipline project by Augustine Iacopelli: one complete, functional, mobile-friendly web app shipped every day.

## What it does

Paste a resume and a job description. Claude extracts every real requirement stated or implied in the posting, then checks the resume against each one individually. The result is a match score computed client-side (never self-graded by the model), an expandable gap grid showing which requirements are met, partial, or missing with evidence and a suggested rewrite for each, a list of structural or ATS-style resume issues, and a set of interview-fit questions worth preparing for.

## How it works

Two sequential Claude API calls. The first reads only the job description and returns structured JSON requirements. The second reads the resume plus those requirements (never the raw job text again) plus optional role context and an optional ranked strengths list, and returns a gap list, structural findings, and fit questions as JSON. The match score is calculated in plain JavaScript from the gap list's status values, not trusted from the model as a number.

## Tech

Single self-contained `index.html`. No frameworks, no build step. Google Fonts is the only external dependency. Calls the Anthropic API directly from the browser using the `anthropic-dangerous-direct-browser-access` header. API key, session name, and strengths scope preference are stored in `localStorage`, wrapped in try/catch. The ranked strengths list itself is session-only and never persisted.

Model: `claude-sonnet-5`

## Category

Productivity (P), AI-powered

## Live

https://augustineiacopelli.github.io/appaday-111-application-gap-analysis/
