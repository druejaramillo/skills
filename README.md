# Skills Repository

A comprehensive collection of expert skills for AI coding assistants, organized into five categories: **Concept Design**, **UI/Frontend**, **Marketing**, **Tools**, and **Project**.

## What are Skills?

Skills are specialized instruction sets that give AI assistants deep expertise in specific domains. Each skill provides detailed methodologies, best practices, reference materials, and workflows to help you accomplish professional-grade work.

---

## Quick Start

Skills are typically loaded automatically by AI coding assistants when they detect relevant work. You can also explicitly invoke them by referencing the skill name or asking for help in that domain.

**Example invocations:**
- "Design a concept for user authentication" → triggers `concept-design`
- "Audit this page for accessibility issues" → triggers `audit`
- "Write copy for my landing page" → triggers `copywriting`
- "Help me implement htmx patterns" → triggers `htmx`

---

## Concept Design (`concept-design/`)

Software design using Daniel Jackson's concept design methodology from *The Essence of Software*.

### Available Skills

#### `concept-design/concept-design/`
Design, write, review, and refine software concepts. Creates concept specs with purpose, state, actions, and operational principles. Use when defining user-facing functionality as discrete, reusable units.

**Use for:** Designing new concepts, reviewing concept specs, splitting/merging concepts, defining product features

#### `concept-design/concept-audit/`
Analyze existing codebases, APIs, and systems through the concept design lens. Identifies concepts, assesses quality, finds overloading and missing concepts, evaluates composition.

**Use for:** Auditing system architecture, finding design problems, refactoring recommendations, concept/code mapping

#### `concept-design/concept-implement/`
Translate concept specs into working code. Maps concepts to packages/modules, implements actions as functions, handles state and synchronization layers.

**Use for:** Implementing concepts in Go/TypeScript/Python, structuring codebases around concepts, creating APIs from specs

**Key principles:**
- Each concept serves one user-facing purpose
- Concepts are independent and don't call each other
- State and actions are explicit and minimal
- Synchronizations compose concepts at the app layer

---

## UI/Frontend (`ui/`)

Professional frontend development with emphasis on design quality, accessibility, and user experience.

### Design & Creation Skills

#### `ui/frontend-design/`
Create distinctive, production-grade interfaces that avoid generic "AI slop" aesthetics. Provides comprehensive guidance on typography, color, layout, motion, and interaction design.

**Use for:** Building web components, pages, applications with exceptional design quality

**Key principles:**
- Bold aesthetic direction (brutalist, refined, playful, etc.)
- Avoid AI tells (cyan-on-dark, gradient text, glassmorphism, nested cards)
- Typography: distinctive fonts, modular scales, proper hierarchy
- Color: OKLCH, tinted neutrals, avoid pure black/white
- Motion: purposeful, natural easing (no bounce/elastic)

#### `ui/web-ui/`
Quick audit rules for web interfaces. Checks accessibility, focus states, forms, animation, typography, performance, and common anti-patterns.

**Use for:** Fast interface reviews, checklist-style feedback

### Quality & Improvement Skills

#### `ui/audit/`
Comprehensive quality audit across accessibility, performance, theming, responsive design, and anti-patterns. Generates detailed reports with severity ratings and recommendations.

**Use for:** Full interface audits, identifying systematic issues, prioritizing improvements

#### `ui/polish/`
Final quality pass before shipping. Fixes alignment, spacing, consistency, interaction states, typography, and detail issues.

**Use for:** Pre-launch polish, catching small details, ensuring production quality

#### `ui/optimize/`
Improve performance: loading speed, rendering, animations, images, bundle size. Covers Core Web Vitals, React optimization, network efficiency.

**Use for:** Fixing performance issues, improving load times, achieving 60fps animations

#### `ui/harden/`
Strengthen against edge cases: text overflow, internationalization, error handling, large datasets, empty states, RTL support.

**Use for:** Production readiness, handling real-world data, i18n support

#### `ui/normalize/`
Align interfaces with design systems and maintain consistency across components.

**Use for:** Design system compliance, visual consistency

### Specialized Enhancement Skills

#### `ui/animate/`
Add purposeful animations and micro-interactions. Covers entrance animations, state transitions, feedback, navigation, and delight moments.

**Use for:** Enhancing interfaces with motion, improving feedback, creating smooth transitions

#### `ui/delight/`
Add moments of joy and personality. Covers micro-interactions, playful copy, illustrations, satisfying interactions, and easter eggs.

**Use for:** Making interfaces memorable, adding brand personality, creating emotional connections

#### `ui/adapt/`
Adapt designs across different screen sizes, devices, platforms, or contexts. Handles mobile/tablet/desktop, touch vs mouse, print, email.

**Use for:** Responsive design, cross-platform adaptation, mobile optimization

### Utility Skills

- `ui/bolder/` - Make designs more confident and prominent
- `ui/quieter/` - Reduce visual noise and create calm
- `ui/clarify/` - Improve clarity and reduce confusion
- `ui/colorize/` - Enhance and refine color usage
- `ui/critique/` - Provide design critique and feedback
- `ui/distill/` - Simplify and focus interfaces
- `ui/extract/` - Extract design patterns and components
- `ui/onboard/` - Improve onboarding experiences
- `ui/teach-impeccable/` - (See skill for details)

---

## Marketing (`marketing/`)

Professional marketing, copywriting, SEO, and creative strategy.

### Copywriting & Content

#### `marketing/copywriting/`
Write compelling marketing copy for any page type: homepage, landing pages, pricing, features, about pages. Covers value propositions, CTAs, headlines, structure.

**Use for:** Writing or improving marketing copy, landing page text, product descriptions

**Key principles:**
- Clarity over cleverness
- Benefits over features
- Specificity over vagueness
- Customer language over company jargon
- One idea per section

#### `marketing/creative-director/`
Generate creative concepts using world-class methodologies (SIT, TRIZ, Lateral Thinking, bisociation). Recursive self-assessment with Cannes/D&AD calibration.

**Use for:** Campaign ideas, Big Ideas, creative concepts, evaluating creative work

**Process:** Intake → Insight discovery → Ideation (multiple methods) → Evaluate & refine (until 9+ score) → Articulate

#### `marketing/humanizer/`
Make AI-generated or corporate copy sound more human and natural.

**Use for:** Improving stiff or robotic copy, adding warmth to corporate content

### SEO & Optimization

#### `marketing/ai-seo/`
Optimize content for AI search engines (ChatGPT, Perplexity, Google AI Overviews, Claude, Gemini). Focus on citations and AI visibility.

**Use for:** Getting cited by AI systems, appearing in AI-generated answers, AI Overviews

**Key tactics:**
- Structure content for extractability (definition blocks, comparison tables, FAQs)
- Add statistics (+40% citation boost), expert quotes (+30%), authoritative tone (+25%)
- Include `/pricing.md` for AI agents to parse
- Use schema markup (FAQ, HowTo, Product)
- Be present on third-party sites (Wikipedia, Reddit, review sites)

#### `marketing/seo-audit/`
Traditional technical and on-page SEO audits.

**Use for:** SEO health checks, technical SEO issues, on-page optimization

#### `marketing/schema-markup/`
Implement structured data for search engines and AI systems.

**Use for:** Adding schema.org markup, improving rich snippets, structured data

### Conversion & Strategy

#### `marketing/page-cro/`
Conversion rate optimization for marketing pages. Analyzes value proposition, headlines, CTAs, hierarchy, trust signals, objections, friction points.

**Use for:** Improving conversion rates, landing page optimization, reducing bounce rates

#### `marketing/form-cro/`
Optimize forms for higher completion rates.

**Use for:** Signup forms, contact forms, checkout flows

#### `marketing/marketing-ideas/`
Generate marketing campaign ideas and tactics.

**Use for:** Brainstorming marketing approaches, campaign planning

#### `marketing/marketing-psychology/`
Apply psychological principles to marketing and persuasion.

**Use for:** Understanding user psychology, persuasive design, behavioral triggers

### Other Marketing Skills

- `marketing/launch-strategy/` - Product launch planning
- `marketing/site-architecture/` - Information architecture and site structure
- `marketing/product-marketing-context/` - Provide product context to other skills

---

## Tools (`tools/`)

Technical integrations and specialized libraries.

### `tools/htmx/`
Expert guidance for building hypermedia-driven web applications with htmx. Covers attributes, patterns, server-side integration, and common UI patterns.

**Use for:** Building htmx applications, HTML-over-the-wire, AJAX without JavaScript frameworks

**Key concepts:**
- Any element can issue requests (not just `<a>` and `<form>`)
- Any event can trigger (click, input, revealed, polling, custom)
- Any HTTP verb (GET, POST, PUT, PATCH, DELETE)
- Server returns HTML fragments, not JSON

**Common patterns:** Click-to-edit, infinite scroll, active search, lazy loading, inline validation

### `tools/stripe/`
End-to-end Stripe payment integration for Go + templ + htmx + Tailwind stacks.

**Use for:** Implementing Stripe payments, subscriptions, saved payment methods

**Covers:**
- One-time payments (Checkout Sessions)
- Setup for future payments (SetupIntents)
- Subscriptions (recurring billing)
- Off-session charges
- Webhook handling and security
- Payment method management

### `tools/pptx-gen/` and `tools/pptx/`
Generate PowerPoint presentations programmatically using PptxGenJS.

**Use for:** Automated presentation generation, report creation

**Covers:** Text, shapes, images, icons (with react-icons), tables, charts, slide backgrounds, masters, layouts

### `tools/revealjs/`
Create polished, professional HTML presentations using reveal.js. No build step required - just open in a browser.

**Use for:** Creating slides, presentations, decks, or slideshows with web technologies

**Key features:**
- Content-informed design approach (palettes match the subject matter)
- Multi-column layouts, code highlighting, animations, speaker notes
- Chart.js integration for data visualization
- Built-in visual overflow checking and screenshot-based review
- Browser-based text editor for quick copy tweaks
- Generates HTML + CSS with no build step - works immediately

**Workflow:**
1. Plan structure (horizontal slides, vertical stacks, section dividers)
2. Generate scaffold with `create-presentation.js` script
3. Customize CSS (colors, fonts, theme)
4. Fill in HTML content incrementally (use Edit tool, not Write)
5. Check for overflow with `check-overflow.js`
6. Visual review with screenshots (decktape)
7. Offer browser editing for copy refinement

**Design principles:**
- Choose colors that match the content (avoid defaults)
- Use `pt` for all font sizes (predictable like PowerPoint)
- Vary layouts across slides (avoid repetition)
- Scale up text when slides have less content
- Text must be in proper HTML elements (`<p>`, `<li>`, `<h1>`-`<h6>`)

---

## Project (`project/`)

Engineering workflow and project management skills for building, debugging, and managing software projects.

### Development & Debugging

#### `project/tdd/`
Test-driven development using the red-green-refactor loop. Emphasizes vertical slices (tracer bullets), testing behavior through public interfaces, and avoiding horizontal layering.

**Use for:** Building features test-first, integration testing, creating testable interfaces

**Key principles:**
- One test → one implementation → repeat (not all tests then all code)
- Test behavior through public interfaces, not implementation details
- Tests should survive refactoring
- Vertical slices cut through ALL layers end-to-end
- Plan with user before coding, prioritize what to test
- Never refactor while RED

**References:** `tests.md`, `mocking.md`, `interface-design.md`, `deep-modules.md`, `refactoring.md`

#### `project/diagnose/`
Disciplined diagnosis loop for hard bugs and performance regressions. Emphasizes building a feedback loop before hypothesizing.

**Use for:** Debugging issues, diagnosing performance problems, investigating broken functionality

**Workflow:**
1. **Build feedback loop** — failing test, curl script, CLI invocation, headless browser, etc.
2. **Reproduce** — confirm the exact symptom
3. **Hypothesise** — generate 3-5 ranked, falsifiable hypotheses
4. **Instrument** — test predictions one variable at a time
5. **Fix + regression test** — write test before fix (if good seam exists)
6. **Cleanup + post-mortem** — remove debug instrumentation, document what would have prevented this

**Key insight:** Build the right feedback loop and the bug is 90% fixed. A 2-second deterministic loop is a debugging superpower.

**References:** `scripts/hitl-loop.template.sh` for human-in-the-loop scenarios

### Issue Management & Planning

#### `project/setup-project-skills/`
Sets up agent configuration for a repository: issue tracker (GitHub, local markdown, or `ISSUES.md`), triage label vocabulary, and domain doc layout.

**Use for:** First-time setup, configuring issue tracking for agent workflows

**Creates:**
- `## Agent skills` block in `CLAUDE.md` or `AGENTS.md`
- `docs/agents/issue-tracker.md` — where issues are tracked
- `docs/agents/triage-labels.md` — label mapping for triage states
- `docs/agents/domain.md` — domain doc consumer rules + layout

**Run before:** `to-issues`, `to-prd`, `triage`, `diagnose`, `tdd`, or `zoom-out`

#### `project/triage/`
Triage issues through a state machine: `needs-triage` → `needs-info` / `ready-for-agent` / `ready-for-human` / `wontfix`.

**Use for:** Creating issues, triaging bugs/features, preparing issues for AFK agents

**Workflow:**
1. Show what needs attention (unlabeled, needs-triage, needs-info with new activity)
2. For each issue: gather context, recommend category/state, reproduce (bugs), grill if needed
3. Apply outcome: agent brief, triage notes, close with explanation, or capture in `.out-of-scope/`

**All triage comments/issues start with:** `> *This was generated by AI during triage.*`

**References:** `AGENT-BRIEF.md`, `OUT-OF-SCOPE.md`

#### `project/to-prd/`
Turn current conversation context into a PRD and publish to the issue tracker. No interview required — synthesizes what you already know.

**Use for:** Creating PRDs from conversation context, documenting planned features

**Template sections:**
- Problem Statement
- Solution
- User Stories (extensive numbered list)
- Implementation Decisions (modules, interfaces, architecture — no file paths)
- Testing Decisions (what to test, prior art)
- Out of Scope
- Further Notes

**Applies:** `needs-triage` label to enter normal triage flow

#### `project/to-issues/`
Break a plan, spec, or PRD into independently-grabbable issues using tracer-bullet vertical slices.

**Use for:** Converting plans to issues, breaking down work into tickets

**Vertical slice rules:**
- Each slice is a thin COMPLETE path through ALL layers (schema, API, UI, tests)
- A completed slice is demoable/verifiable on its own
- Prefer many thin slices over few thick ones
- Mark HITL (human-in-the-loop) vs AFK (agent-ready)

**Process:**
1. Gather context (from conversation or issue tracker)
2. Explore codebase (use domain glossary)
3. Draft vertical slices with dependencies
4. Quiz user on granularity, dependencies, HITL/AFK classification
5. Publish to issue tracker in dependency order with `needs-triage` state

### Planning & Context Management

#### `project/grill/`
Relentless interview to stress-test a plan or design until every branch of the decision tree is resolved. Optionally updates `CONTEXT.md` and ADRs.

**Use for:** Stress-testing plans, aligning with agent before building, resolving design ambiguities

**Invocations:**
- `/grill` — interview only
- `/grill --docs` — interview + update `CONTEXT.md` and ADRs inline

**Process:** Walk down each branch of the design tree, resolve dependencies between decisions one-by-one. Ask questions one at a time. Provide recommended answer for each question.

**In --docs mode:**
- Update `CONTEXT.md` with domain terms as they're resolved
- Create ADRs in `docs/adr/` for architectural decisions
- Summarize changes at end

#### `project/zoom-out/`
Request broader context or higher-level perspective when unfamiliar with code.

**Use for:** Understanding how code fits into the bigger picture, getting a map of relevant modules

**Does:** Goes up a layer of abstraction, provides map of relevant modules and callers using domain glossary vocabulary

### Editing & Communication

#### `project/edit-article/`
Edit and improve articles by restructuring, improving clarity, and tightening prose.

**Use for:** Revising articles, improving drafts, refining written content

**Workflow:**
1. **Configuration** — ask for style preferences or use defaults (short paragraphs, neutral tone, preserve voice, general readers)
2. **Structure** — confirm section order (respect information dependencies)
3. **Rewrite sections** — one at a time, wait for approval
4. **Final pass** — check transitions, opening/closing, terminology consistency

**Configurable:** Paragraph length, tone, voice, audience, style notes

#### `project/caveman/`
Ultra-compressed communication mode. Cuts token usage ~75% by dropping filler, articles, and pleasantries while keeping full technical accuracy.

**Use for:** Token-limited scenarios, rapid-fire technical communication

**Triggers:** "caveman mode", "talk like caveman", "use caveman", "less tokens", "/caveman"

**Stays active** until user says "stop caveman" or "normal mode"

**Pattern:** `[thing] [action] [reason]. [next step].`

**Example:**
- Not: "Sure! I'd be happy to help you with that. The issue you're experiencing is likely caused by..."
- Yes: "Bug in auth middleware. Token expiry check use `<` not `<=`. Fix:"

**Auto-clarity exception:** Temporarily drops caveman for security warnings, irreversible actions, multi-step sequences where fragment order risks misread

---

## Skill Organization

Skills are organized by domain:

```
skills/
├── concept-design/    # Software design methodology
│   ├── concept-design/
│   ├── concept-audit/
│   └── concept-implement/
├── ui/                # Frontend & interface design
│   ├── frontend-design/
│   ├── audit/
│   ├── polish/
│   ├── optimize/
│   ├── animate/
│   └── ... (19 UI skills)
├── marketing/         # Marketing, copy, SEO, creative
│   ├── copywriting/
│   ├── creative-director/
│   ├── ai-seo/
│   ├── page-cro/
│   └── ... (13 marketing skills)
├── tools/             # Technical integrations
│   ├── htmx/
│   ├── stripe/
│   ├── pptx-gen/
│   └── revealjs/
└── project/           # Engineering workflow & project management
    ├── tdd/
    ├── diagnose/
    ├── triage/
    ├── to-issues/
    ├── to-prd/
    ├── grill/
    ├── setup-project-skills/
    ├── edit-article/
    ├── caveman/
    └── zoom-out/
```

Each skill contains:
- `SKILL.md` - Main skill instructions and methodology
- `references/` - Supporting reference materials (optional)
- `assets/` - Templates and examples (optional)

---

## How to Use

### Implicit Usage (Recommended)

Just describe what you want to do. The AI will automatically load relevant skills:

- "Help me design the Label concept" → loads `concept-design`
- "Make this page load faster" → loads `optimize`
- "Write landing page copy for my SaaS" → loads `copywriting`
- "Add htmx infinite scroll" → loads `htmx`
- "This bug is hard to reproduce" → loads `diagnose`
- "Let's build this feature test-first" → loads `tdd`
- "Triage incoming issues" → loads `triage`

### Explicit Invocation

Reference skills by name or domain:

- "Use the concept-audit skill to review my codebase"
- "Apply the frontend-design skill to create this component"
- "I need ai-seo help for my blog posts"
- "/grill --docs" → loads `grill` with documentation updates
- "/caveman" → loads `caveman` mode
- "/triage" → loads `triage`

### Sequential Workflow

Many skills work together:

1. `frontend-design` → create distinctive interface
2. `audit` → identify issues
3. `optimize` → improve performance
4. `harden` → handle edge cases
5. `animate` → add motion
6. `delight` → add personality
7. `polish` → final quality pass

Or:

1. `concept-design` → design the concept
2. `concept-implement` → build the code
3. `concept-audit` → review the implementation

Or:

1. `setup-project-skills` → configure issue tracker and domain docs
2. `grill` → stress-test the plan
3. `to-prd` → create PRD from context
4. `to-issues` → break PRD into vertical slices
5. `triage` → move issues through workflow
6. `tdd` → build features test-first
7. `diagnose` → debug hard issues

---

## Best Practices

### For Concept Design

1. Always start with purpose (what user value does this deliver?)
2. Keep concepts independent (no concept imports another)
3. Use SSF notation for state, bracket notation for actions
4. Write operational principles as short scenarios
5. Check against the seven criteria (user-facing, semantic, independent, behavioral, purposive, end-to-end, familiar)

### For UI/Frontend

1. Start with `frontend-design` for new work
2. Check `web-ui` rules or run `audit` for existing work
3. Use `adapt` for responsive/cross-platform needs
4. Apply `optimize` for performance issues
5. Use `harden` before production
6. Finish with `polish` for final quality

### For Marketing

1. Check for `.agents/product-marketing-context.md` first (many skills use this)
2. Use `copywriting` for page copy, `creative-director` for campaign concepts
3. Apply `ai-seo` for modern search visibility (not just traditional SEO)
4. Use `page-cro` to improve conversion rates
5. Combine skills: `copywriting` + `page-cro` for optimized copy

### For Tools

1. `htmx` - Think HTML-over-the-wire, not JSON APIs
2. `stripe` - Always verify webhook signatures, handle async events
3. `pptx-gen` - Avoid common pitfalls (no "#" in colors, fresh objects for shadows)
4. `revealjs` - Choose colors based on content, review all slides visually, use Edit tool incrementally

### For Project Management

1. Run `setup-project-skills` first to configure issue tracker and domain docs
2. Use `grill` to stress-test plans before building (add `--docs` to update `CONTEXT.md` and ADRs)
3. Use `to-prd` to synthesize conversation into structured PRD
4. Use `to-issues` to break plans into vertical slices (thin end-to-end paths, not horizontal layers)
5. Use `triage` to move issues through state machine (`needs-triage` → `ready-for-agent` / `ready-for-human` / `wontfix`)
6. Use `tdd` for vertical-slice implementation (one test → one impl → repeat)
7. Use `diagnose` for hard bugs (build feedback loop first, then hypothesize)
8. Use `caveman` mode when token budget is tight
9. Use `zoom-out` when unfamiliar with code area

---

## Skill Reference

### By Use Case

**Designing software:**
- New features → `concept-design`
- Code review → `concept-audit`
- Implementation → `concept-implement`

**Building interfaces:**
- New UI → `frontend-design`
- Quality check → `audit` or `web-ui`
- Performance → `optimize`
- Edge cases → `harden`
- Motion → `animate`
- Personality → `delight`
- Final pass → `polish`

**Writing marketing content:**
- Page copy → `copywriting`
- Creative concepts → `creative-director`
- AI search → `ai-seo`
- Conversion → `page-cro`

**Technical integration:**
- Hypermedia apps → `htmx`
- Payments → `stripe`
- PowerPoint presentations → `pptx-gen`
- HTML presentations → `revealjs`

**Project management & workflow:**
- Configure repo → `setup-project-skills`
- Stress-test plan → `grill`
- Create PRD → `to-prd`
- Break into issues → `to-issues`
- Issue workflow → `triage`
- Test-driven dev → `tdd`
- Hard bugs → `diagnose`
- Edit articles → `edit-article`
- Token efficiency → `caveman`
- Understand code → `zoom-out`

---

## Contributing

When adding new skills:

1. Place in appropriate category directory
2. Include `SKILL.md` with clear description and methodology
3. Add `references/` directory for supporting materials
4. Update this README with skill description and use cases
5. Follow existing skill structure and format

---

## License

Skills are provided for use with AI coding assistants. Individual skills may have specific license attributions (see `SKILL.md` files).

---

## Support

For questions about specific skills, refer to the `SKILL.md` file in each skill directory. Most skills include:
- Clear use cases and trigger conditions
- Methodology and principles
- Examples and patterns
- Common pitfalls to avoid
- Related skills for sequential workflows
