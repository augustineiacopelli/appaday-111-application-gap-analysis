# AppADay 111 - Application Gap Analysis

Part of [AppADay](https://augustineiacopelli.github.io/appaday/), a daily discipline project by Augustine Iacopelli: one complete, functional, mobile-friendly web app shipped every day.

## What it does

Paste a resume and a job description. Claude extracts every real requirement stated or implied in the posting, then checks the resume against each one individually. The result is a match score computed client-side (never self-graded by the model), an expandable gap grid showing which requirements are met, partial, or missing with evidence and a suggested rewrite for each, a list of structural or ATS-style resume issues, and a set of interview-fit questions worth preparing for.

## How it works

Two sequential Claude API calls. The first reads only the job description and returns structured JSON requirements, each tagged required or preferred, plus any stated salary range and travel percentage. The second reads the resume plus those requirements (never the raw job text again) plus optional role context and an optional ranked strengths list, and returns a gap list, structural findings, and fit questions as JSON. The match score is calculated in plain JavaScript from the gap list's status values, not trusted from the model as a number, and weights required requirements double: a fully missing required item pulls the score down twice as hard as a missing preferred one. A count of fully missing required items surfaces below the score so the percentage is never read in isolation.

Two optional deal breaker settings run client-side against the extracted posting the moment extraction finishes, before the comparison call even starts: a minimum salary requirement, with an option to reject any posting that lists no salary at all, and a maximum travel percentage. A hard remote requirement and a minimum years of experience are checked the same way. All four are pure JavaScript checks against the model's extracted figures, not model judgment calls.

A saved location is passed into the comparison call as known fact about the candidate, so a posting's location, residency, or geographic eligibility requirement is judged against where you actually live rather than only what happens to be typed on the resume itself.

The resume itself is a saved setting rather than a per-analysis paste. Enter it once in Settings and it persists in `localStorage` across sessions, so only the job description changes day to day. A status bar on the main screen shows whether a resume is saved and its word count, with a one-tap link back into Settings to update it. The ranked Strengths list persists the same way.

## Tech

Single self-contained `index.html`. No frameworks, no build step. Google Fonts is the only external dependency. Calls the Anthropic API directly from the browser using the `anthropic-dangerous-direct-browser-access` header. API key, session name, and strengths scope preference are stored in `localStorage`, wrapped in try/catch. The ranked strengths list itself is session-only and never persisted.

Model: `claude-sonnet-5`

## Category

Productivity (P), AI-powered

## Live

https://augustineiacopelli.github.io/appaday-111-application-gap-analysis/
