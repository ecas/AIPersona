# Persona Integration Across Google Workspace

**Audience:** Users and admins who want to activate a persona agent across the full Google Workspace surface area.
**Prerequisite:** Completed persona build + either a deployed Gem or Enterprise Agent (see companion guides).
**Scope:** Gmail, Google Chat, Google Docs, Google Sheets, Google Calendar, Google Slides, Apps Script automations.

---

## Integration Philosophy

A Workspace-integrated persona is not a single bot — it is a layer that can surface wherever the user works. Each Workspace app has different integration depth:

| App | Integration Depth | Best Use |
|-----|-------------------|----------|
| Gmail | Medium (compose assist) | Email drafts in persona voice |
| Google Chat | Deep (full agent) | Real-time drafts, Q&A, research |
| Google Docs | Medium (sidebar / Gemini tab) | Long-form writing in persona style |
| Google Sheets | Light (Gemini panel) | Data summaries in persona voice |
| Google Calendar | Light (meeting prep) | Pre-meeting briefs |
| Google Slides | Light (speaker notes) | Presentation coaching |
| Apps Script | Deep (automation) | Persona-driven workflows, triggers |

The depth reflects what the platform currently supports natively. Apps Script fills the gaps.

---

## 1. Gmail: Compose Assistant and Reply Suggestions

### What's Possible

- Gemini in Gmail (built-in) can draft emails, but uses Google's generic Gemini, not your persona
- Custom persona in Gmail requires either:
  - A Chrome extension (see Apps Script section)
  - Workflow via Chat: draft in Chat > copy to Gmail
  - Google AppSheet or Workspace Add-on (developer path)

### Native Gemini in Gmail (Quick Win)

Even without a custom agent, you can inject persona voice into Gmail's built-in Gemini:

1. In a Gmail compose window, click the Gemini star icon (bottom toolbar)
2. Type your prompt in persona voice:
   > "Draft a follow-up email in a direct, casual professional tone. Lead with the action item. Skip 'hope this email finds you well' and similar phrases. Sign off with 'Thanks!' and first name only."

This is manual but zero-setup. Use it while a proper integration is being built.

### Custom Persona via Gmail Add-on (Advanced)

Build a Workspace Add-on that adds a sidebar to Gmail:

1. In Apps Script, create a new project linked to your Workspace
2. Implement a callback function (e.g., `buildPersonaSidebar`) to render a sidebar card, and declare it in `appsscript.json` under `gmail.contextualTriggers[].onTriggerFunction`
3. Call the Gemini API (Google AI Developer API, same endpoint as in the Apps Script section below) with your persona system instruction
4. Display the generated draft in the sidebar — user copies it into compose

Full implementation template in the Apps Script section below.

### Reply Suggestion Pattern

When viewing a received email, the persona can suggest a reply. Trigger:
1. User opens email
2. Clicks persona sidebar button: "Draft reply in my voice"
3. Add-on reads the email content (via Gmail API in Apps Script)
4. Sends to Gemini API with persona instruction + email context
5. Returns draft in sidebar

---

## 2. Google Chat: Full Agent Integration

### This is the Primary Integration Surface

Google Chat supports full custom agents with bi-directional messaging, mentions, and persistent sessions. This is where the persona works best.

### Setup Recap

See `gemini-enterprise-agent-setup.md` Part 3.1 for complete Chat setup instructions.

### Usage Patterns in Chat

**Pattern 1: Private drafting workspace**
Open a DM with the persona agent. Treat it as your private drafting assistant. All conversations stay in that thread.

Example prompts:
```
Draft an email to [client name] confirming the meeting on Thursday at 2pm.
Reply to the message above — keep it under 3 sentences.
What are 3 ways to frame this request that won't sound like a demand?
```

**Pattern 2: Group chat assist**
In a team Chat space, mention the agent:
```
@PersonaName draft a summary of what we've decided in this thread
@PersonaName write a follow-up message to send to the client about the delay
```

**Pattern 3: Research mode**
```
@PersonaName research [Company X] and give me a 1-page customer brief
@PersonaName what do we know about [person]'s role before my meeting tomorrow?
```

**Pattern 4: Meeting prep trigger**
```
@PersonaName prepare me for my meeting with [Client] at 3pm — here's the context: [paste notes]
```

### Chat Bot Response Format

Configure the agent's Chat-specific behavior in the system instruction:

```
In Google Chat:
- All responses are single-message blocks (never multi-part)
- Formatting: Use bold for headers, bullet points for lists — Chat renders markdown
- Length: Match the ask. Chat question = short answer. Drafting request = full draft.
- Never use Chat for final output — always label drafts as "[DRAFT — review before sending]"
```

---

## 3. Google Docs: Writing Assistant with Persona Style

### Native Path (Gemini in Docs)

Google Docs has a built-in Gemini panel (Help Me Write / Gemini tab in the sidebar).

**How to use it for persona-style writing:**

1. Open the Gemini side panel (click Gemini icon, right side of toolbar)
2. Paste a brief prompt establishing persona context, then your actual request:
   > "[Persona context: direct, no corporate jargon, conclusions first, concrete over abstract.] Write an executive summary of the following content in this style: [paste content]"

This is not a true persona integration — it's prompt injection. It works but requires repetition each session.

### Docs Add-on (Persistent Persona)

Build or deploy a Docs Workspace Add-on that adds a sidebar panel:

1. Panel shows a text input: "What do you want to write?"
2. User inputs the request
3. Add-on calls the Gemini API with persona system instruction
4. Output appears in sidebar — user clicks "Insert" to add to doc

**Apps Script trigger for Docs** (see Section 7 for full code):
```javascript
function onOpen() {
  DocumentApp.getUi()
    .createAddonMenu()
    .addItem('Open Persona Assistant', 'showSidebar')
    .addToUi();
}
```

### Long-Form Writing Rules (add to persona instruction for Docs mode)

```
In Docs mode (long-form writing):
- Apply the same vocabulary rules as email: no banned words, no AI-isms
- Section headers are conclusions (3-7 words), not topics
- Paragraphs: max 3 sentences for non-technical content, 5 for technical
- Never use passive voice where active is possible
- Data points over generic claims — if you can't quantify, say "no data available"
- End every section with an implication, not a summary
```

---

## 4. Google Sheets: Data Analysis in Persona Voice

### Use Case

The persona agent summarizes data, writes insights, and formats findings in the persona's communication style — not in generic analyst voice.

### Native Path

In Google Sheets, Gemini is available in the side panel for data questions. Same prompt-injection approach as Docs:
> "Summarize the trend in column C in 2 sentences. Use direct language — lead with the finding, then explain why."

### Sheets Add-on Pattern

For recurring analysis:

1. Create a custom menu item: "Persona: Summarize selection"
2. Script reads the selected range
3. Sends to Gemini API: `[persona instruction] Summarize this data for a business audience: [table data]`
4. Output written into a designated summary cell or sidebar

### Communication Style Rules for Sheets Mode

Add to persona instruction:

```
When summarizing data:
- Lead with the insight, not the methodology
- Numbers: always include units and time period ("Q1 2026 revenue: $2.3M, up 12% YoY")
- Never: "The data suggests that..." -> replace with "Revenue grew because..."
- Maximum 3 bullet points per data summary
- End with: one recommended action based on the data
```

---

## 5. Google Calendar: Meeting Prep Using Persona's Approach

### Native Path (Gemini in Calendar)

Google Calendar's Gemini integration (if enabled in your Workspace) can summarize meeting context. It does not support custom personas natively.

**Manual workflow:**
1. User opens upcoming event
2. Copies attendees, agenda, and description into Chat
3. Sends to persona agent: "Prepare me for this meeting — here are the details: [paste]"
4. Agent returns a pre-meeting brief in the persona's format

### Pre-Meeting Brief Format (define in persona instruction)

```
MEETING PREP MODE — Output format:
1. Meeting Objective (1 sentence)
2. Attendee Context (name, role, what they care about — 2 lines max each)
3. Key Background (3-5 bullet points)
4. My Role in This Meeting (1-2 lines)
5. Recommended Agenda (table: Time | Topic | Owner)
6. Watch Points (1-3 items — potential tensions or landmines)
7. Ideal Outcome (1 sentence)

Rules: Max 1 page when printed. No fabricated data. End with concrete next step.
```

### Apps Script Automation (Calendar Trigger)

Automatically generate pre-meeting briefs 30 minutes before each calendar event.

**Note:** This is a conceptual outline. For a complete, production-ready implementation — including the `callGeminiAPI` utility and email delivery — see **Section 7, Automation 4**.

```javascript
// Conceptual outline — see Section 7 Automation 4 for full implementation

function createMeetingBriefTrigger() {
  ScriptApp.newTrigger('generatePreMeetingBrief')
    .timeBased()
    .everyMinutes(30)
    .create();
}

function generatePreMeetingBrief() {
  const now = new Date();
  const thirtyMinutes = new Date(now.getTime() + 30 * 60 * 1000);
  const events = CalendarApp.getDefaultCalendar()
    .getEvents(now, thirtyMinutes);

  events.forEach(event => {
    // Build a natural-language prompt from the event details
    const prompt = `Generate a pre-meeting brief for: ${event.getTitle()}`;
    // PERSONA_SYSTEM_INSTRUCTION = your completed persona system instruction string
    const brief = callGeminiAPI(prompt, PERSONA_SYSTEM_INSTRUCTION);
    // Deliver the brief — options: GmailApp.sendEmail(...) or a Chat message
    GmailApp.sendEmail(Session.getActiveUser().getEmail(),
      `Pre-Meeting Brief: ${event.getTitle()}`, brief);
  });
}
```

---

## 6. Google Slides: Presentation Coaching in Persona Voice

### Use Case

The persona reviews slides and provides feedback in the persona's style: direct, opinionated, no generic polish suggestions.

### Native Path

Gemini in Slides can generate speaker notes and suggest layouts. For persona voice:
1. Open the Gemini panel in Slides
2. Select a slide
3. Prompt: "Write speaker notes for this slide. Keep it conversational — like talking points, not a script. Lead with the key message. Max 3 sentences."

### Persona-Specific Slide Review Rules

```
SLIDE REVIEW MODE:
- Evaluate headlines first: are they conclusions (3-7 words) or just topics?
- Flag any slide with more than 3 bullet points
- Flag any claim without a data point
- Evaluate the narrative arc: World Today > Gap > Vision > How > Next Steps
- Speaker notes: conversational, not scripted. "What this actually does is..." framing.
- Feedback style: direct, specific. "Slide 3 headline is a topic, not a conclusion. Change to: [proposed headline]."
- Never give generic feedback like "consider adding more visuals"
```

### Apps Script: Batch Slide Analysis

```javascript
function analyzePresentation() {
  const presentation = SlidesApp.getActivePresentation();
  const slides = presentation.getSlides();

  const slideData = slides.map((slide, index) => ({
    number: index + 1,
    title: getSlideTitle(slide),
    body: getSlideBody(slide),
    speakerNotes: slide.getSpeakerNotesShape().getText().asString()
  }));

  const analysis = callGeminiAPI(
    `Review this presentation. For each slide: evaluate headline quality, bullet count, data presence. Apply persona's slide rules. Return feedback as a table: Slide # | Issue | Recommended Fix.`,
    PERSONA_SYSTEM_INSTRUCTION + SLIDE_REVIEW_MODE_ADDENDUM,
    JSON.stringify(slideData)
  );

  // Write analysis to a new slide at the end of the deck
  appendFeedbackSlide(presentation, analysis);
}
```

---

## 7. Apps Script Automations for Persona-Driven Workflows

### Core Utility: Gemini API Call Function

Reuse this across all Apps Script integrations:

```javascript
const GEMINI_API_KEY = PropertiesService.getScriptProperties().getProperty('GEMINI_API_KEY');
const GEMINI_ENDPOINT = 'https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent';

// Load persona instruction from Drive (keeps it maintainable)
function getPersonaInstruction() {
  const instructionFileId = PropertiesService.getScriptProperties()
    .getProperty('PERSONA_INSTRUCTION_FILE_ID');
  return DriveApp.getFileById(instructionFileId).getBlob().getDataAsString();
}

function callGeminiAPI(userPrompt, systemInstruction, additionalContext) {
  const payload = {
    system_instruction: {
      parts: [{ text: systemInstruction }]
    },
    contents: [{
      role: 'user',
      parts: [{ text: additionalContext ? `${userPrompt}\n\nContext:\n${additionalContext}` : userPrompt }]
    }],
    generationConfig: {
      temperature: 0.7,
      maxOutputTokens: 2048
    }
  };

  const options = {
    method: 'post',
    contentType: 'application/json',
    headers: { 'x-goog-api-key': GEMINI_API_KEY },
    payload: JSON.stringify(payload),
    muteHttpExceptions: true
  };

  const response = UrlFetchApp.fetch(GEMINI_ENDPOINT, options);
  const result = JSON.parse(response.getContentText());

  if (result.error) {
    throw new Error(`Gemini API error: ${result.error.message}`);
  }

  return result.candidates[0].content.parts[0].text;
}
```

**Security note:** Store `GEMINI_API_KEY` in Script Properties, never hardcode it. Use a service account key or Workspace-scoped OAuth for production deployments.

### Automation 1: Email Reply Drafts (Gmail Add-on Trigger)

**Important:** This function is a Gmail contextual trigger callback. It is not called automatically by name — you must declare it in the Apps Script manifest (`appsscript.json`) under `gmail.contextualTriggers[].onTriggerFunction`. The function name can be anything you choose; `onGmailMessage` is used here for clarity.

In `appsscript.json`, add:
```json
"gmail": {
  "contextualTriggers": [{
    "unconditional": {},
    "onTriggerFunction": "onGmailMessage"
  }]
}
```

```javascript
function onGmailMessage(e) {
  const accessToken = e.messageMetadata.accessToken;
  GmailApp.setCurrentMessageAccessToken(accessToken);
  const message = GmailApp.getMessageById(e.messageMetadata.messageId);

  const card = buildReplyDraftCard(message.getPlainBody(), message.getFrom());
  return [card];
}

function buildReplyDraftCard(emailBody, sender) {
  const draft = callGeminiAPI(
    `Draft a reply to this email in the persona's voice. Apply Mode 1 (external email) rules.`,
    getPersonaInstruction(),
    `From: ${sender}\n\nEmail content:\n${emailBody}`
  );

  return CardService.newCardBuilder()
    .setHeader(CardService.newCardHeader().setTitle('Persona Draft'))
    .addSection(
      CardService.newCardSection()
        .addWidget(CardService.newTextParagraph().setText(draft))
        .addWidget(
          CardService.newTextButton()
            .setText('Copy to Compose')
            .setOnClickAction(CardService.newAction().setFunctionName('copyDraftToCompose'))
        )
    ).build();
}
```

### Automation 2: Chat Summary → Email Draft Pipeline

Triggered when a user sends a specific message in Chat:

```javascript
// Deployed as a Chat app
function onMessage(event) {
  const message = event.message.text;
  const sender = event.message.sender.displayName;

  if (message.toLowerCase().startsWith('draft email:')) {
    const request = message.replace(/^draft email:/i, '').trim();
    const draft = callGeminiAPI(
      `Draft an email based on this request: ${request}`,
      getPersonaInstruction()
    );

    return {
      text: `*[DRAFT — review before sending]*\n\n${draft}`
    };
  }

  // Default: process as persona conversation
  const response = callGeminiAPI(message, getPersonaInstruction());
  return { text: response };
}
```

### Automation 3: Weekly Comms Review

Runs every Friday. Analyzes outgoing emails from the week and produces a style consistency report:

```javascript
function formatDateForGmailSearch(date) {
  // Gmail search requires YYYY/MM/DD format
  const yyyy = date.getFullYear();
  const mm = String(date.getMonth() + 1).padStart(2, '0');
  const dd = String(date.getDate()).padStart(2, '0');
  return `${yyyy}/${mm}/${dd}`;
}

function weeklyCommsReview() {
  const oneWeekAgo = new Date(Date.now() - 7 * 24 * 60 * 60 * 1000);
  const threads = GmailApp.search(`in:sent after:${formatDateForGmailSearch(oneWeekAgo)}`, 0, 20);

  const emailSamples = threads.slice(0, 10).map(thread => {
    const msg = thread.getMessages()[0];
    return `Subject: ${msg.getSubject()}\n${msg.getPlainBody().substring(0, 500)}`;
  }).join('\n---\n');

  const review = callGeminiAPI(
    'Review these sent emails for persona consistency. Check for: banned words, correct greeting/sign-off, appropriate formality level, vocabulary compliance. Produce a table: Email # | Issue Found | Severity (Low/Medium/High). Finish with 2-3 patterns to improve next week.',
    getPersonaInstruction(),
    emailSamples
  );

  // Send review to self
  GmailApp.sendEmail(
    Session.getActiveUser().getEmail(),
    'Weekly Persona Consistency Report',
    review
  );
}
```

### Automation 4: Meeting Prep Auto-Brief

```javascript
function scheduleMeetingBriefs() {
  // Delete existing trigger first to avoid duplicates
  ScriptApp.getProjectTriggers()
    .filter(t => t.getHandlerFunction() === 'sendPreMeetingBriefs')
    .forEach(t => ScriptApp.deleteTrigger(t));

  ScriptApp.newTrigger('sendPreMeetingBriefs')
    .timeBased()
    .everyHours(1)
    .create();
}

function sendPreMeetingBriefs() {
  const now = new Date();
  const in30min = new Date(now.getTime() + 30 * 60 * 1000);
  const in60min = new Date(now.getTime() + 60 * 60 * 1000);

  const events = CalendarApp.getDefaultCalendar().getEvents(in30min, in60min);

  events.forEach(event => {
    if (event.getDescription() || event.getGuestList().length > 0) {
      const context = `
        Title: ${event.getTitle()}
        Time: ${event.getStartTime()}
        Duration: ${(event.getEndTime() - event.getStartTime()) / 60000} minutes
        Attendees: ${event.getGuestList().map(g => g.getEmail()).join(', ')}
        Description: ${event.getDescription() || 'None provided'}
      `;

      const brief = callGeminiAPI(
        'Generate a pre-meeting brief for this event using the Meeting Prep format.',
        getPersonaInstruction(),
        context
      );

      GmailApp.sendEmail(
        Session.getActiveUser().getEmail(),
        `Pre-Meeting Brief: ${event.getTitle()}`,
        brief
      );
    }
  });
}
```

---

## Integration Priority Guide

If you're starting fresh and want maximum impact with minimum setup time:

| Priority | Integration | Setup Time | Value |
|----------|-------------|------------|-------|
| 1 | Google Chat agent (DM) | 2–4 hours | Immediate drafting in persona voice |
| 2 | Calendar meeting prep (manual) | 30 min | No code — just a Chat workflow |
| 3 | Gmail compose (manual Chat workflow) | 0 min | Draft in Chat, copy to Gmail |
| 4 | Apps Script Gemini utility function | 1 hour | Foundation for all automations |
| 5 | Meeting brief auto-trigger | 2 hours | High daily value |
| 6 | Weekly comms review | 1 hour | Persona drift detection |
| 7 | Docs/Slides sidebar add-on | 4–8 hours | Useful for heavy writing workflows |
| 8 | Gmail add-on (compose sidebar) | 8–16 hours | Best Gmail experience, highest effort |

---

*Last verified: March 2026.*
