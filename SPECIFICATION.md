# ExploreMath – Project Specification

## Vision

ExploreMath is an interactive math learning platform for German and English pupils (grades 5–12). It teaches mathematics through experimentation, interactive animated diagrams, and a deep focus on understanding *why* and *how* math works — not just mechanical execution.

## Core Principles

1. **Understanding over execution** — Pupils learn the reasoning behind mathematical concepts, not just procedures.
2. **Experiment-driven** — Lessons feel like interactive experiments where pupils discover patterns and relationships themselves.
3. **Visual and animated** — Every concept is accompanied by beautifully animated, interactive diagrams that pupils can manipulate.
4. **Bilingual** — Full support for German and English (German primary, English secondary).
5. **Grade-structured** — Content organized by German school grades (Klasse 5–12).

## Target Audience

- German and English-speaking pupils, ages ~10–18
- Grades 5–12 (German school system: Gymnasium/Realschule)
- Self-learners and pupils needing supplemental explanation

## Content Structure

```
Grade (Klassenstufe)
└── Topic (Themengebiet)
    └── Lesson (Lektion)
        ├── Introduction / Motivation (Why does this matter?)
        ├── Interactive Experiment (Explore the concept)
        ├── Explanation (What did we discover?)
        ├── Practice (Apply understanding)
        └── Challenge (Extend thinking)
```

### Grade Overview (Example Topics)

| Grade | Example Topics |
|-------|---------------|
| 5 | Natural numbers, basic geometry, fractions introduction |
| 6 | Fractions, decimals, ratios, angles |
| 7 | Integers, linear equations, proportionality, triangles |
| 8 | Linear functions, Pythagorean theorem, probability |
| 9 | Quadratic functions, powers, similarity, trigonometry basics |
| 10 | Exponential functions, circles, statistics, polynomial functions |
| 11 | Differential calculus, vectors, advanced functions |
| 12 | Integral calculus, stochastics, analytical geometry |

## Lesson Design Philosophy

Each lesson follows the **Experiment → Discover → Understand → Apply** cycle:

1. **Hook** — A real-world question or puzzle that motivates the concept.
2. **Experiment** — Interactive diagrams where pupils manipulate parameters and observe results.
3. **Discovery** — Guided questions that lead pupils to formulate the mathematical principle themselves.
4. **Formalization** — The proper mathematical notation and terminology, now that the concept is understood.
5. **Application** — Problems that require understanding, not just formula application.

### Interactive Diagrams Requirements

- Animated transitions showing how changing parameters affects results
- Draggable points, sliders, and adjustable values
- Real-time visualization of mathematical relationships
- Step-by-step animated walkthroughs of problem-solving approaches
- "What happens if...?" exploration mode

## Technical Architecture

### Stack

| Layer | Technology |
|-------|-----------|
| Framework | Astro (static site generation) |
| UI Components | Svelte (interactive islands) |
| Math Rendering | KaTeX |
| Interactive Diagrams | D3.js + custom Svelte components |
| Animations | GSAP / Svelte transitions |
| Styling | Tailwind CSS |
| Deployment | GitHub Pages via GitHub Actions |
| Language | TypeScript |

### Why This Stack

- **Astro** — Fast static pages with minimal JS; perfect for content-heavy educational site.
- **Svelte** — Lightweight reactive components for interactive diagrams without heavy runtime.
- **D3.js** — Industry standard for data visualization; ideal for mathematical diagrams.
- **KaTeX** — Fast, server-side math rendering.
- **Tailwind** — Utility-first CSS for consistent, responsive design.

### Project Structure

```
explore-math/
├── src/
│   ├── components/        # Reusable Svelte/Astro components
│   │   ├── diagrams/      # Interactive diagram components
│   │   ├── ui/            # UI primitives (buttons, sliders, cards)
│   │   └── layout/        # Page layout components
│   ├── content/           # Markdown/MDX lesson content
│   │   ├── de/            # German content
│   │   │   ├── klasse-5/
│   │   │   ├── klasse-6/
│   │   │   └── ...
│   │   └── en/            # English content
│   │       ├── grade-5/
│   │       ├── grade-6/
│   │       └── ...
│   ├── layouts/           # Astro layout templates
│   ├── pages/             # Route pages
│   ├── styles/            # Global styles
│   └── i18n/              # Internationalization strings
├── public/                # Static assets
├── .github/workflows/     # GitHub Actions (deployment)
├── astro.config.mjs
├── tailwind.config.mjs
├── package.json
└── tsconfig.json
```

### Internationalization (i18n)

- URL-based locale: `/de/klasse-7/lineare-funktionen` vs `/en/grade-7/linear-functions`
- Content files maintained separately per language
- Shared interactive components with localized labels
- Language switcher in navigation

## Design Guidelines

### Visual Style

- Clean, modern, distraction-free
- Large readable typography (optimized for younger readers)
- Generous whitespace
- Color-coded by grade level for easy navigation
- Dark/light mode support
- Mobile-first responsive design

### Interaction Design

- Diagrams respond instantly to user input
- Smooth animations (60fps target)
- Clear affordances (draggable items look draggable)
- Undo/reset buttons on all experiments
- Progressive disclosure (complexity revealed step-by-step)

## Non-Functional Requirements

- **Performance** — Lighthouse score ≥ 90 on all metrics
- **Accessibility** — WCAG 2.1 AA compliant
- **Offline** — Service worker for offline lesson access (future)
- **SEO** — Proper meta tags, structured data for educational content
- **Analytics** — Privacy-respecting usage analytics (future)

## Development Phases

### Phase 1 — Foundation (Current)
- Project setup, build pipeline, deployment
- Base layout and navigation
- Grade selection page
- One complete example lesson with interactive diagrams

### Phase 2 — Content Framework
- Lesson template system
- Multiple diagram component types (function plots, geometry, number lines)
- i18n infrastructure
- 3–5 lessons per grade for grades 5–7

### Phase 3 — Expansion
- Full lesson coverage for all grades
- Advanced diagram types (3D, animations sequences)
- Progress tracking (local storage)
- Search functionality

### Phase 4 — Polish
- Community contributions system
- Teacher resources
- Print-friendly lesson versions
- Mobile app wrapper (PWA)
