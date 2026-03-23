# Data Anonymization Template

**Purpose**: Safely strip identifying information from work communications before using them for persona style analysis.

**Use this when**: You have access to sent emails, chat messages, or documents from work and want to use them for communication style extraction without sharing confidential or personally identifiable information.

---

## Why Anonymization Matters

Work communications typically contain:
- Names of colleagues and clients (their personal data)
- Company names (possibly confidential)
- Deal values, financials (definitely confidential)
- Project names (internal IP)
- Technical system names (security risk)

You only need the **STYLE** of the communication — not the content. Anonymization lets you extract style patterns while protecting everyone involved.

**Legal note**: Under GDPR and similar regulations, only fully anonymized data falls outside personal data protection requirements. Pseudonymized data (where re-identification is theoretically possible) remains regulated. When in doubt, anonymize more aggressively, not less.

---

## Anonymization Rules

Replace every instance of the following with the corresponding placeholder:

| Original Type | Replace With | Example |
|---------------|-------------|---------|
| Person's first and last name | [Person A], [Person B], etc. | "Hi Sarah" → "Hi [Person A]" |
| Your own name | [Me] | "Best, John" → "Best, [Me]" |
| Company name (yours) | [Company] | "At Acme Corp" → "At [Company]" |
| Client company name | [Client] | "The team at Globex" → "The team at [Client]" |
| Partner company name | [Partner] | |
| Competitor company name | [Competitor] | |
| Project name | [Project] | "Project Phoenix" → "[Project]" |
| Product name (yours) | [Product] | |
| Internal tool / system name | [System] | "In Salesforce" → "In [System]" |
| Specific dollar amounts | [Amount] | "$450,000" → "[Amount]" |
| Percentages that could identify a deal | [Percentage] | |
| Specific dates | [Date] | "March 15" → "[Date]" |
| Locations / offices | [Location] | |
| Job titles of others | [Role] | "our CTO" → "our [Role]" |
| Department names | [Department] | |
| Internal code names or initiatives | [Initiative] | |

---

## Anonymization Prompt for AI-Assisted Stripping

If you have a large volume of text, use this prompt with any AI assistant:

```
Please anonymize the following text for me. Replace:
- All people's names (first, last, or full) → [Person A], [Person B], etc.
  (assign consistent letters — if "Sarah" appears 3 times, always use [Person A])
- My own name wherever it appears → [Me]
- All company names → [Company], [Client], [Partner], or [Competitor]
  as appropriate
- All project names → [Project A], [Project B], etc.
- All internal tool or system names → [System]
- All specific dollar amounts → [Amount]
- All specific dates → [Date]
- All specific locations → [Location]
- All job titles of other people → [Role]
- All product names → [Product]

Do NOT change:
- Generic words like "email," "meeting," "team," "project"
- Industry terminology that is not specific to one company
- My own writing style, sentence structure, and vocabulary

Return only the anonymized text — no commentary.

Text to anonymize:
[paste your text here]
```

---

## What to Keep (Style Signal Data)

After anonymization, these elements should remain intact and are exactly what you want:

- Greeting style ("Hey [Person A]," vs. "Dear [Person A]," vs. "Hi,")
- Opening sentence structure
- How you transition between topics
- How you phrase requests vs. statements
- Your sign-off style
- Punctuation habits
- Sentence length patterns
- Bullet point vs. prose preference
- Tone words and intensifiers
- Hedging language vs. direct statements
- How you express disagreement, excitement, uncertainty
- Humor style and timing

---

## What NOT to Include at All

Some data is better left out entirely rather than anonymized:

- Meeting transcripts where others did not consent to AI analysis
- Messages from others (you only need YOUR sent messages)
- HR communications (even anonymized, not appropriate)
- Legal / compliance communications
- Anything marked confidential or under NDA
- Any message you would be uncomfortable with a neutral third party reading

**Rule of thumb**: If you would be uncomfortable if your employer saw it being analyzed, leave it out.

---

## Sample Anonymization

### Before Anonymization

```
Hi Sarah,

Following up on our call last week. I spoke with Marcus and the Globex
team and they're interested in expanding the Titan contract from $180K
to $280K by Q2. I've looped in Amanda from Legal to review the
paperwork by March 31.

The main risk is the competing bid from Apex Solutions — they're
reportedly offering a 15% discount. I think we should lead with the
Project Aurora case study to differentiate.

Best,
John
```

### After Anonymization

```
Hi [Person A],

Following up on our call last week. I spoke with [Person B] and the
[Client] team and they're interested in expanding the [Product] contract
from [Amount] to [Amount] by [Date]. I've looped in [Person C] from
[Department] to review the paperwork by [Date].

The main risk is the competing bid from [Competitor] — they're
reportedly offering [Percentage] discount. I think we should lead with
the [Project] case study to differentiate.

Best,
[Me]
```

Notice what is preserved: the greeting style, the paragraph structure, the way the person flags risk, the closing, the directness of the communication.

---

## Verification Checklist

Before sharing anonymized text, confirm:

- [ ] No real person's name appears anywhere
- [ ] No company names (yours or clients) appear
- [ ] No specific dollar amounts or financial figures
- [ ] No specific dates that could identify a deal or event
- [ ] No internal project or product names
- [ ] No internal system or tool names
- [ ] No job titles or department names that could identify individuals
- [ ] The anonymized text still sounds authentically like you
- [ ] You would be comfortable with a neutral colleague reading this

---

## Minimal Safe Dataset

If you are uncertain about any message, err on the side of exclusion. You only need:

- **10–20 sent emails** covering different tones (request, update, good news, bad news, pushback)
- **20–30 sent chat/Slack messages**
- Optionally: blog posts, LinkedIn posts, or other public writing you've authored

This is sufficient to extract a strong style profile. You do not need hundreds of messages.
