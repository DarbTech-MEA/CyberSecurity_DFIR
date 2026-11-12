# CDF Weekly Applied Assignment — Submission Repository

This repository is where you submit the current week's **Weekly Applied Assignment** for the DarbTech Cybersecurity Operations & Digital Forensics (CDF) programme. It is separate from the day readings and lab rooms — see [ASSIGNMENT.md](ASSIGNMENT.md) for this week's full brief, scenario, tasks, and grading rubric.

## How to submit

1. **Fork this repository** (the "Fork" button, top right of the GitHub page).
2. In your fork, copy the two templates in `submission/` and rename them:
   - `submission/report_template.md` → `submission/report.md`
   - `submission/layer_template.json` → `submission/layer.json`
3. Fill in `submission/report.md` with your answers to Parts 1–4 of the assignment, and `submission/layer.json` with your completed MITRE ATT&CK Navigator layer (Part 5).
4. Commit and push to your fork.
5. Open a **pull request** from your fork back to this repository. Use your name as the PR title (for example, `J. Alvarez — submission`).
6. A GitHub Action runs automatically on your pull request and checks that both files exist, that `layer.json` is valid and non-empty, and that `report.md` contains all five required sections. **A passing check does not mean your answers are correct** — it only confirms the submission is complete and well-formed. Grading is done by your instructor against the rubric in `ASSIGNMENT.md`.
7. Do not merge your own pull request. Your instructor reviews, grades, and closes it.

## A note on academic integrity

This assignment is built so that a generic AI-generated answer is visibly incomplete: the scenario is original to this course, and one section (the judgment call) has no single correct answer to retrieve — only reasoning grounded in the scenario's own specific details is credited. See the "A Note on AI Use" section in [ASSIGNMENT.md](ASSIGNMENT.md).

## What not to do

- Do not open your pull request against anyone else's fork.
- Do not look at another learner's open pull request before submitting your own. Each submission is individual work.
- Do not edit files outside `submission/` in your fork — only `submission/report.md` and `submission/layer.json` are graded.
