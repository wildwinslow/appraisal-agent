# Appraisal Agent — Spec v0.1

**What:** a program that takes a photo of a collectible → identifies it → researches
market values → emits an archive entry JSON (the merge-file schema) with **honest
confidence labels**.

**Why two heads:** it is the flagship feature of the multi-user archive project (v2)
AND Winslow's agentic-AI-engineer capstone. Same object, two careers.

**The wedge is honesty.** The AI-wave competitors auto-fill entries and sound
confident. This agent says what it knows, what it researched, and what it is
guessing — every claim labeled.

## Pipeline (the one Rook currently runs by hand)

1. **see** — read the photo. Multi-pass: first read, then *neutral re-reads*
   (never feed the model the answer you hope for — the notgeld lesson).
2. **identify** — category, date, mint, composition, country.
3. **research** — market comps + guides, each with source and date.
4. **label** — per-claim confidence (rules below).
5. **emit** — one merge-file JSON, importable straight into the archive app.

## Tools the agent may call (v1)

| tool | does |
|---|---|
| `see(image)` | vision-model pass over the photo |
| `search(query)` | web search |
| `fetch(url)` | read a page (comps, price guides, cert pages) |
| `emit(entry)` | output the final merge-file JSON |

No agent frameworks until M2 earns one. The loop is: act → observe → next action.

## Confidence rules (the product's soul)

- **Confirmed** — directly read or verified (photo transcript, cert page).
- **Researched** — sourced market data, source + date attached.
- **Partial** — photo-grade or incomplete evidence (grade from a photo = always Partial).
- **Unverified** — plausible but unsupported. MUST be labeled.
- A guess dressed as Confirmed is the only true failure mode. (Ask the coin that
  claimed to be Swedish.)

## Golden eval set (the suite that judges the agent)

1. **The Notgeld (1920)** — the trap coin. Four claimed nationalities, two vision
  hallucinations, glare, wear. Correct answer: Polish-language emergency money,
  Aachen mint. Source photos + post-mortem in the wiki.
2. **1883 Shield Nickel, NGC MS65** (cert 4735931-007) — graded tier, cert parsing,
   a two-guide price conflict ($613 vs $1,234 with a soft $520 realized comp).
   Correct answer is *the conflict, labeled* — not a fake single number.
3. Every future photo appraisal becomes test data. The set grows forever.

## Milestones (slow and steady — one slice per sitting)

- **M0 — bench open:** Python 3 runs somewhere he can edit and run code. *(first assignment)*
- **M1 — see:** one tool call: photo in → identification out.
- **M2 — the loop:** vision + search working together, error handling.
- **M3 — emit:** output schema + confidence labels enforced.
- **M4 — the judges:** eval harness runs the golden set and scores the agent.
- **M5 — the product:** `python appraise.py photo.jpg > entry.json` and the
  archive eats the file.

## Stack

Python 3. One LLM API with tool calling. No framework, no database, no
distractions. The archive app is already the frontend — this agent is the engine.

## Working agreement

Slow and steady. One slice per sitting, tests before polish, and every milestone
ends with something that *runs*. This repo is also the portfolio: commit like the
hiring manager is watching, because eventually one is.
