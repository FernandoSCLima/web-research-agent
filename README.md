# Web Research Agent

A Claude Code sub-agent for hotel operators. Cost-efficient web research — finding local businesses, suppliers, products, vendors, and competitive intel — and saving the results so you don't re-research the same thing twice.

Built on the **WAT framework** (Workflows, Agents, Tools): probabilistic AI handles reasoning, deterministic scripts handle execution.

---

## What it does

- **Finds local businesses** (plumbers, electricians, suppliers, contractors) — returns name, phone, email, address, rating
- **Researches products** — specs, pricing, availability, where to buy locally
- **Pulls competitor / market data** — pricing, offerings, reviews of comparable properties
- **Returns structured results** — tables or bullets with source URLs, ratings, and a clear recommendation
- **Caches its findings** — every research project is saved to per-agent memory so the next run hits the cache instead of re-searching

---

## What you need

- [Claude Code](https://claude.ai/code) installed
- Web access (the agent uses Claude Code's built-in `WebSearch` and `WebFetch` tools)
- Optional: an Anthropic API key if you're driving the agent outside a billed Claude Code session

---

## Quick start

1. **Clone the repo** into a Claude Code project:

   ```bash
   git clone https://github.com/th1-ai/web-research-agent.git
   cd web-research-agent
   ```

2. **Copy the env template** and fill it in (or skip it — the agent runs on Claude Code defaults if you have no API keys to wire up):

   ```bash
   cp .env.example .env
   ```

3. **Edit [`.claude/agents/web-research.md`](.claude/agents/web-research.md)** — replace the `{{HOTEL_*}}` placeholders described in [Customising](#customising) with your property's details.

4. **Open in Claude Code** from the project root:

   ```bash
   claude
   ```

5. **Invoke the agent**:

   > "Find me three vetted plumbers within 30 minutes of the property who handle commercial work."

   Claude Code will pick up the `web-research` sub-agent automatically because of the front-matter `description` line.

---

## File structure

```
web-research-agent/
├── README.md                                    # This file
├── .env.example                                 # Optional API keys
├── .gitignore
├── LICENSE
└── .claude/
    ├── agents/
    │   └── web-research.md                      # The sub-agent definition
    └── agent-memory/
        └── web-research/
            ├── MEMORY.md                        # Index of saved research projects
            └── example-feedback-pattern.md      # Example "memory note" — replace with your own
```

---

## Customising

The agent file uses placeholders so it can be adapted to any property anywhere in the world. Replace the following before first run:

| Placeholder | What to put | Example |
|---|---|---|
| `{{HOTEL_NAME}}` | Your property name | `The Olive Tree Inn` |
| `{{HOTEL_LOCATION}}` | Town / village | `<your town>` |
| `{{HOTEL_REGION}}` | Region or state | `<your region>` |
| `{{HOTEL_NEARBY_TOWNS}}` | Comma-separated list of nearby towns | `<town A>, <town B>, <town C>` |
| `{{HOTEL_LANGUAGE}}` | Local search language | `French`, `Spanish`, `English` |

You can do this with any text editor or with one `sed` pass:

```bash
sed -i '' \
  -e 's|{{HOTEL_NAME}}|The Olive Tree Inn|g' \
  -e 's|{{HOTEL_LOCATION}}|<your town>|g' \
  -e 's|{{HOTEL_REGION}}|<your region>|g' \
  -e 's|{{HOTEL_NEARBY_TOWNS}}|<nearby towns>|g' \
  -e 's|{{HOTEL_LANGUAGE}}|<local language>|g' \
  .claude/agents/web-research.md
```

---

## How memory works

The agent uses Claude Code's per-agent memory at `.claude/agent-memory/web-research/`. After each research session, it saves:

- **Project files** (`project_<slug>.md`) — one file per research topic, with the question, the answer, contact details, and source URLs. The next run reads these first so it doesn't re-search what's already known.
- **Feedback notes** — patterns that make future research faster or cheaper (e.g. "for local suppliers, one focused query in the local language beats three broad English ones").

[`MEMORY.md`](.claude/agent-memory/web-research/MEMORY.md) is the index — keep one line per project file, dated, so stale research is obvious. [`example-feedback-pattern.md`](.claude/agent-memory/web-research/example-feedback-pattern.md) shows the recommended format for a feedback note. Delete it once you have your own.

> **Heads-up on committing memory files:** project research often contains real phone numbers, prices, and email addresses for local businesses. The included `.gitignore` keeps `.env` out of git, but does not exclude `.claude/agent-memory/`. If you want to keep memory private, add `.claude/agent-memory/` to your local `.gitignore` or maintain a private fork.

---

## Related agents

This agent works well alongside:

- [`concierge-agent`](https://github.com/th1-ai/concierge-agent) — pulls in research findings when adding new restaurants, beach clubs, or providers to the vetted-contacts list
- [`marketing-report-agent`](https://github.com/th1-ai/marketing-report-agent) — uses competitive research to ground recommendations
- [`general-purpose-agent`](https://github.com/th1-ai/general-purpose-agent) — for open-ended research questions that need broader reasoning than `web-research` alone

---

## License

MIT — see [LICENSE](LICENSE).
