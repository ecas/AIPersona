# Communication Style Analysis Prompts

These prompts are designed to be **copy-pasted into your corporate AI tool** (Gemini Enterprise, Google AI Studio, or any internal deep research tool with access to your email/chat data).

**How to use:**
1. Open your corporate AI tool (Gemini in Google Workspace, Copilot in Microsoft 365, etc.)
2. Copy one prompt at a time
3. Run it against YOUR sent emails/chats
4. Save the output report
5. Feed the reports into Phase 2 of the AIPersona skill

**Privacy note:** These prompts are designed to extract STYLE PATTERNS, not content. The reports should contain zero confidential information — no names, project details, or business secrets. Always review reports before sharing outside your corporate environment.

---

## Prompt Set 1: Overall Communication Style Profile

**Scope:** All sent emails from the last 6 months

```
Analyze my sent emails from the last 6 months and create a Communication Style Profile. Extract ONLY style patterns — no names, project details, or confidential content.

Report the following:

1. SENTENCE STRUCTURE
- Average sentence length (word count)
- Do I use short punchy sentences or long compound ones?
- How often do I start sentences with "I" vs other words?
- Do I use fragments? How often?

2. PARAGRAPH PATTERNS
- Average paragraph length
- Do I use one-sentence paragraphs?
- How do I structure multi-paragraph emails? (context-first or conclusion-first?)

3. FORMALITY SPECTRUM
- Rate my average formality 1-10 (1=texting friend, 10=legal document)
- Does my formality change based on recipient seniority?
- How do I greet people? (list my top 5 greetings by frequency)
- How do I sign off? (list my top 5 sign-offs by frequency)

4. TONE MARKERS
- Am I generally direct or hedging?
- How often do I use softening language ("just", "maybe", "I think", "perhaps")?
- How often do I use strong/assertive language ("must", "need", "clearly", "absolutely")?
- What's my ratio of statements to questions?

5. UNIQUE PATTERNS
- What phrases do I repeat across emails? (list top 20 by frequency)
- What words do I NEVER use that most people do?
- Any unusual punctuation habits? (em dashes, ellipsis, exclamation marks)
- Do I use emoji? How often and which ones?

Output as a structured report. Replace all names with [Person A], [Person B], etc. Replace all project names with [Project]. Do not include any email content verbatim.
```

---

## Prompt Set 2: Communication with Leadership / Seniors

**Scope:** Sent emails to managers, directors, VPs (last 6 months)

```
Analyze ONLY my sent emails to people senior to me (managers, directors, VPs, executives) from the last 6 months. Extract style patterns only — no confidential content.

Report:

1. DEFERENCE PATTERNS
- Do I hedge more with senior people? How?
- Do I use different greeting/closing styles?
- How do I frame requests? (direct ask vs. suggestion vs. question)
- How do I deliver bad news to leadership?

2. INFORMATION STRUCTURE
- Do I lead with the conclusion or build up to it?
- How long are my emails to leadership vs. peers?
- Do I use bullet points more or less than usual?
- How many action items do I typically include?

3. TONE SHIFT
- How does my tone change compared to peer emails?
- Do I become more formal, more cautious, or stay the same?
- How do I push back on leadership requests? (specific language patterns)

4. POSITIONING LANGUAGE
- How do I present my expertise? (confident, humble, balanced?)
- How do I reference my own accomplishments? (directly or indirectly?)
- Do I credit team vs. self when reporting wins?

Replace all names with [Senior A], [Senior B]. No project names or content.
```

---

## Prompt Set 3: Communication with Peers / Colleagues

**Scope:** Sent emails and chat messages to same-level colleagues (last 6 months)

```
Analyze my sent emails and chat messages to peers and same-level colleagues from the last 6 months. Style patterns only.

Report:

1. INFORMAL LANGUAGE
- How casual am I with peers vs. seniors?
- Do I use slang, abbreviations, or shorthand?
- How often do I use humor? What type? (sarcasm, self-deprecation, jokes, wit)
- Do I use emoji/GIFs with peers?

2. COLLABORATION PATTERNS
- How do I ask for help? (direct, indirect, apologetic?)
- How do I offer help? (proactive, wait to be asked?)
- How do I give feedback to peers?
- How do I disagree with a peer's idea? (specific phrases)

3. CHAT vs EMAIL DIFFERENCES
- How does my style change between email and chat?
- Am I more concise in chat? More casual?
- Do I use different greeting/sign-off patterns?
- Chat message length patterns (one-liners vs paragraphs)

4. EMOTIONAL EXPRESSION
- How do I express excitement or agreement?
- How do I express frustration or disagreement?
- What words signal I'm stressed?
- What words signal I'm enthusiastic?

Replace all names with [Peer A], [Peer B]. No project/content details.
```

---

## Prompt Set 4: Communication with External Contacts / Clients

**Scope:** Sent emails to people outside the organization (last 6 months)

```
Analyze my sent emails to external contacts (clients, partners, vendors, prospects) from the last 6 months. Style patterns only.

Report:

1. PROFESSIONAL PERSONA
- How do I introduce myself? What do I emphasize?
- How do I establish credibility? (titles, experience, name-drops, none?)
- Am I more formal externally than internally? How much?
- How do I handle first contact vs. ongoing relationship?

2. PERSUASION PATTERNS
- How do I make recommendations? (data-first, story-first, authority-first?)
- How do I handle objections or pushback?
- What's my follow-up style? (frequency, tone, escalation)
- How do I close / ask for decisions?

3. CULTURAL ADAPTATION
- Do I change style based on the recipient's region/culture?
- Am I more direct with some cultures, more careful with others?
- Do I code-switch between languages? (if bilingual)

4. VALUE LANGUAGE
- What words do I use to describe value/benefits?
- How do I talk about pricing/cost? (avoid, address directly, frame as investment?)
- How do I differentiate from alternatives?

Replace all external names with [Client A], [Partner B]. No company names or deal details.
```

---

## Prompt Set 5: Communication in Specific Situations

**Scope:** Targeted search across all sent messages

```
Find and analyze my sent emails/messages in these SPECIFIC SITUATIONS from the last 12 months. Extract only my communication patterns.

SITUATION 1: Delivering bad news or saying no
- Find 5-10 emails where I declined something, delivered negative feedback, or shared a setback
- How do I open these messages?
- What buffer language do I use before the "no"?
- How do I close? Do I offer alternatives?

SITUATION 2: Responding to urgent requests
- Find 5-10 emails where I responded to urgent/time-sensitive requests
- How fast do I typically respond?
- Does my writing style change under pressure? (shorter? more direct? more errors?)
- Do I acknowledge urgency explicitly or just respond fast?

SITUATION 3: Explaining complex topics
- Find 5-10 emails where I explained a technical or complex concept
- Do I use analogies? What kind?
- Do I assume knowledge or explain from scratch?
- How do I structure the explanation? (top-down, bottom-up, example-first?)

SITUATION 4: Celebrating wins or giving praise
- Find 5-10 messages where I praised someone or announced a win
- What words do I use for praise?
- Do I celebrate publicly or privately?
- How specific is my praise? (generic "great job" vs. specific behavior)

SITUATION 5: Following up on unanswered messages
- Find 5-10 follow-up messages
- How long do I wait before following up?
- What's my tone? (gentle reminder, direct nudge, passive-aggressive?)
- How does my follow-up language change on the 2nd or 3rd attempt?

Replace all names and project details. Style patterns only.
```

---

## Prompt Set 6: Vocabulary Deep Dive

**Scope:** All sent communications (last 12 months)

```
Perform a deep vocabulary analysis on ALL my sent emails and chat messages from the last 12 months.

Report:

1. TOP 50 MOST USED WORDS (excluding common function words like "the", "is", "and")
   - List with approximate frequency

2. SIGNATURE PHRASES (multi-word expressions I repeat)
   - List the top 30 phrases I use repeatedly
   - Group by category: agreement, disagreement, transitions, recommendations, excitement

3. INTENSIFIERS I USE
   - Which intensifiers do I favor? (very, really, absolutely, definitely, extremely, etc.)
   - How often relative to total word count?

4. HEDGING LANGUAGE
   - Which hedges do I use? (maybe, perhaps, I think, might, could, sort of, kind of)
   - In what contexts? (only technical uncertainty, or also opinions?)

5. WORDS I NEVER USE
   - Identify common professional words that appear zero or near-zero times in my writing
   - These become my "banned words" for the AI persona

6. FILLER/TRANSITION WORDS
   - What transitions do I use between ideas?
   - Any verbal tics in my writing? (e.g., starting emails with "So,", overusing "Basically,")

7. TECHNICAL JARGON FREQUENCY
   - How often do I use industry/technical terms vs. plain language?
   - Do I explain jargon or assume the reader knows it?

8. EMOTIONAL VOCABULARY
   - What words do I use when positive? (list with frequency)
   - What words do I use when negative? (list with frequency)
   - What words do I use when neutral/analytical?

Output as a structured report suitable for building an AI vocabulary fingerprint. No verbatim email content.
```

---

## Prompt Set 7: Multilingual Communication Analysis

**Scope:** For bilingual/multilingual communicators

```
Analyze my sent emails and chats in ALL languages I use. Compare my communication style across languages.

Report:

1. LANGUAGE DISTRIBUTION
- What % of my messages are in each language?
- Does language choice correlate with recipient, topic, or formality?

2. CODE-SWITCHING PATTERNS
- Do I mix languages within a single message? How?
- Are certain terms always in one language? (e.g., technical terms always in English)
- Do I switch language mid-conversation? What triggers it?

3. FORMALITY DIFFERENCES
- Am I more formal in one language vs another?
- Does my humor translate? Or is humor only in one language?
- Which language am I more direct in?

4. VOCABULARY DIFFERENCES
- Signature phrases per language (top 10 each)
- Do I use different greeting/sign-off styles per language?
- Are my emails longer or shorter in different languages?

5. CULTURAL ADAPTATION
- Do I adapt my style for the cultural norms of each language?
- Am I more hierarchical/deferential in one language?

Replace all names. Extract patterns only, no content.
```

---

## How to Use These Reports

After running the prompts:

1. **Save each report** as a separate file (e.g., `report-overall-style.md`, `report-leadership-comms.md`)
2. **Review each report** — remove any accidentally included names, projects, or confidential details
3. **Feed into AIPersona Phase 2**: paste reports when the skill asks for communication style data
4. The skill will synthesize all reports into your Vocabulary Fingerprint and Communication Mode Templates

**Recommended minimum**: Run Prompt 1 (overall) + Prompt 6 (vocabulary). These two alone cover 80% of what the persona needs.

**For maximum fidelity**: Run all 7 prompts. This takes ~30 minutes of copy-pasting but produces a persona that genuinely sounds like you across all contexts.
