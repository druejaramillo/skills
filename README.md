# Skills Repository

A comprehensive collection of expert skills for AI coding assistants, organized into four categories: **Concept Design**, **UI/Frontend**, **Marketing**, and **Tools**.

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
└── tools/             # Technical integrations
    ├── htmx/
    ├── stripe/
    ├── pptx-gen/
    └── revealjs/
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

### Explicit Invocation

Reference skills by name or domain:

- "Use the concept-audit skill to review my codebase"
- "Apply the frontend-design skill to create this component"
- "I need ai-seo help for my blog posts"

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
