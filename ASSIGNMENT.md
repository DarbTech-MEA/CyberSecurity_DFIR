# CDF Programme — CyberOps Track, Week C5: Weekly Applied Assignment
### Applying MITRE ATT&CK to an Active Directory Engagement

---

## Overview

This is a weekly, individually graded assignment, separate from the day readings and the lab rooms. It is designed to take approximately two hours and is built around a single skill that is directly relevant in the penetration-testing and red-team workplace: taking what was observed during an engagement and writing it up as a technical finding that maps cleanly to the MITRE Adversarial Tactics, Techniques, and Common Knowledge (ATT&CK) framework, the way a real engagement report does.

The scenario below is original to this assignment. It does not describe a real organisation, a real breach, or a publicly documented incident, and no answer key for it exists anywhere outside this course. It draws mainly on this week's Active Directory content (Week C5) and deliberately requires one technique from Week C4 (Password Attacks, Exploitation and Shells, Privilege Escalation) to connect correctly — the assignment cannot be completed from this week's material alone.

**Time estimate:** approximately two hours.
**Individual work.** Discuss concepts with classmates if you like; the report, the evidence citations, and the judgment call below must be your own.
**Authorisation note:** the scenario describes activity performed under a signed engagement authorisation between the fictional firm "Meridian Health Partners" and the fictional testing firm carrying out the work. Nothing in this assignment should be read as, or used as, guidance for unauthorised activity.

---

## Scenario

Meridian Health Partners (MHP) is a mid-size healthcare provider. Your team is conducting an authorised internal penetration test against the `meridianhealth.local` Active Directory domain. Initial access was obtained earlier in the engagement and is documented in a separate technical appendix (out of scope for this assignment): a password-spray attack against the organisation's VPN portal succeeded against the account `j.alvarez` (Finance department), and a subsequent client-side delivery against the same user resulted in code execution on workstation `FIN-WKS014`.

From that foothold, your team's tooling and the domain's own security event logs recorded the following, in order:

**09:05** — `FIN-WKS014` local Security log, Event ID 4672 (Special privileges assigned to new logon): the session running your team's tooling shows `SeDebugPrivilege` and `SeImpersonatePrivilege` assigned to a SYSTEM-level token, roughly twenty minutes after initial code execution and before any domain enumeration began.

**09:14** — Domain controller `MHP-DC01` Security log, Event ID 4768 (A Kerberos authentication ticket (TGT) was requested): `j.alvarez` requests a TGT from the workstation `FIN-WKS014` (10.40.12.114).

**09:17–09:18** — `MHP-DC01` Security log, three Event ID 4769 entries (A Kerberos service ticket was requested), all for `j.alvarez`, all naming the Service Principal Name `MSSQLSvc/mhp-sql01.meridianhealth.local:1433` (the account `svc_sql`), all within ninety seconds, all showing ticket encryption type `0x17` (RC4-HMAC).

**09:20** — `MHP-DC01` Security log, Event ID 4624 (An account was successfully logged on), logon type 3 (network), `j.alvarez` from `FIN-WKS014` — consistent with routine LDAP enumeration traffic.

**11:42** — `MHP-DC01` Security log, Event ID 5136 (A directory service object was modified): an attribute change is recorded against the computer object `FIN-WKS014$`, delegation-related, made under the security context of a helpdesk account, `svc_helpdesk`, that according to the MHP IT asset register has not required interactive or scripted access to computer-object attributes in over a year.

**14:02** — `MHP-DC01` Security log, Event ID 4698 (A scheduled task was created): a task named `WindowsUpdateCheck` is created under the account `svc_backup`, set to run once (not on a recurring trigger), executing PowerShell with a Base64-encoded command argument. MHP's IT runbook, included in your engagement's background documentation, states that all `svc_backup` scheduled jobs are recurring nightly tasks that start at 02:00 and that no ad hoc or one-time `svc_backup` tasks exist anywhere else in the environment.

That is the complete evidence set for this assignment. You are not expected to have access to the actual lab environment to complete it — every detail you need is given above.

---

## Your Task

Produce a single Markdown report, written as you would submit it to an engagement lead, with the following four parts.

### Part 1 — Technique Mapping with Evidence

For each of the five timestamped events above, identify the MITRE ATT&CK technique (and sub-technique, where one applies) that best explains it. For each one:
- Name the technique and its identifier (for example, T1110.003).
- Quote or closely paraphrase the specific detail from the scenario that supports your mapping — a generic description of the technique without a citation back to the scenario's own evidence will not receive credit.
- State which ATT&CK tactic the technique falls under.

### Part 2 — The Judgment Call

Two of the five events above (11:42 and 14:02) could each be read two ways: legitimate administrative activity, or an attacker using a legitimate-looking mechanism as cover. Pick **one** of the two and argue, using only details given in the scenario, why it is more likely to be attacker activity than routine administration. Then explain what would have to be true for your conclusion to be wrong — in other words, what single additional piece of evidence (not given here) would most change your assessment.

This section is graded on the quality of your reasoning and your use of the specific discriminating detail in the scenario, not on reaching any particular conclusion. There is no single correct answer on file for this part; a well-argued case for either event, built on the evidence actually given, can score full marks.

### Part 3 — The Week C4 Connection

One of the five events (09:05) is best explained not by anything from this week's Active Directory material, but by a technique class covered in Week C4. Identify which C4 day's material it connects to, name the likely technique (concept level — do not describe operational exploitation steps), and explain in one or two sentences why privilege escalation on the workstation would logically happen before any Active Directory enumeration, in an engagement like this one.

### Part 4 — Recommendations

For three of the five events (your choice), write the one control or detection you would recommend to Meridian Health Partners' security team to catch or prevent that specific activity in the future. Be specific to the event — "use MFA" or "monitor logs" without naming what should be monitored and why will not receive credit.

### Part 5 — MITRE ATT&CK Navigator Layer

Produce a MITRE ATT&CK Navigator layer file (JSON) plotting every technique you identified in Part 1. Use the skeleton below as your starting point:

```json
{
  "name": "C5 Weekly Assignment — Meridian Health Partners Engagement",
  "versions": { "attack": "17", "navigator": "5.1.0", "layer": "4.5" },
  "domain": "enterprise-attack",
  "description": "Techniques identified from the MHP internal engagement timeline, Week C5 weekly assignment.",
  "techniques": [
    {
      "techniqueID": "REPLACE_WITH_TECHNIQUE_ID",
      "comment": "Replace with a one-line pointer to the timestamped event and evidence that supports this mapping.",
      "color": "#e60000",
      "enabled": true,
      "score": 1
    }
  ],
  "gradient": { "colors": ["#ffffff", "#e60000"], "minValue": 0, "maxValue": 1 },
  "legendItems": [
    { "label": "Identified from engagement timeline", "color": "#e60000" }
  ]
}
```

Add one `techniques` entry per technique you identified (five entries total, including the Week C4 technique from Part 3). Each `comment` must point back to the specific timestamp and evidence that justified it — this is checked directly against your Part 1 citations when the layer is graded.

---

## Submission Requirements

Submit two files:
1. The Markdown report (Parts 1–4), named `[YourName]_C5_Weekly_Assignment.md`.
2. The Navigator layer (Part 5), named `[YourName]_C5_Weekly_Assignment_Layer.json`, valid against the structure above.

## A Note on AI Use

You may use any tool you like to look up ATT&CK technique definitions, IDs, and general background. What you cannot outsource is the evidence citation in Part 1 (it must point to this scenario's own specific details, which appear nowhere else) or the reasoning in Part 2 (there is no existing answer to copy — the judgment call is original to this assignment and has two defensible answers, not one). A submission that reads as a generic description of Kerberoasting, scheduled-task persistence, or privilege escalation in general, without grounding in the five specific events above, will be graded as such regardless of how it was produced.

---

## Grading Rubric (100 points)

| Criterion | Weight | Exceeds expectations | Meets expectations | Developing | Insufficient |
|---|---|---|---|---|---|
| **Part 1 — Technique mapping and evidence citation** | 30 | All five techniques correctly identified with the correct tactic; every citation quotes or precisely paraphrases the specific scenario detail that supports it | Four or five correctly identified; most citations are specific, one or two are generic | Two or three correctly identified, or citations are mostly generic restatements of the technique rather than scenario-specific | Zero or one correctly identified, or no citations tie back to the scenario |
| **Part 2 — Judgment call reasoning** | 25 | Clear position, argued entirely from scenario-specific detail; the "what would change my mind" answer identifies a genuinely discriminating piece of missing evidence | Clear position with mostly scenario-grounded reasoning; the "what would change my mind" answer is plausible but generic | A position is stated but reasoning relies on generic knowledge rather than the scenario's details | No clear position, or reasoning does not engage with the scenario at all |
| **Part 3 — Week C4 connection** | 15 | Correct technique class identified, correctly traced to the right C4 day, with a clear and correct explanation of the workstation-before-domain sequencing logic | Correct technique class and day, with a thinner explanation of the sequencing logic | Technique class or day is partly wrong, or the sequencing explanation is missing | Not attempted or substantially incorrect |
| **Part 4 — Recommendations** | 20 | All three recommendations are specific to their event, naming the exact log source, control, or detection logic that would catch it | Recommendations are reasonable but one is generic ("enable logging") without naming what or how | Recommendations are mostly generic security advice not tied to the specific events chosen | Recommendations are missing or unrelated to the events |
| **Part 5 — Navigator layer** | 10 | Valid JSON, all five techniques present with correct IDs, every comment specifically ties back to the matching Part 1 citation | Valid JSON with all five techniques, but one or two comments are generic rather than specific | Valid JSON but missing techniques, or comments absent | Invalid JSON, or layer not submitted |

---

*Prepared for CDF Programme — CyberOps, Week C5, Weekly Applied Assignment*

© 2026 DarbTech Cybersecurity and Digital Forensics
