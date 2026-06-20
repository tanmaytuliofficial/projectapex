# Project Apex — Product Requirements Document (PRD)

| | |
|---|---|
| **Status** | Draft v1.0 |
| **Owner** | Project Apex Core Team |
| **Input source** | Engineering Handoff Document (Vision, Curriculum Architecture Dossier, Knowledge Graph, Gap Analysis) |
| **Stage** | Build Phase — Step 2 of 10 (see Handoff §8) |
| **Sequencing note** | This PRD defines *what* the product must do. Database schema, system architecture, and content population follow this document, per the already-locked decision "Database before frontend." |

---

## 0. Executive Summary

Project Apex is a self-paced, project-based software-engineering learning platform that replaces "which college did you attend" with "what can you verifiably build." It gives Tier-2/Tier-3 college students, self-taught developers, and career switchers a single, navigable system — a skill graph, career-to-skill mapping, shippable projects, and (eventually) an AI mentor — that compresses the real prerequisite chain to becoming a hireable software engineer, without the bloat of a four-year academic curriculum or the shallowness of a 90-day bootcamp.

This document defines the product, not the curriculum content itself (curriculum content already exists in `curriculum/` per the handoff) and not the database schema or architecture (next steps).

---

## 1. Problem Statement

**The market sorts engineers by pedigree, not capability.**

A Tier-2/Tier-3 college student or a self-taught developer can be every bit as capable as a graduate of a top-tier institute, but they face three compounding problems:

1. **No credible, structured path.** The free internet has plenty of *content* (videos, blog posts, scattered courses) but no coherent *architecture* — no single place that tells a learner what depends on what, what's actually load-bearing for a job, and what's a distraction.
2. **No honest signal of readiness.** Without a degree from a recognized institute, a learner has no reliable way to demonstrate — to themselves or to a recruiter — that they are actually at the level a 20–50 LPA product-company role requires.
3. **Existing "industry-first" alternatives are still incomplete.** Programs like Scaler School of Technology and Coding Blocks School of Technology outperform traditional B.Tech outcomes, but they are expensive, seat-limited, residential commitments — and even they have structural gaps (no theoretical CS core, applied-only math, security as an afterthought, no GPU/parallel computing despite heavy AI content, research treated as a late output rather than a trained skill).

**Nobody has built the free, open, continuously-maintained, gap-free version of this.** Project Apex is that product: a system, not a syllabus PDF, that a motivated learner can run themselves through and emerge from as someone a top product company is excited to interview.

---

## 2. Target Users

| Persona | Background | Core pain | Jobs-to-be-done |
|---|---|---|---|
| **Tier-2/Tier-3 College Student** | Enrolled in a CS or related program at a college without strong faculty density, peer density, or placement infrastructure | Curriculum is outdated or misaligned with industry; peers/mentors who've cracked top offers are rare | "Tell me exactly what to learn and build, in what order, to be competitive with top-tier-college graduates." |
| **Self-Taught Developer** | Learned from scattered tutorials, bootcamps, or on-the-job experience | Has real but fragmented skill; doesn't know what foundational gaps are quietly capping their ceiling (system design, CS fundamentals, etc.) | "Show me what I'm actually missing, not what I already know." |
| **Career Switcher** | Coming from a non-CS background (other engineering, ops, finance, etc.) | The "real" path from zero to hireable is buried inside four years of academic padding | "Give me the shortest *correct* path — not the shortest *shallow* path." |

All three personas share one outcome: a **20–50 LPA (India) / top-quartile-to-elite product-company offer**, achieved through verifiable evidence of capability rather than credentials.

---

## 3. Goals

### 3.1 Product Goals
- Give every learner a single navigable map of what to learn, in what dependency order, with no guesswork about what's core vs. optional.
- Make every unit of learning resolve into a **verifiable artifact** (a project, a contribution, a defended design) rather than passive completion.
- Give learners an honest, continuously updated view of the gap between their current skill profile and a specific target role/company tier.
- Keep the system **self-paced and asynchronous** — no cohorts, no semesters, no fixed start dates — so it works equally well for a working professional, a full-time student, and someone learning nights and weekends.
- Make the product itself a *rendering layer* over the open, versioned, markdown-in-GitHub content — never a black box that hides the underlying curriculum.

### 3.2 Success Metrics (North Star + Supporting)

| Metric | Type | Why it matters |
|---|---|---|
| % of active learners who reach "Core-Complete" status (mandatory core fully marked complete with linked evidence) | North Star (leading) | Proxy for "is the product actually taking someone through the real path" |
| % of Core-Complete learners who report landing a product-company offer in the target comp band | North Star (lagging) | The actual outcome the whole system is judged against (Handoff §2) |
| Skill Graph engagement: nodes explored per active learner per week | Supporting | Is the core IP (the graph) actually being used for navigation |
| Project completion rate per skill domain | Supporting | Validates "Learn by Building" is happening, not just browsing |
| Career Explorer → Skill Graph click-through rate | Supporting | Validates the skills-to-roles line is actually useful, not decorative |
| Search → entity-detail conversion rate | Supporting | Validates search is functioning as real navigation, not a dead end |

We deliberately avoid vanity engagement metrics (streaks, badges, time-on-site) as primary success signals — they reward surface engagement, not the "Skills over Degrees" / "Project-Based Learning" principles the product is built on.

---

## 4. Non-Goals

These mirror the locked decisions in the handoff document (§10–§11) and apply to product scope as much as content scope:

- **Not a video-course / MOOC platform.** Content is markdown-first, AI-and-human readable, versioned in GitHub. The product is a navigation and verification layer over that content, not a video hosting service.
- **Not a degree, certificate, or accreditation body.** Apex does not issue degrees and does not seek university accreditation. Evidence of capability (projects, contributions, design docs) is the credential.
- **Not cohort- or semester-based.** No fixed start dates, no "Year 1 / Year 2" structure, no calendar-driven pacing. Learning Paths are dependency-ordered, not time-ordered (see §6.5). This is an explicit, already-finalized decision — semester/timeline planning is out of scope until separately and explicitly commissioned.
- **Not a research or PhD-prep platform.** No feature should be justified by academic completeness. Every feature, including AI Mentor recommendations, must trace back to the 20–50 LPA hireability outcome (Handoff §2).
- **Not a generalist "learn everything" platform.** Optional/advanced tracks (Quant Engineering, Blockchain, AR/VR, etc.) are real and resourced but must never receive default visibility or framing equal to the mandatory core.
- **Not (in v1) a recruiting marketplace, job board, or employer-facing product.** That connective tissue is valuable but is explicitly Future Scope (§8), not MVP.
- **Not building the AI Mentor early.** Per the already-finalized build sequencing, AI Mentor ships last because it depends on Skill Graph, Career Explorer, Project Explorer, and Progress Tracking data all existing and being correct first.

---

## 5. Core Features

Each feature below includes purpose, user stories, functional requirements, data dependencies, and MVP phase.

### 5.1 Dashboard

**Purpose:** Authenticated home base — a personalized snapshot of where the learner stands and what to do next.

**User stories**
- As a learner, I want to see my completion % across the mandatory core so I know how close I am to "interview ready."
- As a learner, I want to see the gap between my current skills and a specific target role.
- As a learner, I want a short, honest "what to do next" list, not a wall of options.

**Functional requirements**
- Progress summary: mandatory-core completion %, active-track completion %.
- Gap widget: pulls from Career Explorer — pick a target role, see skills present vs. missing.
- "Next best action" list: rule-based in v1 (next unblocked skill/project in the active Learning Path); AI-assisted once AI Mentor ships.
- Quick access to bookmarked skills/careers/projects.
- Lightweight activity log (recently completed items) — deliberately **not** gamified with streaks/badges/leaderboards, consistent with "Skills over Degrees."

**Data dependencies:** Progress Tracking, Skill Graph, Career Explorer, Bookmarks.

**MVP phase:** Phase 2 (ships after Skill Graph, Career Explorer, Project Explorer, and basic Progress Tracking are live — per the existing "Dashboard comes after the core data layer, not before" decision).

---

### 5.2 Skill Graph

**Purpose:** The interactive, navigable version of the Knowledge Graph — the product's central piece of IP, made explorable.

**User stories**
- As a learner, I want to see a skill's prerequisites and what it unlocks, so I know what to learn before/after it.
- As a learner, I want to distinguish mandatory-core skills from optional/track-specific ones at a glance.
- As a learner, I want to jump from a skill directly to the content and projects that teach it.

**Functional requirements**
- Graph/tree visualization of skill nodes and dependency edges (zoomable, filterable).
- Filter by: Mandatory Core / Specialization Track / Optional Track.
- Skill detail view: description, prerequisites, dependent skills, related skills, linked content, linked projects, linked careers (roles that require it).
- Per-skill status control: Not Started / In Progress / Completed (writes to Progress Tracking).

**Data dependencies:** `curriculum/knowledge-graph.md` and `curriculum/knowledge-inventory.md` as seed data, migrated into the Skill database (Handoff §8 Step 5).

**MVP phase:** Phase 1 — this is the first user-facing feature to ship once the Skill database is populated. It is the spine the rest of the product hangs off.

---

### 5.3 Career Explorer

**Purpose:** The honest, concrete line from "skills I have" to "roles I can target" — directly serving the 20–50 LPA outcome.

**User stories**
- As a learner, I want to browse roles (e.g., Backend Engineer, AI Engineer) with their required skills and typical comp band.
- As a learner, I want to see my personal gap against a specific role: skills I have vs. skills required.
- As a learner, I want to see which company tiers/types typically hire for a role.

**Functional requirements**
- Career detail page: role description, required skills (linked into Skill Graph), typical comp band, example company tiers, related specialization tracks.
- Personalized gap view for logged-in learners (requires Progress Tracking).
- Filters: by comp band, by specialization track, by required-skill overlap with learner's current progress.
- **Compensation disclaimer:** comp bands are directional/illustrative, sourced and refreshed periodically, never presented as guaranteed outcomes.

**Data dependencies:** Career database (Handoff §8 Step 6), Skill Graph.

**MVP phase:** Phase 1, ships immediately after the Career database is populated.

---

### 5.4 Project Explorer

**Purpose:** The "Learn by Building" backbone — browsable, shippable project specs tied to skill domains.

**User stories**
- As a learner, I want to find a project that exercises the skill I'm currently learning.
- As a learner, I want a clear spec and a clear "definition of done," not a vague prompt.
- As a learner, I want to submit evidence (a repo/PR link) once I've shipped it.

**Functional requirements**
- Project detail view: spec, prerequisite skills, skills exercised, definition of done, optional stretch goals.
- Evidence submission: link an external repo/PR/deployed URL (Open Source First — evidence lives in the open, not locked inside Apex).
- Filters: by skill domain, by specialization track, by difficulty.
- Mark project status: Not Started / In Progress / Completed (writes to Progress Tracking).

**Data dependencies:** Projects database (Handoff §8 Step 7), Skill Graph.

**MVP phase:** Phase 1, ships after the Projects database is populated.

---

### 5.5 Learning Paths

**Purpose:** A dependency-ordered traversal of the Skill Graph for the mandatory core and for each specialization/optional track — explicitly **not** a calendar or semester plan.

**User stories**
- As a learner with no CS background, I want a recommended order through the mandatory core, so I'm not guessing what to learn first.
- As a learner who has chosen the AI Engineering track, I want the track's path laid on top of the core path.

**Functional requirements**
- A Path is an ordered list of skills (and their associated projects) derived purely from prerequisite edges in the Skill Graph — one Path for Mandatory Core, one per specialization/optional track.
- **No dates, no week numbers, no fixed duration, no cohort attached to any Path.** This is a hard constraint, not a stylistic preference — it preserves the already-finalized decision that semester/timeline planning is out of scope.
- Per-Path progress indicator (derived from Progress Tracking, not a separate data store).
- A learner can be "on" multiple Paths simultaneously (Core + one or more tracks).

**Data dependencies:** Skill Graph, Track definitions (`curriculum/tracks/`).

**MVP phase:** Phase 1 — ships alongside the Skill Graph, since a Path is largely a derived/filtered view of the graph rather than new data.

---

### 5.6 Progress Tracking

**Purpose:** The persistent record of a learner's status across skills, projects, and tracks — the data layer that the Dashboard, Career gap-view, and future AI Mentor all read from.

**User stories**
- As a learner, I want to mark a skill as learned, in progress, or not started.
- As a learner, I want to mark a project complete and attach evidence.
- As a learner, I want to see my rollup completion % for the core and for any active track.

**Functional requirements**
- Per-skill status with optional self-assessed confidence level.
- Per-project status with evidence link.
- Aggregate rollups: mandatory-core %, per-track %, overall.
- Private by default (the *record* of progress); the *evidence* (linked repos/PRs) is inherently public, consistent with Open Source First.

**Data dependencies:** Skill Graph, Project Explorer, Auth/User identity.

**MVP phase:** Phase 1 — must ship alongside Skill Graph and Project Explorer, since "mark as done" is what makes those two features actionable rather than read-only.

---

### 5.7 AI Mentor

**Purpose:** A conversational, agentic layer that uses the learner's actual progress plus the Skill/Career/Project data to give grounded, personalized guidance. Intentionally the most complex feature and intentionally last.

**User stories** *(future)*
- As a learner, I want to ask "what should I learn next to be ready for a Backend Engineer role" and get an answer grounded in my actual progress.
- As a learner, I want to get unblocked when stuck on a specific project.

**Functional requirements** *(future)*
- Grounded in Apex's own Skill/Career/Project data (retrieval over Apex's content), not a generic open-ended tutor — recommendations must trace back to an actual graph node, role, or project, not be invented ad hoc.
- Inherits the Primary Goal filter (Handoff §2): must not recommend research-depth tangents or theory "because it's interesting" without a stated line back to hireability.
- Cites the specific Skill Graph node(s)/project(s) behind any recommendation.

**Data dependencies:** Skill Graph, Career Explorer, Project Explorer, and Progress Tracking — **all must already exist, be populated, and be correct.** Building this earlier means building a mentor with nothing reliable to mentor against.

**MVP phase:** Out of scope for v1/v2. Phase 3+ (see §7).

---

### 5.8 Search

**Purpose:** Fast, unified lookup across skills, careers, projects, and content.

**User stories**
- As a learner, I want to search "Kubernetes" and immediately see the matching skill, related projects, and related career paths in one result set.

**Functional requirements**
- Unified index across Skill, Career, and Project entities (and, later, full content markdown).
- Typeahead with entity-type filters (Skill / Career / Project).
- Result cards link directly into the relevant detail page.

**Data dependencies:** Skill, Career, and Project databases once populated.

**MVP phase:** Phase 1, lightweight keyword search at launch; full-text content search added once `content/` population matures.

---

### 5.9 Bookmarks

**Purpose:** Let a learner save any skill, career, project, or path to revisit later.

**User stories**
- As a learner, I want to bookmark a skill/career/project so I can find it again without re-searching.
- As a learner, I want one place that shows everything I've saved.

**Functional requirements**
- Bookmark toggle available on every Skill/Career/Project/Path detail view.
- A single "Saved" list view, filterable by entity type.

**Data dependencies:** Auth/User identity; Skill, Career, Project, and Path entities.

**MVP phase:** Phase 1 — simple to add once entities and auth exist; high value-to-effort ratio.

---

## 6. Release Phasing (MVP Definition)

This phasing follows the already-locked build sequence in Handoff §8 ("Database before frontend," "Dashboard after the core data layer," "AI Mentor last").

| Phase | Ships | Excludes |
|---|---|---|
| **Phase 0 — Foundation** | Database schema, system architecture, Skill/Career/Project databases populated. No end-user UI yet. | Everything user-facing |
| **Phase 1 — MVP** | Skill Graph, Learning Paths, Career Explorer, Project Explorer, basic Progress Tracking, Search, Bookmarks | Dashboard, AI Mentor |
| **Phase 2 — Personalization** | Dashboard (progress rollups, gap widget, next-best-action), richer Progress Tracking (confidence levels, evidence review) | AI Mentor |
| **Phase 3 — Intelligence** | AI Mentor (grounded recommendations, citations into the graph) | Employer/recruiting features |

A feature does not move to an earlier phase just because it's easy to build — the dependency order above reflects the actual decisions in the handoff, not implementation convenience.

---

## 7. Open Questions

- What is the precise, measurable definition of "Core-Complete" / "job-ready" used for the North Star metric? (Needs to be nailed down before Progress Tracking rollup logic is built.)
- What is the sourcing and refresh methodology for Career Explorer compensation bands?
- Will evidence links (repos/PRs) be verified in any automated way in v1, or is self-reported linking sufficient until a future GitHub-integration feature?
- Should optional/advanced tracks be hidden by default for new learners (to avoid distraction from the mandatory core) or simply de-emphasized? Needs a UX decision, not just a data-modeling one.

---

## 8. Future Scope (explicitly post-MVP, not committed)

- **Employer/recruiting layer:** a way for product companies to discover learners with verified, graph-mapped skill evidence — the natural extension of "skills over degrees," but deliberately not in MVP scope.
- **Automated evidence verification:** GitHub integration to auto-detect merged PRs, contribution activity, and link it to Progress Tracking instead of relying on self-reported links.
- **Competitive-programming integration:** pull Codeforces/similar ratings automatically into a learner's profile, mirroring CBSOT's externally-verifiable outcome tracking.
- **Community layer:** peer code review, study groups, open-source contribution matching.
- **Mobile app.**
- **Internationalization:** comp-band data and role framing for markets beyond India.
- **Adaptive skill verification:** lightweight assessments that validate self-reported skill status rather than relying purely on self-assessment.
- **Monetization model:** premium tier, sponsorships, or employer-side pricing — explicitly undecided and out of scope for this PRD.
- **AI-generated personalized project briefs:** once AI Mentor exists, extend it to generate net-new project specs tailored to a learner's gaps, rather than only surfacing existing ones.

None of the above should be treated as committed roadmap — they are documented here so they aren't lost, and so future scoping discussions start from a list rather than from scratch.

---

## 9. Traceability

Every feature and constraint in this PRD traces back to a decision already locked in the handoff document:

- Mandatory-core/track architecture → §5 of handoff → drives Skill Graph and Learning Paths structure.
- "Database before frontend" → handoff §10 → drives the Release Phasing in §6 of this PRD.
- "Semester/timeline planning postponed" → handoff §10/§11 → drives the hard no-calendar constraint on Learning Paths (§5.5).
- "AI Mentor last" → handoff §8 Step 10 → drives Phase 3 placement (§5.7, §6).
- "Research content is supportive, never central" → handoff §2/§10 → drives the AI Mentor's grounding/citation requirement and the Non-Goals in §4.

Any future change to this PRD that would contradict one of these should be flagged explicitly, not silently merged — consistent with handoff §11.
