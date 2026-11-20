# Website V3 - Slice 1 preview report

**Branch:** `cursor/website-v3-slice1-7348`  
**Base:** `d8ae75da24cff1f7c4ad947de7291425c5e38a82` (`d8ae75d`)  
**Content reviewed:** `523a8a550f4ce972585665b5684fc8db217ce75e` (`523a8a5`)  
**Live vs main (step A):** All seven pages matched `origin/main` at build start.

This file and the PNGs beside it are internal review material. They are not part of the public site.

## Why this folder is not published

GitHub Pages for this repo is legacy branch deploy: source branch `main`, folder `/` (`build_type: legacy`). There is no site build of our own. An empty `.nojekyll` at the repo root makes the managed `pages-build-deployment` workflow skip Jekyll. The last run on this repo is Checkout, then Upload artifact, with no Jekyll step.

That upload step is `actions/upload-pages-artifact@v3`. Its archive command is `tar --exclude=.git --exclude=.github`. It does not apply Jekyll's underscore or dot-file skips, and it does not read a `_config.yml` `exclude` list. This live site returns HTTP 200 for `/.nojekyll`, which matches that: ordinary dotfiles are published, and `.github` is the directory the uploader drops.

So these locations would go public after merge:

- `reports/` (anything under the publishing root)
- an underscore directory such as `_reports/` (`.nojekyll` exists specifically so underscore paths are copied)
- a `_config.yml` exclude (Jekyll never runs, so the file is ignored)
- a generic dot directory such as `.reports/` (v3 still packs dotfiles other than `.git` and `.github`)

`/.github/website-v3-slice1-preview/` is the path this publisher actually omits. A local tar with the same two excludes leaves this directory out and still includes the HTML pages.

## Local preview

```bash
cd /agent/repos/janus-site && python3 -m http.server 8080
```

Screenshots below are full-page PNGs of that server at content `523a8a5`. Pixel width is the viewport width (375, 768, or 1280). `blog.html` did not change in slice 1 and is not shot.

## Screenshots

| Page | Width | Screenshot |
|------|------:|------------|
| index.html | 375 | [index_375.png](index_375.png) |
| index.html | 768 | [index_768.png](index_768.png) |
| index.html | 1280 | [index_1280.png](index_1280.png) |
| about.html | 375 | [about_375.png](about_375.png) |
| about.html | 768 | [about_768.png](about_768.png) |
| about.html | 1280 | [about_1280.png](about_1280.png) |
| company.html | 375 | [company_375.png](company_375.png) |
| company.html | 768 | [company_768.png](company_768.png) |
| company.html | 1280 | [company_1280.png](company_1280.png) |
| contact.html | 375 | [contact_375.png](contact_375.png) |
| contact.html | 768 | [contact_768.png](contact_768.png) |
| contact.html | 1280 | [contact_1280.png](contact_1280.png) |
| contact.html?sent=1 | 375 | [contact_sent1_375.png](contact_sent1_375.png) |
| contact.html?sent=1 | 768 | [contact_sent1_768.png](contact_sent1_768.png) |
| contact.html?sent=1 | 1280 | [contact_sent1_1280.png](contact_sent1_1280.png) |
| product.html | 375 | [product_375.png](product_375.png) |
| product.html | 768 | [product_768.png](product_768.png) |
| product.html | 1280 | [product_1280.png](product_1280.png) |
| roadmap.html | 375 | [roadmap_375.png](roadmap_375.png) |
| roadmap.html | 768 | [roadmap_768.png](roadmap_768.png) |
| roadmap.html | 1280 | [roadmap_1280.png](roadmap_1280.png) |
| technical.html | 375 | [technical_375.png](technical_375.png) |
| technical.html | 768 | [technical_768.png](technical_768.png) |
| technical.html | 1280 | [technical_1280.png](technical_1280.png) |

## Automated checks (local)

Checked the seven pages plus `blog.html` where noted. Contact address text is omitted from this file. The HTML still contains the single site mailto.

| Check | Result |
|-------|--------|
| Primary nav and footer link targets on the seven pages | Pass |
| Blog links are `https://blog.dtm-system.com` | Pass |
| Relative `href` and `src` targets exist on disk | Pass |
| One distinct `mailto:` domain, `dtm-systems.com` | Pass |
| No `@dtm-system.com` in `*.html` | Pass |
| `private beta` in `*.html` | Pass (none) |
| `Toth` as a standalone spelling in `*.html` | Pass (none; `Thoth` remains on roadmap) |
| `Complicated` in `*.html` | Pass (none) |
| Contact labels: Name (required), Email (required), Organization (optional), Your message (required) | Pass |
| Privacy note visible | Pass |
| `required` on name, email, and message; organization is not required | Pass |
| Empty required fields do not submit (`checkValidity` is false; the submit event does not fire) | Pass |
| `/contact.html?sent=1` shows "Message sent." and the thanks line; the form is hidden | Pass |
| Outside-users copy in `*.html` | Pass (none) |
| Header link DOM click reaches product, technical, company, roadmap, about, and contact, each with a nav and a footer | Pass |
| Form POST during preview | Not performed |

Rendering note, unchanged from base `d8ae75d`: the stylesheet rule `nav { position: fixed; top: 0; }` also matches `<nav class="footer-nav">`. The footer nav paints over the header, so the top bar in these screenshots is the footer link row, the header brand is covered, and that link row is not in the footer. A coordinate click on the header hits that overlay. The mobile menu button stays under it, so opening the hamburger did not reveal `.nav-links`.

## Exact changed lines

Line numbers are on `523a8a5` (the new side of `git diff -U0 d8ae75d`). Mailto text is replaced with `[contact address omitted]`. `style.css` is shared by every page, not a page of its own.

### index.html

Deleted, with no replacement line: the hero paragraph that began "Designed for Risk, Compliance, and IT leaders...".

```
6: <meta name="description" content="Janus OS is being built to turn expertise into repeatable AI-assisted workflows, with clear rules, checks and people retaining authority over decisions that matter.">
10: <meta property="og:description" content="Janus OS is being built to turn expertise into repeatable AI-assisted workflows, with clear rules, checks and people retaining authority over decisions that matter.">
15: <meta name="twitter:description" content="Janus OS is being built to turn expertise into repeatable AI-assisted workflows, with clear rules, checks and people retaining authority over decisions that matter.">
48: <p class="hero-plain">Janus OS is being built to turn expertise into repeatable AI-assisted workflows, with clear rules, checks and people retaining authority over decisions that matter.</p>
55: <span class="dogfood-eyebrow">Internal</span>
56: <p class="dogfood-text">DTM is using an early implementation of these ideas in the development and testing of Janus itself.</p>
83: <p class="demo-step-desc">Approve, hold, or allow with debt. Recording the resolution is a design goal being tested. The workflow continues to Completed.</p>
100: <p class="page-card-desc">A walkthrough of a gate holding a workflow and a person clearing it.</p>
107: <p class="page-card-desc">How Janus OS is being built to coordinate work, with checks and people retaining authority.</p>
149: <div class="footer-l">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 &middot; Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

### about.html

```
59: <p class="page-card-desc">DTM is building Janus OS so people can delegate work without giving up authority over the decisions that matter. Institutional memory is a capability being developed.</p>
65: <p class="page-card-desc">Janus is being built so checks and records can support human authority. It is not offered as a certified system for any regulated industry.</p>
95: <p class="closing-sub">Early access is a conversation with the founder.</p>
102: <div class="footer-l">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 &middot; Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

### company.html

```
48: <p class="hero-sub">We believe AI's long-term potential is real, and that potential is only as good as the structure built to govern it. DTM is building that architecture: Janus OS, using an early implementation on parts of its own build and test work.</p>
59: <p class="pull">DTM is using an early implementation of these ideas in the development and testing of Janus itself.</p>
74: <p>Employees delegate the task, not the decision. The aim is not human approval of every minor step. It is to assign authority deliberately: automate what is already permitted, stop when evidence is insufficient, and seek authorized human judgment when the situation calls for it. The review and escalation experience is still being developed and tested.</p>
111: <p class="closing-sub">Early access is a conversation with the founder.</p>
118: <div class="footer-l">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 &middot; Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

### contact.html

```
6: <meta name="description" content="Tell DTM what you are working on. No finished plan is required.">
10: <meta property="og:description" content="Tell DTM what you are working on. No finished plan is required.">
15: <meta name="twitter:description" content="Tell DTM what you are working on. No finished plan is required.">
47: <h1>Let&rsquo;s talk about what you&rsquo;re working on.</h1>
48: <p class="hero-sub">Interested in Janus OS, looking for help with a business problem, exploring consulting, or simply want to make contact? Tell us a little about what&rsquo;s on your mind. You don&rsquo;t need a finished plan to start a conversation.</p>
57: <label for="name">Name (required)</label>
61: <label for="email">Email (required)</label>
65: <label for="company">Organization (optional)</label>
69: <label for="message">Your message (required)</label>
73: <p class="form-note" style="margin-bottom:20px">Please avoid including sensitive personal information or confidential documents in your first message.</p>
75: <input type="hidden" name="_subject" value="DTM website message">
80: <button type="submit" class="btn-primary">Send message</button>
81: <p class="form-note">Prefer email? You can reach us directly at <a href="mailto:[contact address omitted]">[contact address omitted]</a>.</p>
87: <p class="contact-success-body">Thanks for reaching out. We&rsquo;ll get back to you. You can also write to <a href="mailto:[contact address omitted]">[contact address omitted]</a>.</p>
96: <div class="footer-l">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 &middot; Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

Line 74 is the blank line inserted with the privacy note.

### product.html

Deleted, with no replacement line: the same regulated-industries hero paragraph removed on the home page.

```
6: <meta name="description" content="Janus is being built as reusable infrastructure for delegating, coordinating and checking work, with people retaining authority over decisions that matter.">
10: <meta property="og:description" content="Janus is being built as reusable infrastructure for delegating, coordinating and checking work, with people retaining authority over decisions that matter.">
15: <meta name="twitter:description" content="Janus is being built as reusable infrastructure for delegating, coordinating and checking work, with people retaining authority over decisions that matter.">
198: <p class="demo-step-desc">Operator reviews grounded chat, then approves, holds, or allows with debt.</p>
223: <p class="demo-beat-text">The operator resolves - approve, hold, or allow with debt. Recording the resolution is a design goal being tested; the workflow moves to Completed.</p>
285: <p class="hero-plain">Janus OS is being built to turn expertise into repeatable AI-assisted workflows, with clear rules, checks and people retaining authority over decisions that matter.</p>
286: <p class="dogfood-text" style="margin-top:24px"><span class="dogfood-eyebrow" style="margin-right:14px">Internal</span>DTM is using an early implementation of these ideas in the development and testing of Janus itself.</p>
310: <p>Every AI system running on model judgment is a black box. <strong>Janus is being built to make gates and review visible.</strong> Your organization defines the trigger before deployment. When the condition is true, the gate fires on a fact - not an inference. Defined boundaries and records are how exceptions can be reviewed. The review and escalation experience is still being developed and tested.</p>
336: <div class="gate-line">A record of the gate is a design goal, still being tested</div>
357: <div class="gate-part-val">Allow, hold, and override are the paths the design aims for. Recording them is still being developed and tested.</div>
420: <div class="prep-val">Structured intake before execution is a design goal being tested - gates aim to fire on facts, not assumptions.</div>
447: <div class="footer-l mono">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 · Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

### roadmap.html

`roadmap.html` also no longer ends with a newline after `</html>` (line 159). The tag text is unchanged.

```
6: <meta name="description" content="Where Janus is heading. Direction we are pursuing, not guaranteed release dates.">
10: <meta property="og:description" content="Where Janus is heading. Direction we are pursuing, not guaranteed release dates.">
15: <meta name="twitter:description" content="Where Janus is heading. Direction we are pursuing, not guaranteed release dates.">
48: <p class="hero-sub">This roadmap shows the direction we are pursuing, not guaranteed release dates.</p>
54: <p class="body-text" style="max-width:720px;margin-bottom:44px">Janus OS is a long-term approach to helping people organize and direct AI-assisted work. This roadmap shows the direction we're pursuing, not guaranteed release dates or a promise that every idea will ship in its present form.</p>
60: <h2>Governed foundations</h2>
61: <p class="roadmap-lead">Operating and being tested internally.</p>
62: <p>Bounded tasks, defined checkpoints, review records and a development assembly line provide a first place to test whether work can move reliably while important decisions remain accountable. This is internal evidence, not production validation across customer environments.</p>
69: <h2>Arbiter and workflow creation</h2>
70: <p class="roadmap-lead">In active development.</p>
71: <p>The next step is making these controls usable by people who understand their work but don't write software: clarify the intent, shape the steps, and manage exceptions through a useful human experience.</p>
78: <h2>Coaching and learning from execution</h2>
79: <p class="roadmap-lead">Early-stage direction.</p>
80: <p>Compare what was intended with what actually happened. Use evidence from mistakes, holds and variation to identify where a process needs to be clarified or improved. A finished coaching product is not yet claimed.</p>
87: <h2>Thoth and broader intelligence</h2>
88: <p class="roadmap-lead">Future exploration.</p>
89: <p>We envision tools that can identify patterns across governed work, suggest changes and help people make better decisions without handing over final authority. Thoth is a proposed part of that wider ecosystem, not a currently available product.</p>
96: <h2>Connected organizational knowledge</h2>
97: <p class="roadmap-lead">Longer-term research direction.</p>
98: <p>We want an organization to learn from useful work it has already done: connect relevant context, retrieve the right lessons, and surface relationships that otherwise disappear between projects. The particular database, query language or memory architecture remains open to testing.</p>
103: <p class="body-text" style="max-width:720px;margin-top:44px">Better delegation should protect people's attention without surrendering their authority or understanding. We will keep testing the technology against that standard as Janus grows.</p>
109: <p class="closing-sub">Talk to us about the direction.</p>
116: <div class="footer-l">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 &middot; Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

### technical.html

```
57: <tr><th>LLM</th><td>Model-agnostic. Provider choice is environment-configurable, including a local fallback.</td></tr>
83: <tr><th>Audit</th><td>Traceability on transitions is a design goal being tested</td></tr>
158: <p class="big-serif serif">One governed step at a time.<br>Progression is designed to wait for gate clearance before the next step.</p>
164: <p class="body-text" style="max-width:720px;margin-top:0"><strong>Ritual sequence:</strong> intake → refine intent → plan → approve → apply → verify → publish. Recording transitions is a design goal being tested. Your organization defines required artifacts and danger points before deployment; Janus is being built to apply them at runtime.</p>
176: <p>When a condition is true, the gate fires. The workflow stops. The operator sees what triggered it and decides. Recording operator paths is still being developed and tested.</p>
245: <p>LLM provider is environment-configurable, including a local fallback. Native SaaS connectors are on the roadmap.</p>
253: <p class="sec-label">Early access</p>
254: <p class="big-serif serif" style="max-width:560px">Early access is a conversation with the founder.</p>
256: <a href="contact.html" class="tech-cta">Early access: talk to the founder</a>
265: <div class="footer-l">DTM - Dynamic Trajectory Memory Inc.<br><span>1 Rideau Street, Suite 700 &middot; Ottawa, ON</span><br><a href="mailto:[contact address omitted]">[contact address omitted]</a></div>
```

### style.css (shared)

```
1386: .footer-l a {
1387:   color: var(--accent);
1388:   text-decoration: underline;
1389:   text-underline-offset: 2px;
1390: }
```

## Flag for Jesse

1. Early-access closings on about, company, and technical now say early access is a conversation with the founder. The sentence previously flagged for a publish decision has been removed from those pages and is not in the HTML.
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
| Early access is a conversation with the founder | testing | Founder conversation; internal build only. The earlier early-access sentence is not in the HTML. |

## Near-miss sweep (`*.html`, case-insensitive)

See PR body for per-hit **changed** vs **left: reason** list.
