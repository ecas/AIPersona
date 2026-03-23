# Deploying a Persona as a Gemini Enterprise Agent

**Audience:** Google Workspace admins and developers who want to deploy a persona-driven agent available organization-wide — in Google Chat, Gmail, and other Workspace surfaces.
**Prerequisite:** Google Workspace Business Plus or Enterprise plan with Gemini for Workspace. Agentspace / Agent Builder access enabled by admin.
**Skill level:** Technical — requires familiarity with Google Cloud Console and Workspace Admin Console.

---

## Architecture Overview

A Gemini Enterprise Agent differs from a personal Gem in four critical ways:

| Dimension | Gem (Personal) | Enterprise Agent |
|-----------|---------------|-----------------|
| Availability | Personal only | Org-wide or team-scoped |
| Surfaces | Gemini App chat | Chat, Gmail, Docs, Meet, Agentspace |
| Grounding | Uploaded files | Google Drive, web, Workspace data |
| Actions | None (generation only) | Can call APIs, search Drive, create docs |
| Admin | Self-managed | Requires Workspace admin + Cloud console |

For persona deployment, the Enterprise Agent adds organizational reach and grounding — the persona can access the user's actual emails, Drive files, and calendar when drafting responses.

---

## Part 1: Agent Builder Setup

### 1.1 Prerequisites

- Google Cloud project linked to your Workspace org
- Vertex AI API enabled (`gcloud services enable aiplatform.googleapis.com`)
- Agent Builder API enabled (`gcloud services enable discoveryengine.googleapis.com`)
- IAM role: `roles/aiplatform.admin` or `roles/owner` on the project
- Workspace admin access (for the Chat/Gmail integration steps)

### 1.2 Create the Agent

1. Open [Google Cloud Console](https://console.cloud.google.com)
2. Navigate to **Vertex AI > Agent Builder** (or search "Agent Builder")
3. Click **Create Agent**
4. Select **Conversational Agent** (not Search Agent)
5. Fill in:
   - **Agent name:** `[PersonName]AI` — visible to end users in Chat
   - **Location:** Choose your data residency region (typically `us-central1` or `europe-west1`)
   - **Model:** `gemini-2.5-flash` for speed, `gemini-2.5-pro` for complex reasoning
6. Click **Create**

### 1.3 Configure the Custom Instruction (System Prompt)

In the agent editor, find the **Agent Instructions** field (also called system instructions or persona).

**Structure for Enterprise Agents** (use `templates/persona-output-gemini-agent.md`):

The instruction must accommodate tool use. Unlike a Gem, the agent can call tools — the system instruction must tell it when and how.

```
[IDENTITY]
Who the agent is and what it does.

[DATA ACCESS RULES]
Which grounding sources to use. When to search Drive. When to use web grounding.

[TOOL USE RULES]
When to use each configured tool. Priority order.

[PERSONA RULES]
Same as Gem: vocabulary, banned words, modes, self-check.

[RESPONSE CONSTRAINTS]
What the agent must never produce or commit to.
```

**Character limit:** Agent Builder system instructions support significantly more tokens than Gems — typically 16,000+ characters. Use the extra space for tool-use logic and grounding rules, not more persona content. The persona content should be identical to your Gem.

### 1.4 Add Reference Data Sources

In Agent Builder, navigate to **Data Stores** in the left panel.

**Option A: Upload reference files directly**
- Click **Create Data Store > Cloud Storage**
- Upload your persona reference files (same set as Gem: psychometric profile, vocabulary fingerprint, tone guide, etc.)
- This creates a RAG-capable knowledge base the agent queries during conversations

**Option B: Connect to Google Drive**
- Click **Create Data Store > Google Drive**
- Authorize access to specific Drive folders
- The agent can then search the persona owner's actual Drive content when grounding responses

**Recommended:** Use both. Reference files for persona behavior, Drive for real-world content grounding.

---

## Part 2: Tool and Action Definitions

Tools define what the agent can DO, beyond generating text. For persona agents, these are the most useful:

### 2.1 Google Drive Search Tool

Allows the agent to retrieve relevant documents before drafting a response.

**Use case:** User says "Draft a follow-up email based on our last proposal." Agent searches Drive for the proposal, reads it, drafts the follow-up in persona voice.

**Configuration:**
```yaml
tool_name: drive_search
description: "Searches the user's Google Drive for relevant documents. Use when drafting responses that should reference prior work, proposals, meeting notes, or project files."
trigger_conditions:
  - User mentions a specific document or project
  - User asks to "follow up on" something
  - User references "our last" anything
output: Relevant file content passed to persona for draft generation
```

In Agent Builder UI: Tools > Add Tool > Google Drive Search (pre-built connector).

### 2.2 Gmail Context Tool

Reads recent email threads to provide context before drafting replies.

**Configuration:**
```yaml
tool_name: gmail_thread_reader
description: "Retrieves the email thread the user is currently viewing or referencing. Use when asked to draft a reply or summarize an email conversation."
trigger_conditions:
  - User asks to "reply to this email"
  - User references "the email from [Name]"
  - Gmail compose trigger is active
output: Thread content (sender, subject, body) passed to persona
```

In Agent Builder UI: Tools > Add Tool > Gmail (pre-built connector) > configure read-only scope.

### 2.3 Calendar Tool

Retrieves upcoming meetings for meeting prep mode.

**Configuration:**
```yaml
tool_name: calendar_reader
description: "Retrieves upcoming calendar events. Use when asked to prepare for a meeting, build a pre-meeting brief, or check availability."
trigger_conditions:
  - User asks "prepare me for my next meeting"
  - User mentions a meeting by name or date
  - User asks about schedule or agenda
output: Event title, attendees, time, description
```

### 2.4 Web Grounding (Google Search)

For research and customer intelligence modes.

In Agent Builder: **Grounding > Enable Google Search Grounding**. This gives the agent real-time web access. Configure it to only activate when the persona's research mode is triggered.

---

## Part 3: Workspace Surface Integration

### 3.1 Add the Agent to Google Chat

**Step 1: Publish the agent**
In Agent Builder: **Integrations > Google Chat > Enable**

**Step 2: Configure in Workspace Admin Console**
1. Open [admin.google.com](https://admin.google.com)
2. Navigate to **Apps > Google Workspace > Google Chat > Manage Chat Apps**
3. Click **Add App**
4. Search for your agent by the name you configured in Agent Builder
5. Set visibility: **Specific organizational units or groups** (recommended for rollout) or **All users**
6. Click **Save**

**Step 3: User access**
Users can now find the agent by:
- Opening Google Chat > searching for `@[AgentName]` in the search bar
- Clicking Apps in the sidebar > finding the agent
- In a group conversation, mentioning `@[AgentName]`

**Interaction in Chat:**
- Direct message the agent for private persona-assisted drafts
- Mention in a group chat: `@PersonaName draft a reply to the client's question above`
- The agent will respond using the persona's voice, applying the correct mode based on the channel

### 3.2 Gmail Compose Trigger

This enables the persona to assist directly inside Gmail compose windows.

**Step 1:** In Agent Builder, enable **Gmail integration** under Integrations.

**Step 2:** In Workspace Admin Console:
- Apps > Add-ons > Manage third-party marketplace apps
- Alternatively: Apps > Google Workspace > Gmail > User settings > Smart Compose / Gemini in Gmail

**Note:** As of early 2026, Gmail-native Gemini integration uses Gemini's built-in persona (set in Workspace admin, not Agent Builder). Full custom agent triggering in compose requires the Gemini API approach (see Part 4). The Chat integration is more reliable for custom agents currently.

**Workaround for Gmail:** Users can open a Chat DM with the agent, request the draft there, copy it into Gmail. Not seamless but functional without custom development.

### 3.3 Agentspace (Google Workspace Hub)

If your org has Agentspace enabled:
1. In Agent Builder: Integrations > Agentspace > Publish
2. The agent appears in the Agentspace catalog at `workspace.google.com/agentspace`
3. Users can discover and invoke it alongside other org agents

Agentspace is the preferred enterprise surface — it provides a central directory of all agents and handles auth/access control uniformly.

---

## Part 4: Grounding with Google Workspace Data

Grounding is what separates an Enterprise Agent from a Gem: the agent can pull real data before generating.

### Grounding Configuration

In Agent Builder > **Grounding**:

```
Priority 1: Enterprise Data Store (your persona reference files)
Priority 2: Google Drive (user's documents, scoped to their auth)
Priority 3: Google Search (web grounding, for research modes)
```

**Set this in the system instruction:**
```
When drafting a response that references prior work:
1. First, search the user's Drive for relevant files.
2. If found, read and incorporate — cite the file name in your draft.
3. If not found in Drive, proceed with available context.
4. Never fabricate file content. If uncertain, state what you found and ask for clarification.
```

### Data Access Boundaries

Define these clearly in the system instruction to prevent over-retrieval:

```
GROUNDING BOUNDARIES:
- Access Drive files the user explicitly owns or has been shared.
- Do NOT access HR systems, payroll, or files outside the user's scope.
- Do NOT retain or summarize file content across separate sessions.
- Do NOT include confidential file content verbatim in responses visible to others.
```

---

## Part 5: Security and Data Access

### IAM Configuration

| Role | Grant To | Reason |
|------|----------|--------|
| `roles/aiplatform.user` | Agent service account | Allows agent to call Vertex AI |
| `roles/drive.readonly` | Agent service account | Drive grounding — read only |
| `roles/gmail.readonly` | Agent service account | Gmail context — read only |
| `roles/calendar.readonly` | Agent service account | Calendar tool — read only |

**Principle of least privilege:** Grant only read access. The persona agent generates drafts — it does not send emails, create files, or modify calendar events without explicit user action.

### Data Residency

In Agent Builder, set the region matching your Workspace data region. For EU customers using Google Workspace data residency, select `europe-west` regions. Validate with your DPA/data processing agreement.

### Audit Logging

Enable Cloud Audit Logs for Agent Builder:
```
gcloud logging sinks create persona-agent-audit \
  bigquery.googleapis.com/projects/[PROJECT_ID]/datasets/agent_audit \
  --log-filter='resource.type="aiplatform.googleapis.com/Endpoint"'
```

Log what you need: query inputs, which data stores were accessed, session IDs. Do NOT log full output if it may contain personal or confidential content.

### User Consent

If the agent accesses individual users' Drive or Gmail, ensure:
- Users are informed via IT communication that the agent has read access
- Access is scoped to their own data (OAuth user-scoped tokens, not service-account-wide access)
- Data processing complies with your Workspace DPA and applicable privacy law

---

## Deployment Checklist

**Before going live:**

- [ ] Agent system instruction reviewed for confidentiality leaks
- [ ] All behavioral constraints included (especially NEVER auto-send)
- [ ] Data store connected and indexed
- [ ] Tools configured with correct trigger conditions
- [ ] IAM grants verified — read-only for all data sources
- [ ] Test protocol completed (minimum 10 prompts across all modes)
- [ ] Chat integration published to pilot group (not all users)
- [ ] Audit logging enabled
- [ ] User communication sent (what the agent does, what it accesses)

**Pilot rollout:**
Deploy to 5–10 users first. Collect feedback for 1 week. Fix instruction issues before full org rollout. The most common issues are mode-detection failures and grounding over-retrieval.

---

## Troubleshooting

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| Agent responds generically, ignores persona | System instruction not loaded | Check instruction field in Agent Builder — may have been truncated |
| Agent fabricates Drive content | Grounding not configured | Verify Data Store is connected and indexed |
| Agent refuses all requests citing confidentiality | Constraints too aggressive | Narrow the constraint scope — "customer deal sizes" not "all business data" |
| Agent uses banned words | Vocabulary rules vague | Make ban list explicit and add to system instruction AND the vocabulary reference file |
| Chat integration not visible to users | Admin publish not completed | Admin Console > Chat Apps > verify visibility scope |
| Slow responses | Model is `gemini-2.5-pro` | Switch to `gemini-2.5-flash` for chat interactions; keep Pro for complex document work |

---

*Last verified: March 2026. Agent Builder UI evolves rapidly — core concepts (system instruction, data stores, tools, integrations) are stable.*
