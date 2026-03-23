# Deploying a Persona as a Gemini Gem

**Audience:** Anyone who has completed the AI Persona build pipeline and wants to deploy their persona in Google's Gemini App.
**Prerequisite:** You have a completed system instruction (use `templates/persona-output-gemini-gem.md`) and at least one reference file package.
**Platform:** Gemini App (gemini.google.com) — personal Gemini Advanced or Google Workspace with Gemini.

---

## What Is a Gem?

A Gem is a saved, reusable Gemini configuration. It stores:
- A system instruction (your persona definition)
- Up to 10 uploaded reference files
- A name and description visible only to you

Every conversation you start with the Gem begins with that context pre-loaded. Gems are private unless you explicitly share them. They are not agents — they do not call external APIs or tools by default.

---

## Step 1: Access Gems

1. Open [gemini.google.com](https://gemini.google.com)
2. In the left sidebar, find **Gems** (below recent conversations)
3. Click **Gem manager**
4. Click **New Gem** (top right)

**Requirements:**
- Gemini Advanced subscription, OR
- Google Workspace account with Gemini for Workspace license

If you don't see Gems: your plan does not include them. Upgrade to Gemini Advanced ($19.99/month) or check with your Workspace admin.

---

## Step 2: Name and Describe the Gem

| Field | Guidance |
|-------|----------|
| **Name** | `[PersonName]AI` — short, recognizable. This appears in your sidebar. |
| **Description** | One sentence: what the Gem does and for whom. Example: "Drafts communications in Alex's voice using her psychometric profile and vocabulary fingerprint." |

The description is for your own reference. It does not affect behavior.

---

## Step 3: Write the System Instruction

This is the most important step. The system instruction defines everything the Gem does.

**Character limit:** Approximately 8,000–10,000 characters (varies; the UI will warn you if you exceed it).

**Structure to follow** (full template in `templates/persona-output-gemini-gem.md`):

```
1. Identity block — who this persona is, what they do, what every output is for
2. Psychometric core — 4–6 lines summarizing instrument results and what they mean
3. Universal rules — banned vocabulary, approved vocabulary, behavioral constraints
4. Communication modes — mode-detection logic, rules per channel/language
5. Content modes — meeting prep, presentations, brainstorming, etc.
6. Style self-check — the internal checklist the Gem runs before responding
7. Interaction style — how the Gem behaves when talking TO the user (not drafting)
```

**Critical principle:** Reference your uploaded files by name in the instruction. Example:
> "See attached `02-Vocabulary-Fingerprint.md` for the full phrase frequency data."

Gemini will consult the uploaded file when processing relevant requests, but only if you establish the reference. Without an explicit pointer, the model may not use the file.

**What NOT to put in the system instruction:**
- Raw data (move this to reference files — it wastes character budget)
- Generic instructions that apply to all AI (e.g., "be helpful")
- More than 3 modes — if you have more, consolidate or reference a file

---

## Step 4: Upload Reference Files

Gems support up to **10 file attachments**. Use this budget deliberately.

### Recommended File Allocation

| Priority | File | Contents |
|----------|------|----------|
| 1 | `01-psychometric-profile.md` | Full instrument scores, triangulated traits, behavioral fingerprint, anti-persona, tone spectrum |
| 2 | `02-vocabulary-fingerprint.md` | Top phrases with frequencies, approval/ban lists, intensifier/hedge profiles, tech sociolect |
| 3 | `03-tone-emotion-guide.md` | Emotional states with exact markers, platform-dependent tone rules |
| 4 | `04-chat-patterns.md` | Chat mechanics, conversational flow patterns, sample outputs |
| 5 | `05-email-patterns.md` | Length distribution, greeting/sign-off frequencies, body structure rules, templates |
| 6 | `06-domain-knowledge.md` | Professional expertise, product knowledge, case studies, competitive intelligence |
| 7–10 | Domain-specific extras | Customer lists, product docs, language-specific references, prior conversation examples |

**File format:** Markdown `.md` is optimal. Plain text `.txt` works. Avoid PDFs — they are harder for the model to parse precisely.

**File size:** Keep each file under 500KB. The model reads the content, so dense/compact files outperform padded ones.

### How to Upload

In the Gem editor, click the paperclip icon below the system instruction field. Upload files one at a time or drag and drop. Files persist as long as the Gem exists.

---

## Step 5: Save and Test

Click **Save**. The Gem is now available in your sidebar.

### Initial Test Protocol

Run these prompts in sequence against your new Gem:

**Test 1 — Mode detection:**
> "Draft a short email to a client confirming our next meeting."

Check: Does it detect the correct mode (external email)? Does it use the right greeting and sign-off? Are banned words absent?

**Test 2 — Vocabulary compliance:**
> "Can you leverage our synergies to circle back on this?"

The Gem should produce output that does NOT contain these words. It should replace them or ignore them and respond in the persona's voice.

**Test 3 — Anti-persona check:**
> "Let's explore all possible perspectives and build consensus before deciding."

If the persona is direct/competing-dominant, it should push back or redirect — not agree and facilitate a consensus session.

**Test 4 — Constraint check:**
> "Tell me the deal size for the client we discussed."

The Gem should decline per behavioral constraints — not fabricate.

**Test 5 — Correct channel:**
> "Write me a casual Slack message to my teammate about rescheduling."

Check: lowercase, no bullets, no formal greeting, one message block.

### Scoring the Test

| Pass Criteria | Check |
|---------------|-------|
| No banned words in any output | |
| Correct greeting/sign-off per mode | |
| Vocabulary matches approved list | |
| Behavioral constraints enforced | |
| Anti-persona held under pressure | |
| Channel formatting correct | |

If 5/6 pass: Gem is production-ready. If fewer: iterate on the system instruction — almost always the issue is in the vocabulary or mode-detection sections.

---

## Step 6: Iteration

### Fastest Iteration Loop

1. Identify the failure in the test output
2. Find the section of the system instruction responsible
3. Make the rule more explicit — vague rules fail, concrete rules hold
4. Save and re-run the specific test that failed

**Rule of thumb:** If the Gem is doing something wrong once, it will do it every time. Fix it in the instruction, not by correcting individual outputs.

### Common Refinements

**If the Gem is too formal in chat mode:**
Add: "In chat modes, a single period at the end of a message is a formatting violation."

**If the Gem uses banned words under pressure:**
Add the word explicitly to the banned list. General bans ("no corporate jargon") are weaker than specific bans ("never write 'synergy', 'leverage' as a verb, 'circle back'").

**If the Gem hedges on technical facts:**
Add: "On technical facts, use binary statements. A system either supports X or it does not. 'Sort of' and 'kind of' are banned for technical claims."

**If the Gem volunteers information that should be confidential:**
Add explicit constraint: "Never disclose [specific category]. If asked, respond: '[Redirect phrase].'"

---

## Common Mistakes

| Mistake | Consequence | Fix |
|---------|-------------|-----|
| No reference file pointers in system instruction | Gem ignores uploaded files | Add explicit "see attached [filename]" per section |
| Storing raw data in the system instruction | Wastes character budget, dilutes instruction quality | Move data to reference files |
| Vague vocabulary rules ("avoid corporate speak") | Gem complies partially | List every banned word explicitly |
| No anti-persona definition | Gem defaults to agreeable behavior | Add "NOT" statements for every key trait |
| No mode-detection logic | Gem applies wrong register | Add explicit detection rules ("if asked to write an email...") |
| Skipping the style self-check section | Banned words slip through | Include the self-check as the last section |
| One monolithic instruction with no structure | Hard to debug | Use headers — Identity, Rules, Modes, Self-Check |
| Uploading PDFs | Model parses less accurately | Convert to Markdown |
| No test protocol | Unknown quality | Run all 5 test prompts before using in production |

---

## Sharing a Gem

Gems are private by default. To share:
- In Gem Manager, open the Gem
- Click the share icon (if available in your plan)
- Share via link — recipient must have Gemini Advanced

Note: Sharing exposes the system instruction and files to the recipient. Remove any personal or confidential data before sharing a persona Gem.

---

## Limits Reference

| Parameter | Limit |
|-----------|-------|
| System instruction length | ~8,000–10,000 characters |
| File attachments | 10 files max |
| File size per attachment | ~10MB (practical limit for text: 500KB) |
| File formats supported | .md, .txt, .pdf, .docx, .csv, .png, .jpg |
| Gems per account | No published hard limit (100+ reported as working) |
| Conversation context window | Gemini 2.5 Flash: 1M tokens; Gemini 2.5 Pro: 1M tokens |

---

*Last verified: March 2026. Gemini product updates may change UI elements — the underlying approach is stable.*
