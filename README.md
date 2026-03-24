# AIPersona — Open-Source AI Persona Builder

Build a high-fidelity AI clone of yourself. Deploy it on Claude, Gemini, or ChatGPT. Anyone can do it.

---

## What This Is

AIPersona is an open-source methodology and toolkit for building an AI persona — a version of yourself that can write in your voice, think through your decision-making process, and communicate with your style.

It is not a software product. It is a **structured process** — a skill you run with any AI assistant to guide you through collecting the right data about yourself and turning it into deployable AI personas.

**It is designed to be used with**: Claude, Gemini, ChatGPT, or any capable LLM.

**The output is**: A set of system prompts and supporting documents ready to deploy as a Claude Project, Gemini Gem, or ChatGPT Custom GPT.

---

## Why This Exists

Most people who try to build an "AI version of themselves" end up with a generic chatbot that sounds like a press release. This happens because:

1. They skip the psychometric foundation (no real self-knowledge data)
2. They describe who they want to be, not who they are
3. They have no Anti-Persona (no explicit "not me" constraints)
4. They never give the AI real examples of their writing and speaking

This framework fixes all four problems with a structured, research-grounded process.

---

## What You Get

After completing the process, you will have:

- **Core Identity Document** — who you are, psychometrically grounded
- **Behavioral Fingerprint** — how you act in 10+ distinct situations
- **Vocabulary Fingerprint** — what you say and what you'd never say
- **Communication Mode Templates** — email, chat, presentation, meeting styles
- **Anti-Persona** — explicit constraints preventing persona drift
- **Decision-Making Model** — how you evaluate options and reach conclusions
- **Platform-Ready System Prompts** — for Claude, Gemini, and ChatGPT

---

## Research Foundation

This methodology is grounded in peer-reviewed AI persona research:

- **Stanford HAI (2024)**: 85% personality match achieved using 2-hour interview + psychometric data on 1,052 individuals
- **NN/g (2025)**: Rich prompt augmentation outperforms fine-tuning and RAG for persona fidelity
- **Twin-2K-500**: Interview-based twins achieve 80%+ accuracy vs. 55–71% for profile-only approaches
- **TwinVoice Framework**: 6-capability evaluation model used for validation
- **Nature Machine Intelligence (2025)**: Psychometric framework specifically recommended for LLM personality modeling

---

## How to Use

### Option 1: Install as a Claude Code Skill (fastest)

```bash
claude skill add --from https://github.com/ecas/AIPersona
```

Then run:
```
/ai-persona
```

Claude Code guides you through the entire process conversationally — psychometric tests, communication style analysis, interview protocol, and persona deployment. Everything happens inside your terminal.

### Option 2: Install as a Gemini CLI Skill

```bash
gemini skill add --from https://github.com/ecas/AIPersona
```

Then run:
```
/ai-persona
```

### Option 3: Manual setup (any AI assistant)

1. Open Claude, Gemini, or ChatGPT
2. Copy the contents of `SKILL.md` and paste as your system prompt or first message
3. The AI will guide you through the process conversationally

### Option 4: Self-guided (no AI)

1. Work through `SKILL.md` yourself, section by section
2. Use the templates in `/templates/` to collect and organize your data
3. Generate your system prompts manually using the output templates

### Option 5: Run with a partner

Have someone who knows you well act as interviewer for Phase 3. The interview sessions produce better results when you can't see the questions in advance.

---

## File Structure

```
AIPersona/
├── SKILL.md                              # Main guided process (start here)
├── README.md                             # This file
├── LICENSE                               # MIT License
├── templates/
│   ├── anonymization-prompt.md           # How to anonymize work data safely
│   ├── communication-analysis-prompts.md # 7 ready-to-paste prompts for corporate AI tools (Gemini/Copilot)
│   ├── style-extraction-template.md      # Writing style capture form
│   ├── interview-protocol.md             # All 37 interview questions (standalone)
│   ├── persona-output-gemini-gem.md      # Output template for Gemini Gems
│   ├── persona-output-gemini-agent.md    # Output template for Gemini Enterprise Agents
│   ├── persona-output-claude.md          # Output template for Claude Projects
│   └── persona-output-chatgpt.md         # Output template for ChatGPT Custom GPTs
└── guides/
    ├── gemini-gem-setup.md               # Step-by-step Gemini Gem deployment guide
    ├── gemini-enterprise-agent-setup.md  # Enterprise Agent (GCP Vertex AI Agent Builder)
    └── google-workspace-integration.md   # Integrating across Gmail, Docs, Sheets, etc.
```

---

## Time and Cost

| Budget | Time Investment | Cost | Quality |
|--------|----------------|------|---------|
| Free ($0) | 3–5 hours | $0 | ~80% of max fidelity |
| Budget ($50–100) | 4–6 hours | $50–100 | ~88% |
| Professional ($200–500) | 6–8 hours | $200–500 | ~95% |
| Premium ($500+) | 8–12 hours | $500–2000 | ~99% |

The biggest quality jump is from $0 to $50–100. After that, returns diminish.

---

## Privacy & Data Safety

**You never send personal data to this repository or to any third party.**

All data collection happens locally between you and your chosen AI platform. The anonymization templates in this repo exist specifically to help you strip identifying information before sharing any work communications.

See `templates/anonymization-prompt.md` for the safe data handling protocol.

**GDPR note**: Your own sent emails, personality test results, and LinkedIn profile are data you control. Work communications that include others' data must be anonymized before use. The methodology accounts for this throughout.

If you operate in a jurisdiction with data protection laws (EU GDPR, UK GDPR, PIPEDA, etc.), review what constitutes personal data before sharing psychometric results with any AI platform. Personality test results may constitute sensitive personal data under certain interpretations of GDPR Article 9.

---

## Contributing

Contributions are welcome. This is a methodology project, not a software project — improvements to the process, additional platform templates, and translated versions of the interview protocol are all valuable.

Please open an issue before submitting significant changes to `SKILL.md` to discuss the proposed direction.

---

## License

MIT — free to use, adapt, and distribute with attribution.

---

## Acknowledgments

Research sources that shaped this methodology:
- [Stanford HAI — AI Agents Simulate Personalities](https://hai.stanford.edu/news/ai-agents-simulate-1052-individuals-personalities-with-impressive-accuracy)
- [NN/g — Digital Twin Research](https://www.nngroup.com/)
- [Nature Machine Intelligence — Psychometric Framework for LLMs](https://www.nature.com/articles/s42256-025-01115-6)
- [VIA Character Strengths](https://www.viacharacter.org/)
- [IPIP Big Five](https://ipip.ori.org/)
- [TwinVoice / Twin-2K-500 Dataset](https://huggingface.co/datasets/twin2k-500)
