# Website V3 - Slice 1 preview report

**Branch:** `cursor/website-v3-slice1-7348`  
**Base:** `d8ae75da24cff1f7c4ad947de7291425c5e38a82` (`d8ae75d`)  
**Live vs main (step A):** All seven pages matched `origin/main` at build start.

## Local preview

```bash
cd /agent/repos/janus-site && python3 -m http.server 8080
```

Screenshots: `reports/website-v3-slice1-preview/` (index, contact, roadmap, contact?sent=1 at 375 / 768 / 1280; product, company, technical samples).

## Automated checks (local)

| Check | Result |
|-------|--------|
| Primary nav/footer links (7 pages) | Pass |
| Blog links → `https://blog.dtm-system.com` | Pass |
| All `mailto:` = `jessebinstock@dtm-systems.com` | Pass |
| No `@dtm-system.com` email on pages | Pass |
| `private beta` in `*.html` | Pass (none) |
| `Toth` (wrong spelling) in `*.html` | Pass (none; `Thoth` on roadmap) |
| `Complicated` in `*.html` | Pass (none) |
| Contact copy / labels per V3 06-contact | Pass |
| Privacy note visible | Pass |
| Required fields have `required` attribute | Pass |
| `/contact.html?sent=1` thank-you copy | Pass |
| Form POST during preview | Not performed |

Empty required fields: browser-native `required` on name, email, message prevents submit without a network request (verified in markup).

## Flag for Jesse

1. **Publish** the sentence *"There are no outside users yet."* on company/about closings and technical early-access block? (Currently live on those pages per V3 early-access override.)
2. **Footer street address** unchanged: `1 Rideau Street, Suite 700 · Ottawa, ON`.

## Old → new (public sentences)

| Old | New | Tag |
|-----|-----|-----|
| Janus stops unapproved AI actions… before they hit production. | Janus OS is being built to turn expertise into repeatable AI-assisted workflows, with clear rules, checks and people retaining authority over decisions that matter. | new wording (needs Jesse) |
| Meta home/product regulated-industries pitch | Same hero sentence / product meta per plan 3.1 | new wording (needs Jesse) |
| Designed for Risk, Compliance… industries paragraph | *(removed)* | new wording (needs Jesse) |
| Dogfood / institutional memory line | DTM is using an early implementation of these ideas in the development and testing of Janus itself. | new wording (needs Jesse) |
| Every decision logged. Every override audited. | Defined boundaries and records… still being developed and tested. | new wording (needs Jesse) |
| Decision logged with full context | A record of the gate is a design goal, still being tested | new wording (needs Jesse) |
| Operator paths logged… | Allow, hold, and override… Recording them is still being developed and tested. | new wording (needs Jesse) |
| Company nothing executes / nothing forgotten | Employees delegate the task, not the decision… (V3 authority paragraph) | V3 verbatim |
| Company hero running today | DTM is building… early implementation on parts of its own build and test work. | new wording (needs Jesse) |
| About mission / regulated industry cards | Slice 1 replacements per plan 3.2 | new wording (needs Jesse) |
| Roadmap phases 1–4 | Text from `05-roadmap.md`; Thoth spelling; meta/hero per plan | V3 verbatim |
| Private beta / technical LLM names | Early access sentence; neutral LLM lines (decision 4) | new wording (needs Jesse) |
| Contact page | Copy from `06-contact.md`; `_subject` = DTM website message | V3 verbatim |
| Janus is the glass box | Janus is being built to make gates and review visible. | new wording (needs Jesse) |
| Every workflow arrives structured | Structured intake… design goal being tested | new wording (needs Jesse) |
| The decision is logged (home demo) | Recording the resolution is a design goal being tested | new wording (needs Jesse) |
| Nothing moves until the gate clears | Progression is designed to wait for gate clearance… | new wording (needs Jesse) |
| Each transition is logged / Janus enforces at runtime | Recording transitions… Janus is being built to apply them at runtime | new wording (needs Jesse) |
| Every path is logged (technical) | Recording operator paths is still being developed and tested | new wording (needs Jesse) |

## Claim → evidence (capabilities still described on site)

| Claim (summary) | Tag | Source |
|-----------------|-----|--------|
| Gate blocks workflow / human clears (demo narrative) | testing | Internal demo / Vimeo; not customer production evidence |
| Mission Control, card hierarchy, gates | testing | JanusV7 dogfood / `docs/JANUS_OS_LAYERS.md` |
| Institutional memory / STM-LTM / MC ≥ 8.5 | testing | `docs/dogfood/dtm_company_hygiene_spec.md`, tests |
| Assembly line six agents | testing | `AGENTS.md`, hygiene seed |
| Operator chat grounding (contracts query) | testing | `tests/operator/test_dtm_hygiene_chat_grounding.py` |
| Roadmap phases | planned | `plans/website-v3/05-roadmap.md` |
| Thoth ecosystem | planned | Roadmap phase 3 copy |
| Native SaaS connectors | planned | Roadmap / technical integration copy |
| Early access / no outside users | testing | Founder conversation; internal build only |

## Near-miss sweep (`*.html`, case-insensitive)

See PR body for per-hit **changed** vs **left: reason** list.