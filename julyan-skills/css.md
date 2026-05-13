# CSS Guide for any AI reading this

version = 2.0

A consolidated CSS-first guide for building polished, production-ready interfaces without falling into generic “AI slop.” This guide combines the attached design references into one practical Markdown file and intentionally excludes workflows, terminal commands, JavaScript-driven implementations, external scripts, and multi-file setup instructions.

## Table of Contents

1. [Basics](#basics)
1. [Core Design Laws](#core-design-laws)
2. [Pre-CSS Design Brief](#pre-css-design-brief)
3. [Brand vs Product Register](#brand-vs-product-register)
4. [AI Slop: What to Avoid](#ai-slop-what-to-avoid)
5. [CSS Token Foundation](#css-token-foundation)
6. [Color and Contrast](#color-and-contrast)
7. [Typography](#typography)
8. [Layout and Spacing](#layout-and-spacing)
9. [Responsive Adaptation](#responsive-adaptation)
10. [Components](#components)
11. [Interactive States](#interactive-states)
12. [Motion and Animation](#motion-and-animation)
13. [Depth, Surfaces, and Texture](#depth-surfaces-and-texture)
14. [Quieting a Loud Interface](#quieting-a-loud-interface)
15. [Hardening for Real Content](#hardening-for-real-content)
16. [UX Writing for Interface States](#ux-writing-for-interface-states)
17. [Onboarding and Empty States](#onboarding-and-empty-states)
18. [Delight Without Noise](#delight-without-noise)
19. [Performance-Oriented CSS](#performance-oriented-css)
20. [Accessibility CSS](#accessibility-css)
21. [Print and Low-Context Adaptation](#print-and-low-context-adaptation)
22. [Audit Checklists](#audit-checklists)
23. [Polish and Design-System Alignment](#polish-and-design-system-alignment)
24. [Ready-to-Paste CSS Starter](#ready-to-paste-css-starter)

---

## Basics

### Naming classes:

1) All classes must be in lowercase, separated by "-" (without spaces)
2) Try to not repeat symbols
3) Do not use "_"
4) Names should be short, usually: component + html element

```css
/* Good: */
.home-div

/* Bad: */
.home__Div
```

### Properties order:

When you create/edit a css class, make sure the properties order is similar to the following example:

```css
.test {
    /* Transform properties */
    flex
    flex-shrink
    width
    max-width
    padding
    margin
    position
    top
    right
    inset
    transform
    z-index
    vertical-align

    /* Display properties */
    display
    flex-flow: row nowrap;
    justify-content
    align-items
    gap
    grid-template-columns

    /* Content properties */
    content
    box-sizing
    overflow
    overflow-x: hidden;
    overflow-y: auto;
    overscroll-behavior-y: contain;
    scroll-behavior
    word-wrap
    overflow-wrap
    white-space

    /* Border properties */
    border
    border-radius
    border-color

    /* Background properties */
    outline
    background
    background-color
    box-shadow

    /* Image properties */
    object-fit
    aspect-ratio
    cursor
    opacity
    visibility
    filter
    mix-blend-mode
    pointer-events
    -webkit-appearance

    /* Text properties */
    color
    font-size
    text-align
    font-family
    font-weight
    line-height
    text-decoration
    text-transform
    list-style
    letter-spacing
    text-shadow
    line-clamp
    accent-color

    /* Transition properties */
    transition
    will-change
    animation
}
```

Note: If one propertie is not listed here, try to add it in a section it could belong

### @media screen

1) These are the allowed resolutions to use.
2) Default is for phones since I like using mobile-first structure
3) They should be placed at the end of the .css file

```css
/* Phones */

/* Default starting point (around 320px) */

/* Small tablets */

@media screen and (min-width: 480px) {}

/* Large tablets */

@media screen and (min-width: 768px) {}

/* Laptops */

@media screen and (min-width: 1024px) {}

/* Desktop */

@media screen and (min-width: 1200px) {}
```

### Pseudo-classes

For pseudo-classes (like links and buttons), follow this pattern whenever possible (hover must be inside @media):

```css
:link    { }
:visited { }
:focus-visible { }
@media (hover: hover) {
    :hover {

    }
}
:active  { }
:disabled { }
```

### All variables must be inside :root

If this is a React+Vite project, it should be inside /frontend/src/index.css
If this is a Nextjs project, it should be inside /frontend/src/globals.css

```css
:root {
    --white: white;
    --black: black;
}
```

### Try not to repeat code/css properties that have already been established in a parent and that affects all its children

For example, if it was declared:

```css
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}
```

Then there's no need to add box-sizing again to a parent (unless it's a very specific and rare case where the box-sizing changes and then it has to be reset), or adding padding/margin: 0

---

## Core Design Laws

### 1. Design is hierarchy, not decoration

Every screen needs a clear answer to:

- What should the user notice first?
- What should they do next?
- What is secondary?
- What can disappear until needed?

If everything has equal weight, the interface becomes mental noise.

### 2. Bold does not mean more effects

Bold means committed. It can come from scale, contrast, layout, typography, rhythm, color dosage, or a single memorable visual motif. It does not mean cyan/purple gradients, glass panels, neon glows, or random animation.

### 3. Simpler does not mean empty

Distillation removes obstacles, not meaning. Keep what helps the user act. Remove redundant copy, repeated containers, decorative borders, weak shadows, and competing calls to action.

### 4. Responsive design is adaptation, not scaling

A desktop design cannot simply shrink into a mobile design. Touch targets, navigation, reading rhythm, density, hierarchy, and content priority must change for the context.

### 5. Motion must explain state

Use motion to show feedback, reveal relationships, smooth transitions, and add earned delight. Cut animation that only says “look at me.”

### 6. Production design survives bad data

A good interface handles long names, empty states, slow loading, narrow screens, translated strings, emoji, focus states, and errors.


### 7. Good CSS starts before CSS

Before writing styles, define the surface you are styling. A product dashboard, a marketing landing page, a game lobby, and a checkout form can all use the same CSS properties but should not feel the same. CSS quality improves when the target experience is clear first.

---

## Pre-CSS Design Brief

Use this short brief before styling a new surface. It is not a workflow command and does not require scripts; it is a thinking checklist to prevent generic UI decisions.

### 1. Purpose

Answer in plain language:

- What is this surface for?
- Who uses it, and in what context?
- What is the one thing the user must understand or do first?
- What does success look like?

### 2. Register

Choose one default register for the surface:

- **Brand**: landing pages, campaigns, portfolios, editorial pages, home pages, launch pages. Design is part of the product.
- **Product**: dashboards, app shells, settings, forms, tools, game lobbies, admin screens. Design serves the task.

Mixed surfaces are common. A product can have a brand-heavy hero; a landing page can include product-like pricing tables. Choose the dominant register and override locally when needed.

### 3. User state of mind

Style for the user’s state:

| User mood | CSS implication |
|---|---|
| Rushed | high contrast, obvious actions, less decoration |
| Exploring | richer hierarchy, more visual rhythm, optional details |
| Anxious | plain copy, predictable controls, calmer colors |
| Focused | dense layout, fewer interruptions, restrained motion |
| Celebrating | earned animation, warmer copy, stronger accent moments |

### 4. Content and states

List the realistic states before styling:

- Default.
- Empty.
- Loading.
- Error.
- Success.
- Disabled or unavailable.
- Long content.
- Small content.
- Translated content.
- Mobile/touch version.

A design that only works with perfect demo content is not finished.

### 5. Visual direction

Pick concrete anchors instead of vague adjectives:

- Color strategy: **Restrained**, **Committed**, **Full palette**, or **Drenched**.
- Scene sentence: “This should feel like ___ using it in ___ light while ___.”
- References: 2–3 real products, sites, objects, posters, game screens, or physical materials.
- Anti-goals: what this should never look like.

### 6. Scope

Decide whether you are making:

- A quick sketch.
- A mid-fidelity layout.
- A high-fidelity visual pass.
- A production-ready component or surface.

Do not apply flagship-level polish to a disposable sketch, and do not ship a sketch as production CSS.

---

## Brand vs Product Register

### Brand register

Use brand-register styling when the page must be remembered. The design can be more expressive because the user is not only completing a task; they are forming an impression.

Brand surfaces can support:

- Fluid display typography.
- More dramatic color dosage.
- Asymmetric layouts.
- Richer texture or atmosphere.
- Larger spacing jumps.
- More distinctive font choices.
- More memorable transitions, as long as they do not block reading.

```css
.brand-hero {
  display: grid;
  min-block-size: min(760px, 100svh);
  align-items: end;
  padding-block: clamp(5rem, 12vw, 10rem);
  background:
    radial-gradient(circle at 20% 15%, color-mix(in oklch, var(--color-primary) 35%, transparent), transparent 28rem),
    var(--color-bg);
}

.brand-hero-title {
  max-width: 10ch;
  font-size: clamp(3.5rem, 10vw, 9rem);
  line-height: 0.88;
  letter-spacing: -0.07em;
  text-wrap: balance;
}
```

### Product register

Use product-register styling when users are trying to complete tasks. Familiarity is a feature. The UI should feel trustworthy, stable, and easy to operate.

Product surfaces usually need:

- System or familiar sans fonts.
- Fixed `rem` type scales instead of fluid headings.
- Restrained color.
- Predictable grids.
- Standard navigation patterns.
- Every interaction state.
- Skeletons instead of theatrical loaders.
- Motion that explains state, not mood.

```css
.product-shell {
  min-block-size: 100svh;
  display: grid;
  grid-template-columns: 16rem minmax(0, 1fr);
  background: var(--color-bg);
  color: var(--color-text);
}

.product-sidebar {
  border-inline-end: 1px solid var(--color-border);
  background: color-mix(in oklch, var(--color-surface) 88%, var(--color-bg));
}

.product-main {
  min-width: 0;
  padding: var(--space-xl);
}

@media (max-width: 820px) {
  .product-shell {
    grid-template-columns: 1fr;
  }

  .product-sidebar {
    border-inline-end: 0;
    border-block-end: 1px solid var(--color-border);
  }
}
```

### Product slop test

For product UI, the problem is rarely that it looks “too plain.” The problem is usually that it feels strange in tiny ways: inconsistent controls, too much decoration, invented affordances, loud inactive states, mismatched form elements, and decorative motion.

A strong product UI lets the user fluent in comparable tools sit down and trust it immediately.

### Register-specific type strategy

```css
/* Product: fixed, predictable */
.product-surface {
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.125rem;
  --text-xl: 1.35rem;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
}

/* Brand: fluid and expressive */
.brand-surface {
  --text-base: 1rem;
  --text-lg: clamp(1.125rem, 1vw + 0.9rem, 1.5rem);
  --text-xl: clamp(1.5rem, 3vw + 0.8rem, 4rem);
  --text-display: clamp(3rem, 9vw, 8rem);
}
```

---

## AI Slop: What to Avoid

These choices make a web interface look obviously generated or generic:

- Purple-to-blue gradients as the default “premium” look.
- Glassmorphism everywhere.
- Neon cyan/purple accents on dark backgrounds.
- Gradient text on hero headings or metrics without a brand reason.
- Hero sections with floating cards, fake metrics, and vague SaaS copy.
- Repeating card grids: icon, heading, paragraph, icon, heading, paragraph.
- Centered stack for every landing page.
- Excessive rounded rectangles with identical shadows.
- Generic system fonts used without typographic hierarchy.
- Monospace used as lazy shorthand for “developer.”
- Side accent stripes on cards.
- Gray text on colored backgrounds.
- Pure black or pure gray as large-area design colors.
- Bounce or elastic easing that makes UI feel toy-like.
- Decorative animation with no state meaning.
- Placeholder text used as the only label.
- Icon-only navigation with no text or accessible name.

### Better alternatives

| Slop Pattern | Better Direction |
|---|---|
| Purple-blue gradient | Palette based on the actual brand hue, preferably in OKLCH |
| Glass panels everywhere | Solid surfaces, tonal layering, or one intentional translucent material |
| Centered hero stack | Asymmetric layout, strict grid, editorial split, or product-first composition |
| Repeated cards | Mixed layout rhythm, feature rows, comparison panels, narrative sections |
| Neon glow | Controlled accent, hairline border, background wash, focus ring, or shadow bloom |
| Fake metrics | Real product evidence, screenshots, concrete outcomes, or no metrics |

---

## CSS Token Foundation

Use tokens so the interface feels coherent and stays easy to change.

```css
:root {
  /* Color: use OKLCH for perceptual consistency */
  --color-bg: oklch(96% 0.012 145);
  --color-surface: oklch(91% 0.018 145);
  --color-surface-raised: oklch(94% 0.014 145);
  --color-text: oklch(18% 0.035 145);
  --color-muted: oklch(43% 0.035 145);
  --color-border: oklch(78% 0.025 145);
  --color-primary: oklch(62% 0.16 145);
  --color-primary-strong: oklch(48% 0.18 145);
  --color-accent: oklch(76% 0.15 78);
  --color-danger: oklch(58% 0.2 25);
  --color-success: oklch(58% 0.14 150);
  --color-warning: oklch(72% 0.16 75);

  /* Spacing */
  --space-2xs: 0.25rem;
  --space-xs: 0.5rem;
  --space-sm: 0.75rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  --space-xl: 2rem;
  --space-2xl: 3rem;
  --space-3xl: 4.5rem;
  --space-section: clamp(4rem, 8vw, 9rem);

  /* Radius */
  --radius-sm: 0.375rem;
  --radius-md: 0.75rem;
  --radius-lg: 1.25rem;
  --radius-xl: 2rem;
  --radius-pill: 999px;

  /* Type */
  --font-body: ui-sans-serif, system-ui, sans-serif;
  --font-display: ui-sans-serif, system-ui, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;

  --text-xs: clamp(0.75rem, 0.72rem + 0.15vw, 0.84rem);
  --text-sm: clamp(0.875rem, 0.84rem + 0.2vw, 1rem);
  --text-base: clamp(1rem, 0.96rem + 0.25vw, 1.125rem);
  --text-lg: clamp(1.125rem, 1.04rem + 0.45vw, 1.35rem);
  --text-xl: clamp(1.35rem, 1.1rem + 1.1vw, 2rem);
  --text-2xl: clamp(2rem, 1.4rem + 3vw, 4rem);
  --text-3xl: clamp(3rem, 1.9rem + 5.4vw, 7rem);

  /* Elevation */
  --shadow-sm: 0 1px 2px oklch(18% 0.035 145 / 0.08);
  --shadow-md: 0 1rem 3rem oklch(18% 0.035 145 / 0.12);
  --shadow-lg: 0 2rem 6rem oklch(18% 0.035 145 / 0.18);

  /* Motion */
  --duration-fast: 120ms;
  --duration-base: 220ms;
  --duration-slow: 420ms;
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in: cubic-bezier(0.7, 0, 0.84, 0);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);

  /* Z-index scale */
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-backdrop: 300;
  --z-modal: 400;
  --z-toast: 500;
  --z-tooltip: 600;
}
```

### Token rules

- Use semantic tokens in components: `--color-primary`, not `--green-500`.
- Avoid random one-off values. If a value repeats, tokenize it.
- Tokens should express intent, not just appearance.
- Use tinted neutrals, not pure gray.
- Build color variants by changing OKLCH lightness first, chroma second.
- Do not use alpha everywhere to “fix” an incomplete palette.

---

## Color and Contrast

### Use OKLCH instead of HSL

OKLCH lightness is perceptually more consistent. Equal lightness steps look more even than HSL.

```css
:root {
  --brand-950: oklch(18% 0.06 145);
  --brand-800: oklch(32% 0.1 145);
  --brand-600: oklch(48% 0.16 145);
  --brand-500: oklch(58% 0.18 145);
  --brand-300: oklch(76% 0.12 145);
  --brand-100: oklch(92% 0.04 145);
}
```

Reduce chroma near white and black. High chroma at extreme lightness looks artificial.

### Build tinted neutrals

Pure neutral gray often feels dead next to a brand color. Add a tiny amount of chroma toward the brand hue.

```css
:root {
  --neutral-950: oklch(16% 0.012 145);
  --neutral-800: oklch(30% 0.012 145);
  --neutral-600: oklch(46% 0.01 145);
  --neutral-400: oklch(66% 0.009 145);
  --neutral-200: oklch(86% 0.008 145);
  --neutral-100: oklch(94% 0.006 145);
  --neutral-50: oklch(98% 0.004 145);
}
```

### Color dosage strategies

#### Restrained

Best for product UIs and dashboards. Accent appears on primary actions, selected states, focus, and small highlights only.

```css
.button-primary,
[aria-current="page"],
:focus-visible {
  --active-color: var(--color-primary);
}
```

#### Committed

One saturated color carries a large part of the surface. Useful for brand pages.

```css
.hero {
  background: var(--color-primary);
  color: oklch(98% 0.006 145);
}
```

#### Full palette

Use 3–4 distinct color roles with strict meaning. Do not apply colors randomly.

```css
.badge-success { --badge-color: var(--color-success); }
.badge-warning { --badge-color: var(--color-warning); }
.badge-danger { --badge-color: var(--color-danger); }
```

#### Drenched

The surface is the color. Use for bold brand work only. Keep typography and layout calm enough to carry it.

```css
.campaign-page {
  background: oklch(55% 0.2 145);
  color: oklch(98% 0.02 145);
}
```

### Contrast requirements

| Content | Minimum |
|---|---:|
| Body text | 4.5:1 |
| Large text | 3:1 |
| UI components and icons | 3:1 |
| Focus indicators | 3:1 against adjacent colors |

### Dangerous combinations

Avoid:

- Light gray text on white.
- Gray text on colored backgrounds.
- Red on green or green on red.
- Yellow text on white.
- Blue text on red.
- Thin light text over photos.
- Placeholder text that fails contrast.

### Dark mode

Dark mode is not inverted light mode. Use lighter surfaces for elevation instead of heavy shadows.

```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: oklch(14% 0.018 145);
    --color-surface: oklch(18% 0.02 145);
    --color-surface-raised: oklch(23% 0.024 145);
    --color-text: oklch(92% 0.01 145);
    --color-muted: oklch(72% 0.014 145);
    --color-border: oklch(32% 0.026 145);
    --color-primary: oklch(70% 0.13 145);
  }
}
```

---

## Typography

### Type is voice

Typography should come from the product’s personality, not from reflex. “Modern” and “elegant” are not enough. Pick physical-feeling words such as:

- dense, mechanical, precise
- warm, handmade, grounded
- sharp, editorial, ceremonial
- playful, chunky, kinetic
- quiet, clinical, careful

### Scale rules

- Use a visible scale difference. Headings should not be only slightly larger than body text.
- Use `clamp()` for fluid headings.
- Keep body text readable at all breakpoints.
- Use line length limits: 60–75ch for long reading.
- Do not overuse uppercase. It is harder to scan.
- A single strong font family with clear weight contrast is often better than a timid pairing.

```css
body {
  font-family: var(--font-body);
  font-size: var(--text-base);
  line-height: 1.6;
  color: var(--color-text);
  background: var(--color-bg);
}

h1,
.h1 {
  font-family: var(--font-display);
  font-size: var(--text-3xl);
  line-height: 0.92;
  letter-spacing: -0.055em;
  max-width: 11ch;
  text-wrap: balance;
}

h2,
.h2 {
  font-size: var(--text-2xl);
  line-height: 1;
  letter-spacing: -0.04em;
  text-wrap: balance;
}

h3,
.h3 {
  font-size: var(--text-xl);
  line-height: 1.1;
  letter-spacing: -0.025em;
}

p,
.prose {
  max-width: 68ch;
}

.eyebrow {
  font-size: var(--text-xs);
  font-weight: 700;
  letter-spacing: 0.14em;
  text-transform: uppercase;
  color: var(--color-muted);
}
```

### Text wrapping

```css
h1,
h2,
h3,
.display-text {
  text-wrap: balance;
}

p,
li,
.body-copy {
  text-wrap: pretty;
}
```

### Vertical rhythm

Use line-height as part of the spacing system. If body text is `1rem` with `line-height: 1.5`, the resulting 24px rhythm can guide related spacing values.

```css
:root {
  --leading-body: 1.5;
  --rhythm: 1.5rem;
}

.prose > * + * {
  margin-block-start: var(--rhythm);
}

.prose h2 {
  margin-block-start: calc(var(--rhythm) * 2);
  margin-block-end: calc(var(--rhythm) * 0.75);
}
```

Pick either paragraph spacing or first-line indentation. Do not use both. Most digital UI wants paragraph spacing; editorial long-form can use indentation when the typographic system supports it.

### Modular scale

A 5-size system covers most screens: caption, secondary, body, subheading, heading. Avoid muddy jumps like `14px`, `15px`, `16px`, `17px`.

```css
:root {
  --text-xs: 0.75rem;
  --text-sm: 0.875rem;
  --text-base: 1rem;
  --text-lg: 1.25rem;
  --text-xl: 1.563rem;
  --text-2xl: 1.953rem;
  --text-3xl: 2.441rem;
}

.text-caption { font-size: var(--text-xs); }
.text-secondary { font-size: var(--text-sm); }
.text-body { font-size: var(--text-base); }
.text-subhead { font-size: var(--text-lg); }
.text-heading { font-size: var(--text-2xl); }
```

Popular ratios: `1.125–1.2` for product UI, `1.25` for general use, `1.333` for stronger editorial hierarchy, and `1.5` for dramatic display work.

### Font selection and pairing

You often do not need a second font. One well-chosen family with multiple weights is cleaner than two weakly paired families. Add a second family only when it creates real contrast.

Good contrast pairs:

- Serif display + sans body.
- Condensed display + normal-width body.
- Geometric heading + humanist body.
- Mono accents + sans body, only when the product truly has a technical/data reason.

Avoid pairing two fonts that are similar but not identical. Two geometric sans-serifs usually feel like a mistake, not a system.

### Font loading policy

Prefer system fonts for product UI when performance and native feel matter. If the project already includes web fonts, keep loading conservative: use only the weights you actually need, avoid layout shift, and define a system fallback stack. Do not add a new font file just because the design feels generic; fix hierarchy first.

```css
:root {
  --font-product: -apple-system, BlinkMacSystemFont, "Segoe UI", system-ui, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
}

body {
  font-family: var(--font-product);
}
```

### OpenType polish

```css
body {
  font-kerning: normal;
  font-optical-sizing: auto;
}

.data-table,
.scoreboard,
.stat-list {
  font-variant-numeric: tabular-nums;
}

.fraction {
  font-variant-numeric: diagonal-fractions;
}

abbr,
.small-caps {
  font-variant-caps: all-small-caps;
  letter-spacing: 0.06em;
}

code,
.preformatted {
  font-variant-ligatures: none;
}
```

### Light text on dark backgrounds

Light text on dark surfaces often looks thinner than the same font on light surfaces. Compensate across multiple dimensions.

```css
.dark-prose {
  color: oklch(92% 0.01 145);
  line-height: 1.68;
  letter-spacing: 0.012em;
}

.dark-prose strong {
  font-weight: 650;
}
```

### Typography accessibility

- Body text should be at least `1rem`.
- Use `rem` for font sizes so browser preferences work.
- Never disable zoom.
- Keep long-form text around 45–75 characters per line.
- Uppercase labels need extra tracking.
- Do not use decorative/display fonts for body text.

### Avoid typographic slop

- Do not use monospace unless the content is actually code, data, or intentionally technical.
- Do not use tiny all-caps labels everywhere.
- Do not rely on gradient text as the main brand expression.
- Do not let paragraphs span the entire viewport.
- Do not use the same font size and weight for everything.

---

## Layout and Spacing

Space is a design material. Use it to group, separate, and create rhythm.

### Establish rhythm

- Tight spacing for related items: 8–12px.
- Medium spacing inside components: 16–24px.
- Generous spacing between sections: 48–144px.
- Not every gap should be equal.
- Use `gap` instead of margin stacks when possible.

```css
.stack {
  display: flex;
  flex-direction: column;
  gap: var(--stack-gap, var(--space-md));
}

.cluster {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: var(--space-sm);
}

.section {
  padding-block: var(--space-section);
}

.container {
  width: min(100% - 2rem, 72rem);
  margin-inline: auto;
}
```

### Use a 4pt base system

An 8pt-only system is often too coarse. A 4pt base gives useful intermediate values like 12px without becoming random.

```css
:root {
  --space-3xs: 0.125rem; /* 2px: rare optical correction */
  --space-2xs: 0.25rem;  /* 4px */
  --space-xs: 0.5rem;    /* 8px */
  --space-sm: 0.75rem;   /* 12px */
  --space-md: 1rem;      /* 16px */
  --space-lg: 1.5rem;    /* 24px */
  --space-xl: 2rem;      /* 32px */
  --space-2xl: 3rem;     /* 48px */
  --space-3xl: 4rem;     /* 64px */
  --space-4xl: 6rem;     /* 96px */
}
```

Name spacing semantically by relationship, not raw value. Prefer `--space-sm` or `--gap-card` over `--spacing-12`. Use `gap` for sibling spacing instead of margin cleanup hacks.

### Hierarchy through multiple dimensions

Do not rely on size alone. Strong hierarchy usually combines 2–3 of these at once:

| Tool | Strong hierarchy | Weak hierarchy |
|---|---|---|
| Size | 3:1 or more for major moments | tiny differences |
| Weight | bold vs regular | medium vs regular only |
| Color | clear contrast | similar tones everywhere |
| Position | top/left or centered intentionally | buried lower/right |
| Space | more air around priority | crowded primary element |

```css
.section-heading {
  max-width: 12ch;
  margin-block-end: var(--space-xl);
  font-size: var(--text-3xl);
  line-height: 0.95;
  letter-spacing: -0.06em;
}

.section-intro {
  max-width: 58ch;
  color: var(--color-muted);
  font-size: var(--text-lg);
}
```

### Cards are not required

Cards are useful for distinct, actionable, comparable content. They are noise when used for every paragraph. Never nest cards inside cards. Use spacing, alignment, typography, and subtle dividers before adding another box.

```css
.divided-list {
  display: grid;
}

.divided-list > * {
  padding-block: var(--space-lg);
  border-block-start: 1px solid var(--color-border);
}

.divided-list > :first-child {
  border-block-start: 0;
}
```

### Use Flexbox for 1D, Grid for 2D

```css
.nav-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--space-md);
}

.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(18rem, 100%), 1fr));
  gap: var(--space-lg);
}
```

### Use asymmetric compositions when they earn it

```css
.hero-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.1fr) minmax(18rem, 0.9fr);
  gap: clamp(2rem, 6vw, 7rem);
  align-items: center;
}

@media (max-width: 800px) {
  .hero-grid {
    grid-template-columns: 1fr;
  }
}
```

### Break card-grid monotony

Do not turn every section into equal cards. Mix structural patterns:

```css
.feature-list {
  display: grid;
  gap: var(--space-xl);
}

.feature-row {
  display: grid;
  grid-template-columns: 12rem minmax(0, 1fr);
  gap: var(--space-lg);
  padding-block: var(--space-xl);
  border-block-start: 1px solid var(--color-border);
}

@media (max-width: 700px) {
  .feature-row {
    grid-template-columns: 1fr;
  }
}
```

### Squint test

Blur your eyes. You should still see:

1. The primary element.
2. The secondary group.
3. The action path.
4. Clear group boundaries.

If the screen becomes a flat texture, the hierarchy is weak.

---

## Responsive Adaptation

### Breakpoints are not the design

Use content-driven breakpoints when the layout starts to break. Common ranges:

- Mobile: 320–767px.
- Tablet: 768–1023px.
- Desktop: 1024px+.

### Mobile adaptation

- Single column.
- Full-width components.
- Larger text and tap areas.
- Primary actions near thumb reach.
- No hover-dependent interactions.
- Progressive disclosure for secondary content.
- Minimum touch target: 44×44px.

```css
:where(button, a, input, select, textarea, [role="button"]) {
  min-block-size: 44px;
}

@media (max-width: 700px) {
  .desktop-columns {
    display: grid;
    grid-template-columns: 1fr;
  }

  .mobile-full-bleed {
    margin-inline: calc(var(--space-md) * -1);
    border-radius: 0;
  }
}
```

### Tablet adaptation

- Two-column layouts often work better than single or three-column.
- Support both touch and pointer.
- Keep controls large enough for touch.
- Use master-detail patterns when content calls for it.

```css
@media (min-width: 768px) and (max-width: 1023px) {
  .adaptive-panel {
    display: grid;
    grid-template-columns: minmax(14rem, 0.8fr) minmax(0, 1.2fr);
  }
}
```

### Desktop adaptation

- Use horizontal space, but do not stretch content endlessly.
- Side navigation can be persistent.
- Show more information upfront.
- Add hover states, but never rely on hover alone.

```css
.wide-shell {
  width: min(100% - 3rem, 96rem);
  margin-inline: auto;
}
```

### Container queries

Use container queries when a component should adapt to its own width, not the viewport.

```css
.card-shell {
  container-type: inline-size;
}

@container (min-width: 36rem) {
  .card-shell .card-content {
    display: grid;
    grid-template-columns: 10rem 1fr;
    gap: var(--space-lg);
  }
}
```

### Self-adjusting grids

Use `auto-fit` when the number of columns should adapt without a custom breakpoint.

```css
.auto-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(17.5rem, 100%), 1fr));
  gap: var(--space-lg);
}
```

### Optical alignment

Geometric alignment is not always perceived alignment. Icons, arrows, play buttons, and text edges may need small optical corrections.

```css
.icon-play {
  transform: translateX(0.06em);
}

.headline-optical-left {
  margin-inline-start: -0.05em;
}
```

### Touch target without visual bloat

A control can look compact while still exposing a 44px target.

```css
.icon-button {
  position: relative;
  inline-size: 1.5rem;
  block-size: 1.5rem;
}

.icon-button::before {
  content: "";
  position: absolute;
  inset: -0.625rem;
}
```

### Z-index scale

Never fight stacking with random `999999`. Create semantic levels.

```css
:root {
  --z-dropdown: 100;
  --z-sticky: 200;
  --z-backdrop: 300;
  --z-modal: 400;
  --z-toast: 500;
  --z-tooltip: 600;
}
```

### Mobile-first CSS

Start with the narrow/touch version, then add complexity with `min-width` queries. Desktop-first CSS often makes mobile inherit too many styles and then fight them.

```css
.responsive-layout {
  display: grid;
  gap: var(--space-md);
}

@media (min-width: 48rem) {
  .responsive-layout {
    grid-template-columns: 16rem minmax(0, 1fr);
    gap: var(--space-xl);
  }
}
```

### Breakpoints are not the design

Use content-driven breakpoints when the layout starts to fail. Three common starting points are `40rem`, `48rem`, and `64rem`, but content decides.

### Detect input method

Screen size does not tell you input method. A tablet can have a keyboard; a laptop can have touch.

```css
@media (pointer: coarse) {
  .button {
    min-block-size: 48px;
    padding-inline: 1.25rem;
  }
}

@media (pointer: fine) {
  .button {
    min-block-size: 40px;
  }
}

@media (hover: hover) and (pointer: fine) {
  .card-interactive:hover {
    transform: translateY(-2px);
  }
}

@media (hover: none) {
  .card-interactive:active {
    transform: scale(0.99);
  }
}
```

Never hide necessary information behind hover-only interactions.

### Safe areas

Phones can have notches, rounded corners, and home indicators. Respect safe areas for fixed or edge-touching UI.

```css
.app-frame {
  padding-block-start: env(safe-area-inset-top);
  padding-block-end: env(safe-area-inset-bottom);
  padding-inline-start: env(safe-area-inset-left);
  padding-inline-end: env(safe-area-inset-right);
}

.mobile-action-bar {
  padding-block-end: max(var(--space-md), env(safe-area-inset-bottom));
}
```

### Responsive tables

On narrow screens, dense tables often need a different shape instead of tiny text.

```css
@media (max-width: 44rem) {
  .responsive-table,
  .responsive-table thead,
  .responsive-table tbody,
  .responsive-table tr,
  .responsive-table th,
  .responsive-table td {
    display: block;
  }

  .responsive-table thead {
    position: absolute;
    inline-size: 1px;
    block-size: 1px;
    overflow: hidden;
    clip: rect(0 0 0 0);
  }

  .responsive-table tr {
    padding: var(--space-md);
    border: 1px solid var(--color-border);
    border-radius: var(--radius-lg);
    background: var(--color-surface);
  }

  .responsive-table td {
    display: grid;
    grid-template-columns: 9rem minmax(0, 1fr);
    gap: var(--space-sm);
  }

  .responsive-table td::before {
    content: attr(data-label);
    color: var(--color-muted);
    font-weight: 700;
  }
}
```

### Responsive media

CSS should reserve media space so layout does not jump.

```css
.responsive-media {
  aspect-ratio: var(--ratio, 16 / 9);
  overflow: hidden;
  border-radius: var(--radius-lg);
  background: var(--color-surface);
}

.responsive-media > img,
.responsive-media > video {
  inline-size: 100%;
  block-size: 100%;
  object-fit: cover;
}
```

For real image selection, use responsive image markup in HTML (`srcset`, `sizes`, or `picture`). CSS can shape the box; markup lets the browser choose the best asset.

### Real-device testing

DevTools is useful, but real devices catch touch behavior, browser chrome, keyboard overlap, font rendering, network delay, and cheap-phone performance issues.

---

## Components

### Buttons

Design all states: default, hover, focus, active, disabled, loading, success, error.

```css
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-xs);
  min-block-size: 44px;
  padding: 0.75rem 1rem;
  border: 1px solid transparent;
  border-radius: var(--radius-pill);
  font: inherit;
  font-weight: 700;
  line-height: 1;
  text-decoration: none;
  cursor: pointer;
  transition:
    transform var(--duration-fast) var(--ease-out-quart),
    background-color var(--duration-base) var(--ease-out-quart),
    border-color var(--duration-base) var(--ease-out-quart),
    color var(--duration-base) var(--ease-out-quart),
    box-shadow var(--duration-base) var(--ease-out-quart);
}

.button:hover {
  transform: translateY(-1px);
}

.button:active {
  transform: translateY(1px) scale(0.99);
}

.button:focus-visible {
  outline: 3px solid var(--color-accent);
  outline-offset: 3px;
}

.button:disabled,
.button[aria-disabled="true"] {
  cursor: not-allowed;
  opacity: 0.55;
  transform: none;
}

.button-primary {
  background: var(--color-primary);
  color: oklch(98% 0.006 145);
  box-shadow: var(--shadow-sm);
}

.button-primary:hover {
  background: var(--color-primary-strong);
  box-shadow: var(--shadow-md);
}

.button-secondary {
  background: var(--color-surface);
  color: var(--color-text);
  border-color: var(--color-border);
}

.button-ghost {
  background: transparent;
  color: var(--color-text);
}

.button-ghost:hover {
  background: color-mix(in oklch, var(--color-primary) 10%, transparent);
}
```

### Cards and containers

Cards should represent distinct, actionable, or meaningfully grouped content. Do not wrap everything in cards.

```css
.card {
  padding: clamp(1rem, 2vw, 1.5rem);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  background: var(--color-surface-raised);
  box-shadow: var(--shadow-sm);
}

.card-interactive {
  transition:
    transform var(--duration-base) var(--ease-out-quart),
    border-color var(--duration-base) var(--ease-out-quart),
    box-shadow var(--duration-base) var(--ease-out-quart);
}

.card-interactive:hover {
  transform: translateY(-3px);
  border-color: color-mix(in oklch, var(--color-primary) 50%, var(--color-border));
  box-shadow: var(--shadow-md);
}

.card-interactive:focus-within {
  outline: 3px solid var(--color-accent);
  outline-offset: 4px;
}
```

### Inputs and fields

Placeholders are not labels. Labels must remain visible.

```css
.field {
  display: grid;
  gap: var(--space-xs);
}

.field-label {
  font-size: var(--text-sm);
  font-weight: 700;
  color: var(--color-text);
}

.field-hint,
.field-error {
  font-size: var(--text-sm);
  line-height: 1.4;
}

.field-hint {
  color: var(--color-muted);
}

.field-error {
  color: var(--color-danger);
}

.input {
  width: 100%;
  min-block-size: 44px;
  padding: 0.75rem 0.9rem;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  background: var(--color-surface-raised);
  color: var(--color-text);
  font: inherit;
  transition:
    border-color var(--duration-base) var(--ease-out-quart),
    box-shadow var(--duration-base) var(--ease-out-quart),
    background-color var(--duration-base) var(--ease-out-quart);
}

.input::placeholder {
  color: color-mix(in oklch, var(--color-muted) 80%, var(--color-bg));
}

.input:hover {
  border-color: color-mix(in oklch, var(--color-primary) 45%, var(--color-border));
}

.input:focus {
  outline: none;
}

.input:focus-visible {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 4px color-mix(in oklch, var(--color-primary) 22%, transparent);
}

.input[aria-invalid="true"] {
  border-color: var(--color-danger);
  box-shadow: 0 0 0 4px color-mix(in oklch, var(--color-danger) 16%, transparent);
}
```

### Badges and chips

Use badges for status, metadata, filters, or categories. Do not make every label a rainbow.

```css
.badge {
  display: inline-flex;
  align-items: center;
  gap: 0.4em;
  width: fit-content;
  padding: 0.35rem 0.6rem;
  border: 1px solid color-mix(in oklch, var(--badge-color, var(--color-primary)) 35%, var(--color-border));
  border-radius: var(--radius-pill);
  background: color-mix(in oklch, var(--badge-color, var(--color-primary)) 12%, var(--color-surface));
  color: color-mix(in oklch, var(--badge-color, var(--color-primary)) 65%, var(--color-text));
  font-size: var(--text-xs);
  font-weight: 700;
}

.badge-success { --badge-color: var(--color-success); }
.badge-warning { --badge-color: var(--color-warning); }
.badge-danger { --badge-color: var(--color-danger); }
```

### Navigation

```css
.site-nav {
  position: sticky;
  top: 0;
  z-index: var(--z-sticky);
  background: color-mix(in oklch, var(--color-bg) 88%, transparent);
  backdrop-filter: blur(14px);
  border-block-end: 1px solid var(--color-border);
}

.nav-link {
  display: inline-flex;
  align-items: center;
  min-block-size: 44px;
  padding-inline: 0.75rem;
  border-radius: var(--radius-pill);
  color: var(--color-muted);
  text-decoration: none;
  transition:
    color var(--duration-base) var(--ease-out-quart),
    background-color var(--duration-base) var(--ease-out-quart);
}

.nav-link:hover,
.nav-link[aria-current="page"] {
  color: var(--color-text);
  background: color-mix(in oklch, var(--color-primary) 10%, transparent);
}

.nav-link:focus-visible {
  outline: 3px solid var(--color-accent);
  outline-offset: 3px;
}
```

### Skeleton loading

Skeletons communicate shape better than spinners.

```css
.skeleton {
  border-radius: var(--radius-md);
  background:
    linear-gradient(
      90deg,
      color-mix(in oklch, var(--color-surface) 92%, var(--color-text)) 0%,
      color-mix(in oklch, var(--color-surface) 80%, var(--color-text)) 45%,
      color-mix(in oklch, var(--color-surface) 92%, var(--color-text)) 100%
    );
  background-size: 220% 100%;
  animation: skeleton-sweep 1.2s var(--ease-in-out) infinite;
}

@keyframes skeleton-sweep {
  from { background-position: 100% 0; }
  to { background-position: -100% 0; }
}

@media (prefers-reduced-motion: reduce) {
  .skeleton {
    animation: none;
  }
}
```

---

## Interactive States

Every interactive element needs intentional state design.

| State | Purpose | CSS hook |
|---|---|---|
| Default | At rest | base selector |
| Hover | Pointer preview | `:hover` |
| Focus | Keyboard/programmatic focus | `:focus-visible` |
| Active | Press feedback | `:active` |
| Disabled | Not available | `:disabled`, `[aria-disabled="true"]` |
| Loading | Processing | `[aria-busy="true"]`, `.is-loading` |
| Error | Invalid or failed | `[aria-invalid="true"]`, `.is-error` |
| Success | Completed | `.is-success` |

### Focus rings

Never remove outlines without replacement.

```css
:where(a, button, input, select, textarea, [tabindex]):focus {
  outline: none;
}

:where(a, button, input, select, textarea, [tabindex]):focus-visible {
  outline: 3px solid var(--color-accent);
  outline-offset: 3px;
}
```

### Hover should not be required

```css
@media (hover: hover) and (pointer: fine) {
  .hover-lift:hover {
    transform: translateY(-2px);
  }
}
```

### Press feedback

```css
.pressable {
  transition: transform var(--duration-fast) var(--ease-out-quart);
}

.pressable:active {
  transform: scale(0.98);
}
```

---

## Motion and Animation

### The 100 / 300 / 500 rule

| Duration | Use |
|---:|---|
| 100–150ms | Button press, toggle, instant feedback |
| 200–300ms | Hover, tooltip, menu open, small state changes |
| 300–500ms | Modal, drawer, accordion, layout changes |
| 500–800ms | Page or hero entrance only |

Exit animations should be faster than entrances.

### Easing

Do not use default `ease` as a reflex.

```css
:root {
  --ease-enter: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-exit: cubic-bezier(0.7, 0, 0.84, 0);
  --ease-toggle: cubic-bezier(0.65, 0, 0.35, 1);
  --ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);
  --ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);
}
```

Avoid bounce and elastic curves unless the product is intentionally toy-like.

### CSS-only entrance

```css
.reveal-up {
  animation: reveal-up 520ms var(--ease-enter) both;
}

@keyframes reveal-up {
  from {
    opacity: 0;
    transform: translateY(18px);
    filter: blur(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
    filter: blur(0);
  }
}
```

### CSS-only stagger

```css
.stagger > * {
  animation: reveal-up 520ms var(--ease-enter) both;
  animation-delay: calc(var(--i, 0) * 60ms);
}
```

Use small stagger counts. Ten items at 60ms already adds 600ms.

### Accordions without animating height directly

```css
.disclosure-content {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--duration-slow) var(--ease-toggle);
}

.disclosure[open] .disclosure-content {
  grid-template-rows: 1fr;
}

.disclosure-content > * {
  overflow: hidden;
}
```

### Reduced motion

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }
}
```

### Motion rules

- Use transform and opacity for reliable movement.
- Use blur, masks, filters, and shadows only when the effect is contained and smooth.
- Avoid animating `width`, `height`, `top`, `left`, margins, and large layout-driving properties.
- Do not block the user with page-load choreography.
- Motion should clarify feedback, hierarchy, or relationship.

---

## Depth, Surfaces, and Texture

### Elevation

In light mode, shadows can show depth. In dark mode, use lighter surfaces.

```css
.surface-low {
  background: var(--color-surface);
}

.surface-raised {
  background: var(--color-surface-raised);
  box-shadow: var(--shadow-sm);
}

.surface-floating {
  background: var(--color-surface-raised);
  box-shadow: var(--shadow-lg);
}
```

### Atmospheric texture

Use texture sparingly. It should support the brand, not obscure content.

```css
.noisy-surface {
  position: relative;
  isolation: isolate;
}

.noisy-surface::before {
  content: "";
  position: absolute;
  inset: 0;
  z-index: -1;
  pointer-events: none;
  opacity: 0.18;
  background-image:
    radial-gradient(circle at 20% 10%, color-mix(in oklch, var(--color-primary) 20%, transparent), transparent 28rem),
    radial-gradient(circle at 85% 20%, color-mix(in oklch, var(--color-accent) 16%, transparent), transparent 22rem);
}
```

### Borders

Prefer full hairline borders or subtle background washes over thick side stripes.

```css
.callout {
  border: 1px solid color-mix(in oklch, var(--color-primary) 45%, var(--color-border));
  background: color-mix(in oklch, var(--color-primary) 8%, var(--color-surface));
  border-radius: var(--radius-lg);
  padding: var(--space-lg);
}
```

---

## Quieting a Loud Interface

Quiet design is not weak design. It is design where fewer elements compete for attention. Use this pass when a page feels too saturated, too animated, too decorated, or visually exhausting.

### Find the intensity source

Common sources of loudness:

- Too many saturated colors.
- Too many high-contrast elements.
- Heavy shadows or glows everywhere.
- Patterns behind important text.
- Multiple competing borders.
- All headings set huge and bold.
- Decorative motion that does not explain state.
- Every card using a background, border, shadow, icon, and badge.

### Reduce color intensity

```css
/* Loud */
:root {
  --color-primary: oklch(65% 0.25 145);
}

/* Quieter */
:root {
  --color-primary: oklch(58% 0.14 145);
  --color-primary-soft: oklch(92% 0.035 145);
}
```

Use color as accent, not wallpaper, unless the page intentionally uses a drenched brand direction. A practical rule: let neutrals carry 80–90% of a product UI and reserve saturated color for actions, selection, and important state.

### Reduce visual weight

```css
.quieter-card {
  border: 1px solid color-mix(in oklch, var(--color-border) 70%, transparent);
  background: color-mix(in oklch, var(--color-surface) 88%, var(--color-bg));
  box-shadow: none;
}

.quieter-heading {
  font-weight: 650;
  letter-spacing: -0.025em;
}

.quieter-muted {
  color: color-mix(in oklch, var(--color-text) 62%, var(--color-bg));
}
```

Reduce heavy weights before shrinking everything. A loud `900` heading may become refined at `650` while keeping the same hierarchy.

### Simplify effects

```css
/* Replace dramatic glow with a controlled border wash */
.refined-callout {
  border: 1px solid color-mix(in oklch, var(--color-primary) 32%, var(--color-border));
  background: color-mix(in oklch, var(--color-primary) 7%, var(--color-surface));
}
```

Remove effects that do not serve grouping, state, or brand memory. Quiet does not mean grayscale; it means fewer things shouting at once.

### Reduce motion intensity

```css
.subtle-enter {
  animation: subtle-enter 220ms var(--ease-out-quart) both;
}

@keyframes subtle-enter {
  from {
    opacity: 0;
    transform: translateY(8px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
```

Prefer shorter distances, shorter durations, and no bounce. Keep state motion; cut decorative motion.

### Verify quieter quality

After quieting a design, check:

- Is the primary action still obvious?
- Is the design easier to read for longer?
- Does the brand character still survive?
- Are controls still visibly interactive?
- Did you remove noise rather than remove hierarchy?

---

## Hardening for Real Content

### Long text

```css
.truncate {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 3;
  overflow: hidden;
}

.wrap-anywhere {
  overflow-wrap: anywhere;
  word-break: normal;
}
```

### Flex and grid overflow

```css
.flex-child,
.grid-child {
  min-width: 0;
  min-height: 0;
}
```

### Internationalization

Use logical properties so layouts work better across writing directions.

```css
.logical-card {
  padding-block: var(--space-lg);
  padding-inline: var(--space-lg);
  border-inline-start: 1px solid var(--color-border);
  margin-block-end: var(--space-xl);
}

[dir="rtl"] .flip-in-rtl {
  transform: scaleX(-1);
}
```

Budget 30–40% more room for translated strings.

```css
.action-row {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-sm);
}

.action-row > * {
  flex: 0 1 auto;
}
```

### Empty, loading, error, success

```css
.state-panel {
  display: grid;
  justify-items: start;
  gap: var(--space-sm);
  padding: var(--space-xl);
  border: 1px dashed var(--color-border);
  border-radius: var(--radius-lg);
  background: color-mix(in oklch, var(--color-surface) 70%, transparent);
}

.state-panel[data-state="error"] {
  border-color: color-mix(in oklch, var(--color-danger) 60%, var(--color-border));
  background: color-mix(in oklch, var(--color-danger) 8%, var(--color-surface));
}

.state-panel[data-state="success"] {
  border-color: color-mix(in oklch, var(--color-success) 60%, var(--color-border));
  background: color-mix(in oklch, var(--color-success) 8%, var(--color-surface));
}
```

### Avoid fragile assumptions

Test with:

- 0 items.
- 1 item.
- 1000 items.
- Very long names.
- Emoji.
- Accents and non-Latin scripts.
- Narrow screens.
- Browser zoom at 200%.
- Slow loading.
- Keyboard-only navigation.

---

## UX Writing for Interface States

CSS cannot fix unclear copy. Buttons, errors, empty states, and labels are part of the interface. Style them as real components and write them as real actions.

### Button labels

Avoid vague labels like “OK,” “Submit,” “Yes,” and “Click here.” Use verb + object labels.

| Weak | Strong |
|---|---|
| OK | Save changes |
| Submit | Create account |
| Yes | Delete message |
| Cancel | Keep editing |
| Click here | Download PDF |

CSS can support clearer labels by allowing buttons to grow and wrap instead of forcing cryptic text.

```css
.button {
  min-inline-size: max-content;
  max-inline-size: 100%;
  white-space: normal;
  text-align: center;
}

.action-row {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-sm);
}
```

### Error message pattern

A useful error says:

1. What happened.
2. Why it happened, if known.
3. How to fix it.

```css
.form-error {
  display: flex;
  align-items: flex-start;
  gap: var(--space-xs);
  color: var(--color-danger);
  font-size: var(--text-sm);
  line-height: 1.45;
}

.form-error::before {
  content: "!";
  display: inline-grid;
  place-items: center;
  flex: 0 0 1.25rem;
  block-size: 1.25rem;
  border-radius: 50%;
  background: color-mix(in oklch, var(--color-danger) 14%, transparent);
  font-weight: 800;
}
```

Write: “Email address needs an @ symbol.” Not: “Invalid input.”

### Empty states

A good empty state is short onboarding:

- Acknowledge the state.
- Explain the value of filling it.
- Provide one clear next action.

```css
.empty-state-title {
  margin: 0;
  font-size: var(--text-xl);
  line-height: 1.1;
}

.empty-state-copy {
  max-width: 46ch;
  margin: 0;
  color: var(--color-muted);
}

.empty-state-actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: var(--space-sm);
}
```

### Loading copy

Be specific. “Saving your draft…” is better than “Loading…”. For long waits, show progress or set expectations.

```css
.loading-row {
  display: inline-flex;
  align-items: center;
  gap: var(--space-sm);
  color: var(--color-muted);
  font-size: var(--text-sm);
}
```

### Confirmation dialogs

Most confirmation dialogs should be replaced by undo. When confirmation is necessary, name the consequence.

```css
.confirmation-dialog {
  max-inline-size: min(100% - 2rem, 32rem);
  padding: var(--space-xl);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xl);
  background: var(--color-surface-raised);
  box-shadow: var(--shadow-lg);
}

.confirmation-dialog[data-danger="true"] {
  border-color: color-mix(in oklch, var(--color-danger) 38%, var(--color-border));
}
```

Use labels like “Delete project” and “Keep project,” not “Yes” and “No.”

### Terminology consistency

Pick one term and keep it everywhere: “Sign in,” “Settings,” “Delete,” “Create.” Variety in core nouns and actions creates confusion.

---

## Onboarding and Empty States

Onboarding should get users to first value, not teach everything.

### Empty state formula

A good empty state answers:

1. What will appear here?
2. Why does it matter?
3. What should I do next?
4. Is there a lightweight example or template?

```css
.empty-state {
  display: grid;
  place-items: center;
  gap: var(--space-md);
  min-block-size: min(32rem, 70vh);
  padding: var(--space-2xl);
  text-align: center;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-xl);
  background:
    radial-gradient(circle at 50% 0%, color-mix(in oklch, var(--color-primary) 12%, transparent), transparent 22rem),
    var(--color-surface);
}

.empty-state > * {
  max-width: 42rem;
}
```

### Onboarding CSS principles

- Make skip actions visible.
- Do not trap users in full-screen tutorials.
- Use progress indicators for multi-step setup.
- Put help where decisions happen.
- Use empty states as gentle onboarding.

```css
.progress-steps {
  display: flex;
  flex-wrap: wrap;
  gap: var(--space-xs);
}

.progress-step {
  flex: 1 1 8rem;
  min-block-size: 0.5rem;
  border-radius: var(--radius-pill);
  background: var(--color-border);
}

.progress-step[aria-current="step"],
.progress-step[data-complete="true"] {
  background: var(--color-primary);
}
```

---

## Delight Without Noise

Delight should amplify usability, not block it.

### Good delight moments

- Successful completion.
- First-time empty states.
- Milestones.
- Error recovery.
- Hover or press feedback.
- Small discovery moments.

### Bad delight moments

- Critical errors treated as jokes.
- Animations that delay task completion.
- Confetti for trivial actions.
- The same playful message repeated forever.
- Visual surprises that reduce clarity.

### CSS-only button delight

```css
.button-delight {
  position: relative;
  overflow: hidden;
}

.button-delight::after {
  content: "";
  position: absolute;
  inset: auto auto 50% 50%;
  width: 0;
  aspect-ratio: 1;
  border-radius: 50%;
  background: color-mix(in oklch, white 24%, transparent);
  transform: translate(-50%, 50%);
  opacity: 0;
}

.button-delight:active::after {
  width: 140%;
  opacity: 1;
  transition:
    width 260ms var(--ease-out-quart),
    opacity 360ms var(--ease-out-quart);
}
```

### Success pulse

```css
.success-pulse {
  animation: success-pulse 520ms var(--ease-out-quart) both;
}

@keyframes success-pulse {
  0% { transform: scale(0.96); opacity: 0.6; }
  55% { transform: scale(1.03); opacity: 1; }
  100% { transform: scale(1); opacity: 1; }
}
```

---

## Performance-Oriented CSS

### Avoid layout thrashing by design

CSS choices that help rendering:

- Animate `transform` and `opacity` for movement.
- Avoid animating layout properties casually.
- Use `content-visibility` for long below-fold content.
- Use `contain` for isolated regions.
- Reserve media space with `aspect-ratio`.
- Use lazy loading for images in markup when possible.

```css
.long-section {
  content-visibility: auto;
  contain-intrinsic-size: 800px;
}

.independent-widget {
  contain: layout paint;
}

.media-frame {
  aspect-ratio: 16 / 9;
  overflow: hidden;
  border-radius: var(--radius-lg);
  background: var(--color-surface);
}

.media-frame > img,
.media-frame > video {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
```

### `will-change` rule

Do not apply `will-change` globally. Use it only when animation is imminent.

```css
.lift-on-hover:hover {
  will-change: transform;
}
```

### Core Web Vitals CSS helpers

```css
img,
video,
canvas,
svg {
  max-width: 100%;
  height: auto;
}

img[width][height] {
  height: auto;
}

.no-layout-shift-media {
  aspect-ratio: var(--media-ratio, 16 / 9);
}
```

---

## Accessibility CSS

### Screen-reader-only content

```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.sr-only-focusable:focus,
.sr-only-focusable:focus-visible {
  position: static;
  width: auto;
  height: auto;
  margin: 0;
  overflow: visible;
  clip: auto;
  white-space: normal;
}
```

### Skip links

```css
.skip-link {
  position: fixed;
  inset-block-start: var(--space-sm);
  inset-inline-start: var(--space-sm);
  z-index: var(--z-tooltip);
  padding: var(--space-sm) var(--space-md);
  border-radius: var(--radius-md);
  background: var(--color-text);
  color: var(--color-bg);
  transform: translateY(-150%);
  transition: transform var(--duration-base) var(--ease-out-quart);
}

.skip-link:focus-visible {
  transform: translateY(0);
}
```

### High contrast support

```css
@media (forced-colors: active) {
  :where(button, a, input, select, textarea):focus-visible {
    outline: 2px solid Highlight;
    outline-offset: 3px;
  }

  .card,
  .input,
  .button {
    border: 1px solid CanvasText;
    box-shadow: none;
  }
}
```

### Color is not enough

Pair color with shape, text, icon, border, or pattern.

```css
.status::before {
  content: "";
  display: inline-block;
  width: 0.65em;
  aspect-ratio: 1;
  margin-inline-end: 0.45em;
  border-radius: 50%;
  background: currentColor;
}

.status-error {
  color: var(--color-danger);
}

.status-error::before {
  border-radius: 0.2em;
}
```

---

## Print and Low-Context Adaptation

Print should remove interactive clutter and preserve content.

```css
@media print {
  :root {
    --color-bg: white;
    --color-surface: white;
    --color-text: black;
    --color-muted: #444;
    --color-border: #bbb;
  }

  body {
    background: white;
    color: black;
    font-size: 12pt;
  }

  nav,
  .site-nav,
  .no-print,
  button,
  .button {
    display: none !important;
  }

  a[href]::after {
    content: " (" attr(href) ")";
    font-size: 0.85em;
  }

  .card,
  .section {
    break-inside: avoid;
    box-shadow: none;
  }

  h1,
  h2,
  h3 {
    break-after: avoid;
  }
}
```

---

## Audit Checklists

### Cognitive load checklist

Count failures:

- Is there one clear focus?
- Is information chunked into small groups?
- Are related items visually grouped?
- Is the hierarchy obvious within two seconds?
- Is the user asked to make one decision at a time?
- Are there fewer than five visible choices at a decision point?
- Does the user avoid remembering information from another screen?
- Is complexity disclosed progressively?

Scoring:

- 0–1 failures: good.
- 2–3 failures: moderate load.
- 4+ failures: critical load problem.

### Nielsen-style usability scoring

Score each from 0–4:

1. Visibility of system status.
2. Match between system and real world.
3. User control and freedom.
4. Consistency and standards.
5. Error prevention.
6. Recognition rather than recall.
7. Flexibility and efficiency.
8. Aesthetic and minimalist design.
9. Error recognition and recovery.
10. Help and documentation.

Total:

- 36–40: excellent.
- 28–35: good.
- 20–27: acceptable.
- 10–19: weak.
- 0–9: critical.

### Technical audit dimensions

Score 0–4:

| Dimension | Check |
|---|---|
| Accessibility | Contrast, focus, semantics, keyboard support, labels |
| Performance | image sizing, content visibility, animation cost, layout shifts |
| Theming | token usage, dark mode, hard-coded colors |
| Responsive | mobile layout, overflow, touch targets, text scaling |
| Anti-patterns | AI slop tells, nested cards, generic gradients, weak hierarchy |

### Persona checks

Use 2–3 personas depending on the interface.

#### Power user

- Can the core task be done quickly?
- Are repetitive steps minimized?
- Can they skip hand-holding?
- Is keyboard navigation supported?

#### First-timer

- Is the first action clear in five seconds?
- Are icons labeled?
- Is terminology plain?
- Is success confirmed?

#### Accessibility-dependent user

- Can the primary flow be completed with keyboard only?
- Are focus indicators visible?
- Does text meet contrast requirements?
- Are state changes announced in the UI?

#### Stress tester

- What happens with empty, huge, weird, or long input?
- Does refresh preserve state where appropriate?
- Do errors recover gracefully?

#### Distracted mobile user

- Are actions thumb reachable?
- Are tap targets at least 44×44px?
- Is progress preserved after interruption?
- Is typing minimized?

### Severity labels

- P0: prevents task completion.
- P1: major usability or WCAG issue.
- P2: minor problem with workaround.
- P3: polish issue.

---

## Polish and Design-System Alignment

Polish is the final pass, not the first pass. Do it after the surface works and the main structure is correct.

### Align to the system first

Before polishing individual pixels, identify the existing system:

- Color tokens.
- Spacing scale.
- Typography scale.
- Component vocabulary.
- Motion conventions.
- Form patterns.
- Empty/loading/error state patterns.

Then classify drift by root cause:

| Drift type | Meaning | Fix |
|---|---|---|
| Missing token | Value should exist but does not | add or map a token |
| One-off implementation | Shared component exists but was not used | swap to shared component |
| Conceptual mismatch | Flow or hierarchy differs from nearby features | reshape the experience |

### Polish dimensions

Check these systematically:

- Visual alignment: grid, edges, optical centering.
- Spacing: all gaps use the scale.
- Hierarchy: primary/secondary/tertiary are obvious.
- Typography: consistent sizes, weights, line lengths.
- Color: contrast, semantic meaning, no random accents.
- Interaction states: default, hover, focus, active, disabled, loading, error, success.
- Motion: purposeful, fast enough, reduced-motion safe.
- Copy: clear labels, consistent terminology, no vague errors.
- Forms: labels, hints, validation, tab order.
- Edge cases: empty, huge, missing, long, weird, offline if relevant.
- Responsiveness: real mobile, tablet, desktop behavior.
- Performance: no layout shift, no expensive unbounded effects.

### Polish CSS snippets

```css
/* Prevent media layout shift */
.polished-media {
  aspect-ratio: var(--ratio, 16 / 9);
  overflow: hidden;
  background: var(--color-surface);
}

/* Keep long labels from breaking cards */
.polished-text {
  overflow-wrap: anywhere;
}

/* Consistent icon sizing */
.icon {
  inline-size: 1em;
  block-size: 1em;
  flex: 0 0 auto;
}

/* Proper interactive disabled state */
[aria-disabled="true"],
:disabled {
  cursor: not-allowed;
}
```

### Final polish checklist

- [ ] The surface matches the design system or intentionally defines a new one.
- [ ] Spacing uses tokens.
- [ ] Typography hierarchy is consistent.
- [ ] Body copy has comfortable measure.
- [ ] All interactive states exist.
- [ ] Focus indicators are visible.
- [ ] Touch targets are at least 44×44px where needed.
- [ ] Errors explain recovery.
- [ ] Empty states guide the next action.
- [ ] Loading states preserve layout shape.
- [ ] Long content does not overflow.
- [ ] Translated content has room to expand.
- [ ] Motion respects `prefers-reduced-motion`.
- [ ] No horizontal scroll on mobile.
- [ ] No hard-coded one-off colors or spacing values.
- [ ] No dead CSS, commented-out experiments, or unused classes.

---

## Ready-to-Paste CSS Starter

This starter combines the safest defaults from the guide.

```css
:root {
  color-scheme: light dark;

  --color-bg: oklch(96% 0.012 145);
  --color-surface: oklch(91% 0.018 145);
  --color-surface-raised: oklch(94% 0.014 145);
  --color-text: oklch(18% 0.035 145);
  --color-muted: oklch(43% 0.035 145);
  --color-border: oklch(78% 0.025 145);
  --color-primary: oklch(62% 0.16 145);
  --color-primary-strong: oklch(48% 0.18 145);
  --color-accent: oklch(76% 0.15 78);
  --color-danger: oklch(58% 0.2 25);
  --color-success: oklch(58% 0.14 150);
  --color-warning: oklch(72% 0.16 75);

  --space-xs: 0.5rem;
  --space-sm: 0.75rem;
  --space-md: 1rem;
  --space-lg: 1.5rem;
  --space-xl: 2rem;
  --space-2xl: 3rem;
  --space-section: clamp(4rem, 8vw, 9rem);

  --radius-sm: 0.375rem;
  --radius-md: 0.75rem;
  --radius-lg: 1.25rem;
  --radius-pill: 999px;

  --text-base: clamp(1rem, 0.96rem + 0.25vw, 1.125rem);
  --text-lg: clamp(1.125rem, 1.04rem + 0.45vw, 1.35rem);
  --text-xl: clamp(1.35rem, 1.1rem + 1.1vw, 2rem);
  --text-2xl: clamp(2rem, 1.4rem + 3vw, 4rem);
  --text-3xl: clamp(3rem, 1.9rem + 5.4vw, 7rem);

  --shadow-sm: 0 1px 2px oklch(18% 0.035 145 / 0.08);
  --shadow-md: 0 1rem 3rem oklch(18% 0.035 145 / 0.12);

  --duration-fast: 120ms;
  --duration-base: 220ms;
  --duration-slow: 420ms;
  --ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);
  --ease-toggle: cubic-bezier(0.65, 0, 0.35, 1);
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: oklch(14% 0.018 145);
    --color-surface: oklch(18% 0.02 145);
    --color-surface-raised: oklch(23% 0.024 145);
    --color-text: oklch(92% 0.01 145);
    --color-muted: oklch(72% 0.014 145);
    --color-border: oklch(32% 0.026 145);
    --color-primary: oklch(70% 0.13 145);
  }
}

*,
*::before,
*::after {
  box-sizing: border-box;
}

html {
  min-block-size: 100%;
  scroll-behavior: smooth;
}

body {
  min-block-size: 100%;
  margin: 0;
  background: var(--color-bg);
  color: var(--color-text);
  font-family: ui-sans-serif, system-ui, sans-serif;
  font-size: var(--text-base);
  line-height: 1.6;
  text-rendering: optimizeLegibility;
}

img,
picture,
svg,
video,
canvas {
  display: block;
  max-width: 100%;
}

button,
input,
select,
textarea {
  font: inherit;
}

:where(button, a, input, select, textarea, [role="button"]) {
  min-block-size: 44px;
}

:where(a, button, input, select, textarea, [tabindex]):focus {
  outline: none;
}

:where(a, button, input, select, textarea, [tabindex]):focus-visible {
  outline: 3px solid var(--color-accent);
  outline-offset: 3px;
}

.container {
  width: min(100% - 2rem, 72rem);
  margin-inline: auto;
}

.section {
  padding-block: var(--space-section);
}

.stack {
  display: flex;
  flex-direction: column;
  gap: var(--stack-gap, var(--space-md));
}

.cluster {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: var(--space-sm);
}

.grid-auto {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(18rem, 100%), 1fr));
  gap: var(--space-lg);
}

.card {
  padding: clamp(1rem, 2vw, 1.5rem);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-lg);
  background: var(--color-surface-raised);
  box-shadow: var(--shadow-sm);
}

.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--space-xs);
  padding: 0.75rem 1rem;
  border: 1px solid transparent;
  border-radius: var(--radius-pill);
  background: var(--color-primary);
  color: oklch(98% 0.006 145);
  font-weight: 700;
  line-height: 1;
  text-decoration: none;
  cursor: pointer;
  transition:
    transform var(--duration-fast) var(--ease-out-quart),
    background-color var(--duration-base) var(--ease-out-quart),
    box-shadow var(--duration-base) var(--ease-out-quart);
}

.button:hover {
  transform: translateY(-1px);
  background: var(--color-primary-strong);
  box-shadow: var(--shadow-md);
}

.button:active {
  transform: translateY(1px) scale(0.99);
}

.button:disabled,
.button[aria-disabled="true"] {
  cursor: not-allowed;
  opacity: 0.55;
  transform: none;
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    scroll-behavior: auto !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Final Quality Bar

Before calling a CSS system “done,” verify:

- The first action is clear within five seconds.
- Body text passes contrast.
- Focus states are visible.
- Touch targets are at least 44×44px.
- Mobile does not rely on hover.
- Long text does not break layout.
- Empty, loading, error, and success states exist.
- Motion respects reduced-motion preferences.
- Colors have roles, not random decoration.
- The page does not look like a generic AI-generated landing page.
- The design still works at 200% zoom.
- The layout has rhythm: tight groups and generous separations.
- Components use tokens instead of hard-coded one-offs.
- Every visual effect earns its place.

- Product UI chooses familiarity over decorative novelty.
- Brand UI has a memorable point of view without blocking comprehension.
- Typography uses a deliberate scale, not arbitrary close sizes.
- The interface has been tested against loudness: color, weight, motion, effects, and density.
- UX copy names actions and recovery paths clearly.
