# Curriculum v1.5 Architecture Plan: Agentic-Era Core

*Planning document, not published by Docusaurus. Drafted 2026-10-27. Source proposal: [Fidgetech Curriculum: Preparing Graduates for the Agentic Era (Leadership Draft)](https://claude.ai/artifact/LxkY3CLastaMYXia4m2Xr3).*

**Bottom line:** most of v1.4 stays. The volume problem is in about five places: Intro 1.2, the Webpack toolchain, React NoSQL, the open-ended capstone, and the CS and C#/.NET reference material. Each of those has a clear trim or move. The real gaps are pull requests in Phase 1, component testing, SQL, security review, an AI policy, and any written internship content. These gaps are where new authoring goes.

Core path today (v1.4): **Intro to Programming → Intermediate JavaScript → React → Capstone → Fidgetech Studio Internship**

---

## 1. Evidence: what the docs audit found

The audit covered every md file in `docs/` (863 in total). Word counts come from `wc -w`. They are a proxy for time, not a measurement of it.

| Course (v1.4) | Lessons | Words | Headline finding |
|---|---|---|---|
| Pre-work | 30 | 26.9k | No AI policy. Academic Honesty allows "Stack overflow or other online code resources" and never mentions AI. A retired extension (Live Share Audio) appears in 3 lessons. The capstone page is a 57-word stub. |
| Intro 1.1 Git, HTML & CSS | 52 | 45.8k | **No lesson in Pre-work or Intro teaches pull requests.** A grep for "pull request" finds 0 files. Floats are taught where flexbox/grid should be. There are 7 Bootstrap lessons with mixed 4.5 and 5.2 versions. No IP has an accessibility objective. |
| Intro 1.2 JS & Web Browsers | 73 | 97.3k | This is the largest module and about half of Intro. The debugging sequence is strong. About 12k words are Web-API and interface theory. Events are spread over 4 lessons. |
| Intro 1.3 Arrays & Looping | 46 | 55.0k | TDD is pseudocode only, with no test runner. A LinkedIn lesson sits inside the JS module. There are 5 links to `old.learnhowtoprogram.com`. |
| Intermediate JavaScript | 120 | 143.2k | **13 lessons are mainly about Webpack (about 15.9k words), plus about 8 tooling lessons.** Versions are pinned to Webpack 4 and Jest 24 (2020-era). Vite is never used. XHR comes before fetch, and prototypes before classes. 13 Async files depend on Webpack. |
| React | 166 | 158.9k | Already modern: Vite, TypeScript, function components, no CRA or Redux. **There is no component testing.** The only test lesson tests a reducer. NoSQL is the largest module at 43.6k words. The team-week hosting lesson uses a Webpack build script, which is wrong for Vite apps. |
| Capstone | 29 | 30.9k | The proposal template has 6 fields: name, purpose, MVP, tools, stretch, notes. It has no users, acceptance criteria, data model, security, or deploy field. The rule is "at least 120 hours", with no scope freeze, no midpoint, and no code-quality rubric. |
| Fidgetech Studio Internship | — | — | **Not in the docs.** `marketing-yourself/1_internship-process/internship-overview.md` describes Epicodus: Portland/Seattle campuses, "Epicenter", "Week 18". |

Core total today: **about 558k words** across 516 lessons.

### Cross-cutting findings
- **309 files** in `docs/` mention Epicodus. Most are front-matter `url:` source links, but some are in code samples (`enrolledInEpicodus`) and DEI homework links (`epicenter.epicodus.com`).
- `docs/0_pre-work/0_getting-started-at-fidgetech/1-0-0-01-learn-how-to-program.md` (lines 29 and 33) says "we offer one track at Fidgetech: C#/.NET" and "After JavaScript, we will be doing a course on C#/.NET". Both contradict `docs/courses.md`, which lists C#/.NET as Reference.
- Core team weeks link into `c-and-net`. `2-4-0-08` and `3-6-0-08` cover the C#/MySQL install. `3-6-0-03` and `3-6-0-09` link to the git-collaborators lesson.
- The Intermediate JS and React team weeks are near-duplicates. Lessons 02, 05, and 06 are byte-identical.
- `docusaurus.config.js` sets no `lastVersion`, so Docusaurus treats `v1_2` as the latest version. `customFields.latestVersion` only affects what is displayed.
- AI appears only as a study aid in the capstone self-guided check-ins and a few marketing lessons. None of these is a policy or a disclosure rule.

### Draft claims checked against the repo

| Draft claim | Audit result |
|---|---|
| Webpack is "about 20 lessons" and should shrink | ⚠️ 13 lessons are mainly about Webpack, and about 22 if all toolchain lessons count. Shrinking is still right. |
| TDD with Jest is the base for "tests as the contract" | ⚠️ True in Intermediate JS (red-green commits, 100% coverage). Intro TDD is pseudocode, and **React has no component tests**. |
| TypeScript can be reframed as data contracts | ✅ The Book Tracker domain already "carries forward into later sections". |
| Firebase detail can shrink to make room for SQL | ✅ About 18 Firebase/NoSQL lessons. CAP/BASE, data structuring, and queries can go. |
| Move most of CS, keep Big-O and core structures | ✅ Of 49 lessons, about 14 are core. |
| SQL left the core with C#/.NET | ✅ 4–6 C#-neutral relational lessons can be extracted. |
| Open question: is PR work taught early enough in Phase 1? | ❌ It is not taught in Phase 1 at all. PRs first appear in `2-3-3-03` and the team weeks. |
| Housekeeping: C#/.NET named as track; Epicodus links | ✅ Confirmed, and wider than reported (309 files). |
| Path ends in the Fidgetech Studio internship | ⚠️ No curriculum page exists for it. |

---

## 2. Target architecture: v1.5 core (about 1,400 hours)

The four AI phases follow course order, so no new schedule is needed. Each gate is an added objective on an independent project (IP) review that already runs.

| AI phase | v1.4 material it covers | AI rule | Gate (added to existing IP review) | Company project |
|---|---|---|---|---|
| **1. Hand tools** | Pre-work + Intro 1.1 | Off | 1.1 IP: work submitted as a PR with a Decision Record; automated a11y check passes | Company A: donation page per school |
| **2. Hand tools, sharpened** | Intro 1.2–1.3 + Intermediate JS | Off. A read-only tutor unlocks in Async after the 2.2 gate | 2.2 IP: red-green commits; 2.3 IP: explain-it-back on an API flow | A: donation form and running total |
| **3. Power tools, supervised** | React: FP primer, TypeScript, Fundamentals, trimmed NoSQL/APIs | Agent as pair; student writes the plan first | TS unit: tests written as the contract; Fundamentals IP: find a planted defect in an agent-written PR | A: school admin dashboard |
| **4. Power tools, directed** | **New** Specs, ERD, SQL, Security, Deploy unit + rebuilt Capstone | Agent as implementer; student specifies, reviews, and verifies | Capstone defense: SPEC.md, security and scale review | A acquires B (inherited codebase, multi-tenant leak) |
| **Internship** | Fidgetech Studio | Client data rules apply | — | Real client work |

**Advanced stage (optional, after the core):**
- C#/.NET (220 lessons)
- About 35 CS lessons: BSTs, heaps, tries, graphs, sorts, bits, whiteboarding 1–7
- React further-exploration lessons
- Bootstrap depth
- JS Web-API theory
- Prototypes and regex
- Webpack, kept as "how bundlers work"
- Business skills: scoping, pricing, contracts, support

---

## 3. Module decisions: what stays and what goes

### Keep
| Module / lessons | Why it stays |
|---|---|
| Pre-work: learn how to learn, growth mindset, pair programming, asking for help | The foundation the draft protects |
| 1.1 HTML/CSS core, Git basics, branching and merging (`1-1-2-07`, `1-1-2-08`) | AI-off mental models; prerequisite for PR-per-task |
| 1.2 JS fundamentals and the full debugging sequence (`1-2-0-04/05`, `1-2-1-08`, `1-2-3-03`, `1-2-4-06/07`) | Debugging is the core verification skill |
| 1.3 arrays, loops, separation of concerns, Text Analyzer | Maps onto Decision Records and review |
| 2.2 Jest red-green-refactor rules, ES6 classes and modules | Already "tests as the contract" |
| React 2 TypeScript (all 29 lessons) | Already shaped like data contracts |
| React 3 Fundamentals (Vite, components, state, CRUD, planning) | Planning lessons support "student writes the plan first" |
| CS core, about 14 lessons (Big-O, memory, linked lists, stacks/queues, hash tables, trees, binary search, recursion) | Matches the draft; `2-1-0-10` links to hash tables |
| Marketing: portfolio, resume, interviews | Job-search core, once de-duplicated |

### Trim
| Module | Cut | Est. words saved |
|---|---|---|
| Pre-work remote tools | Discord (3 lessons) and Live Share (2,399 words) cut to about a third; replace the PAT lesson with `gh auth` | about 5k |
| 1.1 Bootstrap and layout | 7 Bootstrap lessons cut to 1–2; teach flexbox/grid instead of floats | about 4k |
| 1.2 JS & Web Browsers | About 30%: Web-API theory, global object, interfaces and inheritance; merge the 4 event lessons | about 29k |
| 1.3 Arrays & Looping | Prototypes, static vs instance, and regex move to advanced; LinkedIn moves to the career track | about 12k |
| 2.1 OO JavaScript | Lead with classes; reduce prototype depth | about 10k |
| React 1 Functional Programming | Compress to a pre-React primer | about 10k |
| React 4 NoSQL | Cut CAP/BASE, data structuring, queries, and the wait-time feature; move 5 whiteboard and 3 career lessons out | about 20k |
| React 5 APIs | Move the 6 further-exploration lessons to advanced | about 9k |
| Capstone team week | Merge into one shared team week | about 8k |

### Replace
| Today | Replacement | Files |
|---|---|---|
| Webpack 4 toolchain (13 lessons) | Vite or native ES modules (about 3 lessons) | `2_intermediate-javascript/2_test-driven-development-and-environments-with-javascript/2-2-0-08…2-2-0-20`, `2-2-1-01`, `2-2-3-02` |
| API keys via `dotenv-webpack` | Vite env, plus "client keys aren't secret" and server-side proxies | `2_intermediate-javascript/3_asynchrony-and-apis/2-3-0-08-protecting-api-keys.md` |
| XHR before fetch | fetch and async/await first | `2_intermediate-javascript/3_asynchrony-and-apis/` |
| Two Webpack gh-pages hosting lessons | One Vite deploy lesson | `2_intermediate-javascript/4_team-week/2-4-0-07-…`, `3_react/6_team-week/3-6-0-07-…` |
| Pseudocode TDD in Intro | A real test runner (Vitest) | `1_introduction-to-programming/3_arrays-looping/1-3-2-02-…` |

### Move to the advanced stage
| Module | Size |
|---|---|
| C#/.NET 5.0–5.7 (and remove the core links into it) | 220 lessons, 214k words |
| CS advanced and whiteboarding | About 35 lessons, about 58k words |
| React further exploration (Native, SEO, custom environment, data viz, animation) | 6 lessons |
| Bootstrap depth, JS Web-API theory, prototypes, regex | From the trims above |

### Reframe
| Module | New framing | File |
|---|---|---|
| Academic Honesty | Phase-based AI policy and a disclosure rule | `0_pre-work/0_getting-started-at-fidgetech/1-0-0-10-academic-honesty.md` |
| 2.3 Async & APIs | Home of the read-only AI tutor | `2_intermediate-javascript/3_asynchrony-and-apis/` |
| Team weeks | One shared, Vite-correct module running the PR-per-task cycle | `2_intermediate-javascript/4_team-week/`, `3_react/6_team-week/`, `capstone/4_team-week/` |
| Capstone | Time-boxed client product; grow `capstone-proposal.md` into a SPEC.md | `capstone/2_capstone-planning/capstones---sign-ups-and-proposal.md`, `capstones---timeline-deadlines-and-homework.md` |
| Internship overview | Rewrite for Fidgetech Studio | `marketing-yourself/1_internship-process/internship-overview.md` |
| Welcome page | Remove the C#/.NET "one track" lines | `0_pre-work/0_getting-started-at-fidgetech/1-0-0-01-learn-how-to-program.md` |

### Add
| New content | Where | Seed material |
|---|---|---|
| Pull requests and Decision Record template | Intro 1.1, week 2, beside branching | `1-1-2-07`, team-week PR lessons |
| Accessibility checklist in every IP rubric | All IPs | `1-2-4-02` a11y paragraph |
| AI-assisted TDD and review unit (Vitest and React Testing Library) | After React 2 TypeScript | Book Tracker domain |
| Phase 4 unit: specs and acceptance tests, ERD and TS data contracts, SQL, security review, deploy/CI/logging | Between React and Capstone | `c-and-net/3_database-basics/3-3-1-02`, `3-3-1-03`, `3-3-3-02`, `3-3-0-03`, `3-3-0-15`; `c-and-net/4_many-to-many-relationships/3-4-0-02` |
| Company-project briefs, 4 phases × 2–3 variants | End of each phase | Fidgetech Studio Giving Portal (fictional data only) |
| Internship entry and Advanced row | `docs/courses.md` | — |

---

## 4. Strengths and weaknesses

| | Strengths | Weaknesses |
|---|---|---|
| **Current v1.4** | Deep debugging sequence. TDD in every JS course. IP code reviews exist and can carry gates. React is already on Vite and TS. TypeScript fits data contracts. 94–98% attendance. | About 558k words with no time box. 2020-era toolchain in Intermediate JS. No PRs in Phase 1. No component tests. No SQL, security review, or AI policy. Open-ended capstone (49% of Student A's hours). Internship undocumented. 309 Epicodus remnants. |
| **Draft proposal** | Gates reuse existing reviews, so no new schedule. Verification-first order (hand tools, then power tools). Cuts move material instead of deleting it. Predictable structure suits autistic learners. | Compression may cost depth. The AI speedup is unproven. Two-stage funding is uncertain (HECC/VR). Instructor time is unestimated (about 48 h per instructor per year for defenses, before reworks). Company projects are net-new authoring. Tool churn. |

---

## 5. Pros and cons of each change

| Change | Pros | Cons / risk | Mitigation |
|---|---|---|---|
| Webpack → Vite | Saves about 12 lessons; same tool as React; current versions | 13 Async files and 2 FP lessons must change together; rubrics need rewriting | Ship as one wave; keep Webpack in advanced as "how bundlers work" |
| Trim Intro 1.2 by about 30% | Biggest saving in the AI-off phase | Weaker mental models if the wrong lessons go | Keep every debugging lesson; run before-and-after debugging exercises |
| PRs and Decision Records in Phase 1 | Unlocks the PR-per-task and gate cycle; written-first lowers verbal load | Extra load in the first weeks | Same short template every phase; replacing the PAT lesson frees room |
| AI policy in Academic Honesty | Closes a real loophole; costs no hours | Needs accommodation rules | Exceptions go through an instructor, per the draft |
| Read-only AI tutor in Async | Low risk; practices AI literacy early | Students may over-rely on it | Tutor explains, never writes; explain-it-back gate |
| Vitest and RTL unit after TypeScript | Fills the real testing gap; makes "tests as the contract" concrete | New content and tooling setup | Reuse the Book Tracker domain |
| Shrink Firebase, add SQL | Relational skills employers expect; feeds Phase 4 | Two data models to teach; database choice is open | Extract the C#-neutral SQL lessons; pick the database (e.g. Postgres or SQLite) first |
| Phase 4 spec/security/deploy unit | The heart of product ownership; builds on the proposal template | About 25k words of new content; instructor authoring time | Build it in the pilot as draft item 4 |
| Time-boxed capstone and rubric | Removes the largest open-ended time sink | Some students won't finish inside the box | Scope freeze, midpoint check, rejoin-next-cohort rule |
| CS and C#/.NET → advanced | Large saving; C#/.NET is already Reference | Interview-prep depth leaves the core | Keep the 14 core CS lessons; advanced stays open |
| Company projects every phase | Practices inherited code and transfer | Heaviest authoring load; needs variants | Start with Phase 4 (A acquires B), add earlier phases later |
| Merge the three team weeks | Removes duplication; fixes the Vite deploy bug | Sidebar and link churn | Keep old slugs working with redirects |
| Document the internship | Completes the path; removes Epicodus text | Needs Fidgetech Studio input | Instructors draft from current practice |

---

## 6. Roadmap

| Wave | Scope | Gate to start | Filing likely? |
|---|---|---|---|
| **0. Housekeeping** | Epicodus front-matter links; C#/.NET track lines; Live Share Audio; Vite deploy lesson; team-week merge; remove core → c-and-net links; set `lastVersion`; fill or remove `student-handbook.md` and the capstone stub | None | Unlikely, but confirm |
| **1. Approvals and data** | HECC/VR filing questions; time-per-lesson data for more graduates; instructors tag lessons Core or Advanced; set the AI tool budget | President and Director of Operations | — |
| **2. Pilot** | The draft's 5 items (AI policy, Async tutor, AI-assisted TDD unit with Vitest/RTL, spec/ERD/SQL mini-unit, capstone SPEC.md + time box + rubric), plus Phase 1 PR and Decision Record lessons as a prerequisite | Filing approved | Yes |
| **3. Core restructure (cut v1.5)** | Webpack → Vite; Intro, Intermediate JS, and React trims; full Phase 4 unit; gate objectives in IP rubrics; Phase 3–4 company projects | Pilot metrics reviewed | Yes |
| **4. Advanced stage** | Move C#/.NET, CS, and further-exploration material under an Advanced row; Phase 1–2 company projects and variants; review placement data | v1.5 core running | Depends on two-stage funding |

**Size direction (estimate):**

| Course | Words today | Words after v1.5 |
|---|---|---|
| Pre-work | 26.9k | about 23k |
| Intro | 198.1k | about 158k |
| Intermediate JS | 143.2k | about 121k |
| React | 158.9k | about 128k |
| New Phase 4 unit | — | about 25k |
| Capstone | 30.9k | about 26k |
| Merged team weeks | — | −11k |
| **Total** | **about 558k** | **about 470k (−16%)** |

Word cuts alone will not reach 1,400 hours. Word count tracks reading time, not project time. The capstone is only 6% of the words but took 49% of Student A's hours. Most hour savings have to come from the capstone time box, pacing checkpoints, and Core/Advanced tagging.

Instructors set final hour budgets from time-per-lesson data. The draft's illustrative budget is Intro 197 h, Intermediate JS 225 h, React 540 h, Capstone 439 h.

### Repo mechanics for Wave 3
- Cut v1.4 as a frozen version with `npm run docusaurus docs:version v1_4`. Live `docs/` then becomes v1.5.
- Set `lastVersion` in `docusaurus.config.js` so the current docs are treated as latest.
- Move advanced material into its own sidebar in `sidebars.json`. Keep slugs stable so links from core lessons (e.g. `2-1-0-10` → hash tables) still work.

---

## 7. Open decisions
1. Per-student budget for AI coding tools.
2. Which relational database the SQL unit uses.
3. HECC/VR filing requirements and timeline, and whether Wave 0 needs filing.
4. Who tags lessons Core or Advanced, and how much instructor time that takes.
5. Fidgetech Studio input for the internship page.
6. Delivery model: self-paced or cohort.
