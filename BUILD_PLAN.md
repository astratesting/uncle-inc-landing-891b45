# Uncle Inc. — Landing Page Build Plan

## 1. PRODUCT

Uncle Inc. is a marketing landing page for an AI-assisted MVP platform aimed at pre-launch startup founders. The page's single job: convert a cold visitor into a waitlist signup. The hero lead — "Validate Your Startup Idea Before You Build It" — and the supporting sections (six feature cards, three pricing tiers, six-item FAQ, email waitlist, footer) are the conversion machinery. The visitor reads top-to-bottom, understands the value in under 10 seconds from the hero, scans features to confirm fit, checks pricing for affordability, resolves objections in FAQ, and submits an email. The page is a single Next.js 14 route (`/`) composed of sticky nav, hero, features, pricing, FAQ, CTA, and footer — no app, no auth, no dashboard in this build.

## 2. WHO IT'S FOR

- **ICP:** solo technical founders and first-time technical co-founders in the 0-to-1 phase, evaluating multiple product ideas before committing engineering time. They are skeptical of vague AI hype and want concrete mechanisms (scoring, validation, mockups) they can act on. They are time-poor, scan-first readers.
- **How that shapes the product and tone:** the page must lead with the highest-stakes pain (wasting weeks on a bad idea), use precise operator language ("validate", "score", "scaffold"), keep copy tight (no filler, no invented customer quotes, no fake logos), and put the email field within easy reach of every section (nav, hero, mid-page CTA, bottom CTA). The "Crisp Operator" archetype from the design session — clean enterprise UI, precise grids, confident whitespace, 4px spacing — fits this ICP exactly.

## 3. LOOK & FEEL

### 3.1 Visual system

- **Vibe/positioning:** Crisp Operator. Enterprise-clean, calm, confident, switchboard motif (signals routing ideas to validated output). Anti-fluff, anti-hype.
- **Color palette (used as Tailwind tokens, locked in `tailwind.config.ts`):**
  - `navy` `#1A3A5C` — primary text, dark backgrounds, wordmark
  - `cobalt` `#4A90D9` — links, primary buttons, icon accents
  - `green` `#22C55E` — CTA accent, positive indicators, "validate" highlights
  - `bg` `#F8FAFC` — page background
  - `surface` `#FFFFFF` — cards, elevated surfaces
  - `ink` `#0F172A` — primary text on light
  - `muted` `#64748B` — secondary text
  - `border` `#E2E8F0` — hairline borders
- **Typography:**
  - Headings: **Inter** (700/600), tight tracking, large display sizes (40–72px hero)
  - Body: **IBM Plex Sans** (400/500), comfortable reading size (16–18px, line-height 1.6)
  - Mono accents (for codes, plan names): **Source Code Pro** (loaded but used sparingly)
  - Both loaded via `next/font/google` in `app/layout.tsx`, exposed as CSS variables `--font-inter` and `--font-plex`
- **Spacing/layout:** 4px base unit. `container` is 1280px max with 24px gutter. Section vertical rhythm: 96px desktop / 64px mobile. 12-column grid.
- **Radius:** 4px default (rounded), 8px on cards, full (rounded-full) on pills and the email input button.
- **Shadows:** `shadow-sm` on cards, `shadow-md` on the sticky nav after scroll, no heavy drop shadows.
- **Iconography:** Lucide-style outline icons, 1.5px stroke, 20–24px, navy or cobalt. No emoji.
- **Imagery:** none on the landing page in this build — abstract SVG motif (switchboard / node-link diagram) used as a decorative hero accent only.
- **Interaction/motion:** subtle only. (a) Nav gains shadow on scroll via a small client effect. (b) FAQ items rotate chevron 180° when open. (c) Buttons darken 1 step on hover. (d) Feature cards lift 2px (`-translate-y-0.5`) on hover. No scroll-jacking, no parallax, no animated gradients.

### 3.2 Screen-by-screen layout (top to bottom of `/`)

**`components/Nav.tsx` — sticky top bar**
- 64px tall, white surface, 1px bottom border (`border-border`), becomes `shadow-md` after 8px scroll.
- Left: Uncle Inc. wordmark (navy text with a small green dot over the "i" in "Inc") + tiny "AI-assisted MVP platform" eyebrow.
- Right (desktop): anchor links — Features · Pricing · FAQ — in `text-ink` 14px, hover cobalt.
- Right: primary "Join waitlist" button (cobalt background, white text, 36px tall, rounded-full), anchors to `#waitlist`.
- Mobile: same wordmark left, hamburger right that toggles a full-width dropdown panel with the 3 links stacked + the CTA button full-width.

**`components/Hero.tsx` — above the fold**
- 96px top padding (clears sticky nav), 96px bottom.
- Two-column on desktop (60/40), single-column on mobile.
- Left column:
  - Eyebrow chip: green dot + "For pre-launch founders" in 12px uppercase tracking-wider, `text-muted`.
  - H1: "Validate Your Startup Idea Before You Build It" — Inter 700, 56px desktop / 36px mobile, `text-ink`, `leading-[1.05]`.
  - Sub: "Uncle scores your idea against real market signals, scaffolds an MVP plan, and generates clickable mockups — in under 10 minutes." — IBM Plex Sans 18px, `text-muted`, max-w-[560px].
  - Inline waitlist form: email input (h-12, rounded-full, border, `text-ink`, placeholder "you@startup.com") + green submit button "Get early access" (h-12, rounded-full, px-6, white text). On submit: POST to `/api/waitlist`, show inline "Thanks — you're on the list." message.
  - Below form: micro-copy "No credit card. Limited founding-member spots." in 12px `text-muted`.
- Right column:
  - A 480×420 abstract SVG (switchboard motif: 4 nodes connected by cobalt lines to a central green node, navy stroke). No fake product screenshot.

**`components/Features.tsx` — 6 feature cards**
- Section header: "Everything you need to go from idea to MVP." (Inter 600, 32px, centered). Subline: "Six focused tools. One workspace. No 12-month roadmap." (Plex 16px, muted, centered).
- 3-column grid on desktop, 2 on tablet, 1 on mobile, 24px gap.
- Each card: white surface, `border border-border`, `rounded-lg`, p-8, hover lift.
  - 1: "Idea scoring" — Lucide `gauge` icon, navy. Body: "Rank your idea on demand, competition, and feasibility. See the score in one number." Green badge: "0–100".
  - 2: "Competitor map" — Lucide `network` icon. Body: "Auto-generated map of the players in your space with positioning gaps highlighted."
  - 3: "MVP plan" — Lucide `list-checks` icon. Body: "A week-by-week build plan tuned to your stack, skills, and runway."
  - 4: "Clickable mockups" — Lucide `layout-template` icon. Body: "Generate wireframes for every core screen. Share a link, collect feedback."
  - 5: "User interview kit" — Lucide `messages-square` icon. Body: "10 tailored questions, a screening script, and a synthesis template."
  - 6: "Founder dashboard" — Lucide `kanban-square` icon. Body: "Track validation progress, blockers, and next actions in one view."

**`components/Pricing.tsx` — 3 tiers**
- Section header: "Simple pricing. No surprises." Inter 600, 32px, centered.
- 3-column grid, middle card (Builder) visually elevated: `border-cobalt`, `shadow-sm`, `-mt-4`, green "Most popular" pill at top.
- Cards share: white surface, `rounded-lg`, p-8, 1px border.
  - **Solo — Free.** $0/mo. Audience: "Curious founders." Features list (3 items): "1 idea validation", "Idea score report", "Email support". CTA: "Start free" → outline cobalt button, links to `#waitlist`.
  - **Builder — $29/mo.** "Most popular" pill (green). Audience: "First-time founders shipping an MVP." Features (6 items): "Unlimited idea validations", "Competitor maps", "Week-by-week MVP plan", "Clickable mockups", "User interview kit", "Email support". CTA: "Join waitlist" → solid cobalt button, links to `#waitlist`.
  - **Studio — $99/mo.** Audience: "Small founding teams." Features (7 items, all Builder features +): "Up to 5 seats", "Shared founder dashboard", "Priority support". CTA: "Talk to us" → outline cobalt button, mailto link.
- All prices displayed as "Coming soon" eyebrow above each card body to be honest that the SaaS is not yet launched (no invented launch date).

**`components/FAQ.tsx` — 6 expandable items**
- Section header: "Questions, answered." Inter 600, 32px, centered.
- Single-column list, max-w-[768px] centered, 8px gap between items.
- Each item: white surface, 1px border, `rounded-lg`, button toggles open state. Closed: question + chevron-right. Open: question + chevron-down, body paragraph below in `text-muted`. Smooth `transition-all`.
- Items:
  1. "What does Uncle actually do?" → "Uncle turns a one-paragraph idea into a validation package: a 0–100 score, a competitor map, a week-by-week MVP plan, clickable mockups, and a user interview kit."
  2. "Who is Uncle for?" → "Solo technical founders and first-time technical co-founders in the 0-to-1 phase, evaluating one or more ideas before committing engineering time."
  3. "How is this different from ChatGPT?" → "ChatGPT is a general assistant. Uncle is a structured workflow: it runs the same validation steps every time, with outputs you can share, compare, and act on."
  4. "Do I need to know how to code?" → "No. The MVP plan adapts to your skill level. If you can describe what you want to build, Uncle can scaffold it."
  5. "When will the product launch?" → "We're onboarding founding members from the waitlist in small cohorts. Joining the waitlist puts you in line."
  6. "Can I cancel anytime?" → "Yes. Builder and Studio are month-to-month. Cancel from your account settings."

**`components/CTA.tsx` — waitlist section**
- Navy background (`bg-navy`), white text, 96px vertical padding.
- Centered, max-w-[640px].
- H2: "Get early access." Inter 600, 40px, white.
- Sub: "Join the waitlist and we'll email you when your cohort opens." Plex 18px, `text-white/70`.
- Waitlist form (same as hero): email input + green submit. On success: replace form with "Thanks — check your inbox for confirmation." in white.
- Micro-copy: "We email at most twice a month. Unsubscribe anytime." 12px `text-white/60`.

**`components/Footer.tsx`**
- `bg-bg` surface, 1px top border, 48px vertical padding.
- 3-column grid: (a) wordmark + 1-line tagline, (b) Product links (Features, Pricing, FAQ) as anchor links, (c) Company links (About, Contact → `mailto:hello@uncle.example`).
- Bottom row: "© 2026 Uncle Inc." left, "Built for founders." right in `text-muted` 12px.
- No social icons (none provisioned); honest copy only.

## 4. USER FLOWS

**Primary flow — waitlist signup (the only conversion):**
1. Land on `/` → see hero within 1 second, H1 legible.
2. Read hero sub-copy → skim 6 feature cards → scan pricing → resolve any objection in FAQ.
3. Click anchor or scroll to bottom CTA, or use the inline hero form, or use the nav button.
4. Type email → click "Get early access".
5. POST `/api/waitlist` with `{ email }`. Server validates RFC-5322 shape, rate-limits per IP (in-memory bucket, 5/min), inserts into Supabase `waitlist` table (or, if no DB is configured, appends to a JSON file in the repo for this build). Returns 200.
6. UI replaces the form with a success message. On error, shows inline red text "Something went wrong — try again."

**Secondary flow — FAQ read:** user clicks a question → row expands with body → clicks again or another question to collapse/expand. No state persisted.

**Tertiary flow — nav anchor scroll:** click "Features" / "Pricing" / "FAQ" in nav → smooth scroll to `#features` / `#pricing` / `#faq`. Mobile hamburger toggles a dropdown panel that links to the same anchors.

States: form (idle / submitting / success / error); FAQ item (closed / open); nav (top-of-page transparent-border / scrolled with shadow); mobile menu (closed / open).

## 5. PAGES / ROUTES

| Route | Purpose | Layout & main UI |
|---|---|---|
| `/` | Marketing landing page | Nav + Hero + Features + Pricing + FAQ + CTA + Footer, all anchored in `app/page.tsx` |
| `/api/waitlist` | POST handler for waitlist signup | Accepts JSON `{ email }`, validates, persists, returns `{ ok: true }` or 400/429/500 |

No other routes in this build. No dashboard, no auth, no app surface — landing page only, as specified.

## 6. CORE FEATURES

1. **Sticky nav with scroll-shadow** — a tiny client component (`useEffect` listening to `window.scroll`) adds `shadow-md` once `scrollY > 8`. Server-renders without the shadow, hydrates to add it. No layout shift.
2. **Hero waitlist form** — controlled email input, client-side validate, POST to `/api/waitlist`, render success or error inline. Disabled state on the button while submitting.
3. **Bottom CTA waitlist form** — same component, used twice (hero + CTA). Implemented as a shared `components/WaitlistForm.tsx` taking a `variant: "light" | "dark"` prop.
4. **6 feature cards** — static grid, hover lift, Lucide outline icons imported as SVG components (no icon library dependency to keep build minimal — use inline SVG paths).
5. **3 pricing tiers** — static grid, middle tier elevated with cobalt border and green "Most popular" pill.
6. **6 expandable FAQ items** — client component using `useState<number | null>(null)` to track the open index, single-open accordion behavior, chevron rotates 180° via `transition-transform`.
7. **Mobile menu** — client component, `useState<boolean>` for open/closed, full-width panel below the nav with the 3 links and a full-width CTA button.
8. **Waitlist API** — POST handler: parse JSON, validate email with a simple regex, in-memory rate limit (Map of ip → timestamps), append to `data/waitlist.json` (or insert into Supabase if `SUPABASE_URL` and `SUPABASE_SERVICE_ROLE_KEY` are present at runtime; both code paths are written, env-gated). Returns `{ ok: true }` on success.

## 7. DATA MODEL

Single entity in this build: `WaitlistEntry`.

```
WaitlistEntry {
  id: string              // crypto.randomUUID()
  email: string           // validated, lowercased, unique
  created_at: string      // ISO 8601, set server-side
  source: "hero" | "cta"  // which form submitted it
  user_agent: string      // request header, for debugging
  ip: string              // request IP, for rate limiting only
}
```

Persistence: `data/waitlist.json` (array of entries) in this build; Supabase `waitlist` table with the same schema if env vars are set. No relations, no migrations.

## 8. AUTH

**No auth on this page.** The landing page is public. The waitlist endpoint is a single POST with rate limiting — no session, no user accounts, no Supabase Auth, no NextAuth, no Clerk (explicitly excluded). The spec is explicit: this is a marketing landing page with a waitlist, not an app.

## 9. FILES

The product goal specifies these concrete files. I list them with one-line purposes; this is what gets created in the repo root.

```
package.json                       — next@14, react@18, tailwindcss, typescript, lucide-react
next.config.js                     — minimal Next config, reactStrictMode
tsconfig.json                      — Next.js TS config with @/* path alias
tailwind.config.ts                 — Tailwind config, navy/cobalt/green tokens, font family wiring
postcss.config.js                  — Tailwind + autoprefixer
app/globals.css                    — Tailwind directives + base resets
app/layout.tsx                     — Inter + IBM Plex Sans via next/font, metadata
app/page.tsx                       — assembles Nav, Hero, Features, Pricing, FAQ, CTA, Footer
components/Nav.tsx                 — sticky nav with scroll-shadow + mobile menu
components/Hero.tsx                — H1, sub, waitlist form, switchboard SVG
components/Features.tsx            — 6 feature cards in a 3-col grid
components/Pricing.tsx             — 3 pricing tiers, middle elevated
components/FAQ.tsx                 — 6-item accordion
components/CTA.tsx                 — navy waitlist section
components/Footer.tsx              — wordmark + links + copyright
components/WaitlistForm.tsx        — shared email form (light/dark variant)
components/icons.tsx               — inline SVG Lucide-style icons used by Features
app/api/waitlist/route.ts          — POST handler, validate + persist + rate limit
data/waitlist.json                 — empty array, append-only waitlist store
.gitignore                         — node_modules, .next, .env*.local
.env.example                       — documents optional Supabase env vars
README.md                          — one-paragraph product summary + run instructions
```

## 10. ACCEPTANCE

Done and working means every box below is checked:

- [ ] `npm install` completes without errors.
- [ ] `npm run build` completes without errors or type errors.
- [ ] `npm run dev` serves `/` at `http://localhost:3000` with no console errors.
- [ ] Nav is sticky, transparent border at top, gains shadow after 8px scroll.
- [ ] Nav mobile (≤768px) hamburger toggles a dropdown with Features / Pricing / FAQ / Join waitlist.
- [ ] Nav anchor links smooth-scroll to `#features`, `#pricing`, `#faq`.
- [ ] Hero H1 reads "Validate Your Startup Idea Before You Build It" at Inter 700, navy/ink.
- [ ] Hero and CTA waitlist forms POST to `/api/waitlist` and show a success message on 200, an error message on non-200.
- [ ] API route rejects malformed emails with 400, rate-limits per IP, appends to `data/waitlist.json` (and inserts to Supabase if env vars are set).
- [ ] Features section shows exactly 6 cards in a 3-col grid (desktop), 2-col (tablet), 1-col (mobile), each with an icon, title, and one-paragraph body.
- [ ] Pricing section shows exactly 3 tiers; the middle tier has a "Most popular" green pill and is visually elevated (`-mt-4`, cobalt border).
- [ ] FAQ section has exactly 6 items, single-open accordion behavior, chevron rotates on open.
- [ ] CTA section is navy background, white text, contains a working waitlist form.
- [ ] Footer shows wordmark, Product and Company link groups, and "© 2026 Uncle Inc."
- [ ] No fake testimonials, no invented customer quotes, no fake logos, no fake user counts, no fake ratings, no fake press mentions anywhere on the page.
- [ ] Tailwind colors `navy` `#1A3A5C`, `cobalt` `#4A90D9`, `green` `#22C55E` are defined in `tailwind.config.ts` and used throughout.
- [ ] Inter and IBM Plex Sans are loaded via `next/font/google` and applied to headings and body respectively.
- [ ] No Clerk, no Google/GitHub/social login buttons.
- [ ] `package.json` declares `next@14`, `react@18`, `tailwindcss`, `typescript`, and `lucide-react` only (no extras).
- [ ] `next.config.js` is minimal (no custom webpack beyond Next defaults).
- [ ] Vercel deploy completes and the live URL serves the page.

FILES: ["package.json", "next.config.js", "tsconfig.json", "tailwind.config.ts", "postcss.config.js", "app/globals.css", "app/layout.tsx", "app/page.tsx", "components/Nav.tsx", "components/Hero.tsx", "components/Features.tsx", "components/Pricing.tsx", "components/FAQ.tsx", "components/CTA.tsx", "components/Footer.tsx", "components/WaitlistForm.tsx", "components/icons.tsx", "app/api/waitlist/route.ts", "data/waitlist.json", ".gitignore", ".env.example", "README.md"]# Uncle Inc. — Landing Page Build Plan

## 1. PRODUCT

Uncle Inc. is a marketing landing page for an AI-assisted MVP platform aimed at pre-launch startup founders. The page's single job: convert a cold visitor into a waitlist signup. The hero lead — "Validate Your Startup Idea Before You Build It" — and the supporting sections (six feature cards, three pricing tiers, six-item FAQ, email waitlist, footer) are the conversion machinery. The visitor reads top-to-bottom, understands the value in under 10 seconds from the hero, scans features to confirm fit, checks pricing for affordability, resolves objections in FAQ, and submits an email. The page is a single Next.js 14 route (`/`) composed of sticky nav, hero, features, pricing, FAQ, CTA, and footer — no app, no auth, no dashboard in this build.

## 2. WHO IT'S FOR

- **ICP:** solo technical founders and first-time technical co-founders in the 0-to-1 phase, evaluating multiple product ideas before committing engineering time. They are skeptical of vague AI hype and want concrete mechanisms (scoring, validation, mockups) they can act on. They are time-poor, scan-first readers.
- **How that shapes the product and tone:** the page must lead with the highest-stakes pain (wasting weeks on a bad idea), use precise operator language ("validate", "score", "scaffold"), keep copy tight (no filler, no invented customer quotes, no fake logos), and put the email field within easy reach of every section (nav, hero, mid-page CTA, bottom CTA). The "Crisp Operator" archetype from the design session — clean enterprise UI, precise grids, confident whitespace, 4px spacing — fits this ICP exactly.

## 3. LOOK & FEEL

### 3.1 Visual system

- **Vibe/positioning:** Crisp Operator. Enterprise-clean, calm, confident, switchboard motif (signals routing ideas to validated output). Anti-fluff, anti-hype.
- **Color palette (used as Tailwind tokens, locked in `tailwind.config.ts`):**
  - `navy` `#1A3A5C` — primary text, dark backgrounds, wordmark
  - `cobalt` `#4A90D9` — links, primary buttons, icon accents
  - `green` `#22C55E` — CTA accent, positive indicators, "validate" highlights
  - `bg` `#F8FAFC` — page background
  - `surface` `#FFFFFF` — cards, elevated surfaces
  - `ink` `#0F172A` — primary text on light
  - `muted` `#64748B` — secondary text
  - `border` `#E2E8F0` — hairline borders
- **Typography:**
  - Headings: **Inter** (700/600), tight tracking, large display sizes (40–72px hero)
  - Body: **IBM Plex Sans** (400/500), comfortable reading size (16–18px, line-height 1.6)
  - Mono accents (for codes, plan names): **Source Code Pro** (loaded but used sparingly)
  - Both loaded via `next/font/google` in `app/layout.tsx`, exposed as CSS variables `--font-inter` and `--font-plex`
- **Spacing/layout:** 4px base unit. `container` is 1280px max with 24px gutter. Section vertical rhythm: 96px desktop / 64px mobile. 12-column grid.
- **Radius:** 4px default (rounded), 8px on cards, full (rounded-full) on pills and the email input button.
- **Shadows:** `shadow-sm` on cards, `shadow-md` on the sticky nav after scroll, no heavy drop shadows.
- **Iconography:** Lucide-style outline icons, 1.5px stroke, 20–24px, navy or cobalt. No emoji.
- **Imagery:** none on the landing page in this build — abstract SVG motif (switchboard / node-link diagram) used as a decorative hero accent only.
- **Interaction/motion:** subtle only. (a) Nav gains shadow on scroll via a small client effect. (b) FAQ items rotate chevron 180° when open. (c) Buttons darken 1 step on hover. (d) Feature cards lift 2px (`-translate-y-0.5`) on hover. No scroll-jacking, no parallax, no animated gradients.

### 3.2 Screen-by-screen layout (top to bottom of `/`)

**`components/Nav.tsx` — sticky top bar**
- 64px tall, white surface, 1px bottom border (`border-border`), becomes `shadow-md` after 8px scroll.
- Left: Uncle Inc. wordmark (navy text with a small green dot over the "i" in "Inc") + tiny "AI-assisted MVP platform" eyebrow.
- Right (desktop): anchor links — Features · Pricing · FAQ — in `text-ink` 14px, hover cobalt.
- Right: primary "Join waitlist" button (cobalt background, white text, 36px tall, rounded-full), anchors to `#waitlist`.
- Mobile: same wordmark left, hamburger right that toggles a full-width dropdown panel with the 3 links stacked + the CTA button full-width.

**`components/Hero.tsx` — above the fold**
- 96px top padding (clears sticky nav), 96px bottom.
- Two-column on desktop (60/40), single-column on mobile.
- Left column:
  - Eyebrow chip: green dot + "For pre-launch founders" in 12px uppercase tracking-wider, `text-muted`.
  - H1: "Validate Your Startup Idea Before You Build It" — Inter 700, 56px desktop / 36px mobile, `text-ink`, `leading-[1.05]`.
  - Sub: "Uncle scores your idea against real market signals, scaffolds an MVP plan, and generates clickable mockups — in under 10 minutes." — IBM Plex Sans 18px, `text-muted`, max-w-[560px].
  - Inline waitlist form: email input (h-12, rounded-full, border, `text-ink`, placeholder "you@startup.com") + green submit button "Get early access" (h-12, rounded-full, px-6, white text). On submit: POST to `/api/waitlist`, show inline "Thanks — you're on the list." message.
  - Below form: micro-copy "No credit card. Limited founding-member spots." in 12px `text-muted`.
- Right column:
  - A 480×420 abstract SVG (switchboard motif: 4 nodes connected by cobalt lines to a central green node, navy stroke). No fake product screenshot.

**`components/Features.tsx` — 6 feature cards**
- Section header: "Everything you need to go from idea to MVP." (Inter 600, 32px, centered). Subline: "Six focused tools. One workspace. No 12-month roadmap." (Plex 16px, muted, centered).
- 3-column grid on desktop, 2 on tablet, 1 on mobile, 24px gap.
- Each card: white surface, `border border-border`, `rounded-lg`, p-8, hover lift.
  - 1: "Idea scoring" — Lucide `gauge` icon, navy. Body: "Rank your idea on demand, competition, and feasibility. See the score in one number." Green badge: "0–100".
  - 2: "Competitor map" — Lucide `network` icon. Body: "Auto-generated map of the players in your space with positioning gaps highlighted."
  - 3: "MVP plan" — Lucide `list-checks` icon. Body: "A week-by-week build plan tuned to your stack, skills, and runway."
  - 4: "Clickable mockups" — Lucide `layout-template` icon. Body: "Generate wireframes for every core screen. Share a link, collect feedback."
  - 5: "User interview kit" — Lucide `messages-square` icon. Body: "10 tailored questions, a screening script, and a synthesis template."
  - 6: "Founder dashboard" — Lucide `kanban-square` icon. Body: "Track validation progress, blockers, and next actions in one view."

**`components/Pricing.tsx` — 3 tiers**
- Section header: "Simple pricing. No surprises." Inter 600, 32px, centered.
- 3-column grid, middle card (Builder) visually elevated: `border-cobalt`, `shadow-sm`, `-mt-4`, green "Most popular" pill at top.
- Cards share: white surface, `rounded-lg`, p-8, 1px border.
  - **Solo — Free.** $0/mo. Audience: "Curious founders." Features list (3 items): "1 idea validation", "Idea score report", "Email support". CTA: "Start free" → outline cobalt button, links to `#waitlist`.
  - **Builder — $29/mo.** "Most popular" pill (green). Audience: "First-time founders shipping an MVP." Features (6 items): "Unlimited idea validations", "Competitor maps", "Week-by-week MVP plan", "Clickable mockups", "User interview kit", "Email support". CTA: "Join waitlist" → solid cobalt button, links to `#waitlist`.
  - **Studio — $99/mo.** Audience: "Small founding teams." Features (7 items, all Builder features +): "Up to 5 seats", "Shared founder dashboard", "Priority support". CTA: "Talk to us" → outline cobalt button, mailto link.
- All prices displayed as "Coming soon" eyebrow above each card body to be honest that the SaaS is not yet launched (no invented launch date).

**`components/FAQ.tsx` — 6 expandable items**
- Section header: "Questions, answered." Inter 600, 32px, centered.
- Single-column list, max-w-[768px] centered, 8px gap between items.
- Each item: white surface, 1px border, `rounded-lg`, button toggles open state. Closed: question + chevron-right. Open: question + chevron-down, body paragraph below in `text-muted`. Smooth `transition-all`.
- Items:
  1. "What does Uncle actually do?" → "Uncle turns a one-paragraph idea into a validation package: a 0–100 score, a competitor map, a week-by-week MVP plan, clickable mockups, and a user interview kit."
  2. "Who is Uncle for?" → "Solo technical founders and first-time technical co-founders in the 0-to-1 phase, evaluating one or more ideas before committing engineering time."
  3. "How is this different from ChatGPT?" → "ChatGPT is a general assistant. Uncle is a structured workflow: it runs the same validation steps every time, with outputs you can share, compare, and act on."
  4. "Do I need to know how to code?" → "No. The MVP plan adapts to your skill level. If you can describe what you want to build, Uncle can scaffold it."
  5. "When will the product launch?" → "We're onboarding founding members from the waitlist in small cohorts. Joining the waitlist puts you in line."
  6. "Can I cancel anytime?" → "Yes. Builder and Studio are month-to-month. Cancel from your account settings."

**`components/CTA.tsx` — waitlist section**
- Navy background (`bg-navy`), white text, 96px vertical padding.
- Centered, max-w-[640px].
- H2: "Get early access." Inter 600, 40px, white.
- Sub: "Join the waitlist and we'll email you when your cohort opens." Plex 18px, `text-white/70`.
- Waitlist form (same as hero): email input + green submit. On success: replace form with "Thanks — check your inbox for confirmation." in white.
- Micro-copy: "We email at most twice a month. Unsubscribe anytime." 12px `text-white/60`.

**`components/Footer.tsx`**
- `bg-bg` surface, 1px top border, 48px vertical padding.
- 3-column grid: (a) wordmark + 1-line tagline, (b) Product links (Features, Pricing, FAQ) as anchor links, (c) Company links (About, Contact → `mailto:hello@uncle.example`).
- Bottom row: "© 2026 Uncle Inc." left, "Built for founders." right in `text-muted` 12px.
- No social icons (none provisioned); honest copy only.

## 4. USER FLOWS

**Primary flow — waitlist signup (the only conversion):**
1. Land on `/` → see hero within 1 second, H1 legible.
2. Read hero sub-copy → skim 6 feature cards → scan pricing → resolve any objection in FAQ.
3. Click anchor or scroll to bottom CTA, or use the inline hero form, or use the nav button.
4. Type email → click "Get early access".
5. POST `/api/waitlist` with `{ email }`. Server validates RFC-5322 shape, rate-limits per IP (in-memory bucket, 5/min), inserts into Supabase `waitlist` table (or, if no DB is configured, appends to a JSON file in the repo for this build). Returns 200.
6. UI replaces the form with a success message. On error, shows inline red text "Something went wrong — try again."

**Secondary flow — FAQ read:** user clicks a question → row expands with body → clicks again or another question to collapse/expand. No state persisted.

**Tertiary flow — nav anchor scroll:** click "Features" / "Pricing" / "FAQ" in nav → smooth scroll to `#features` / `#pricing` / `#faq`. Mobile hamburger toggles a dropdown panel that links to the same anchors.

States: form (idle / submitting / success / error); FAQ item (closed / open); nav (top-of-page transparent-border / scrolled with shadow); mobile menu (closed / open).

## 5. PAGES / ROUTES

| Route | Purpose | Layout & main UI |
|---|---|---|
| `/` | Marketing landing page | Nav + Hero + Features + Pricing + FAQ + CTA + Footer, all anchored in `app/page.tsx` |
| `/api/waitlist` | POST handler for waitlist signup | Accepts JSON `{ email }`, validates, persists, returns `{ ok: true }` or 400/429/500 |

No other routes in this build. No dashboard, no auth, no app surface — landing page only, as specified.

## 6. CORE FEATURES

1. **Sticky nav with scroll-shadow** — a tiny client component (`useEffect` listening to `window.scroll`) adds `shadow-md` once `scrollY > 8`. Server-renders without the shadow, hydrates to add it. No layout shift.
2. **Hero waitlist form** — controlled email input, client-side validate, POST to `/api/waitlist`, render success or error inline. Disabled state on the button while submitting.
3. **Bottom CTA waitlist form** — same component, used twice (hero + CTA). Implemented as a shared `components/WaitlistForm.tsx` taking a `variant: "light" | "dark"` prop.
4. **6 feature cards** — static grid, hover lift, Lucide outline icons imported as SVG components (no icon library dependency to keep build minimal — use inline SVG paths).
5. **3 pricing tiers** — static grid, middle tier elevated with cobalt border and green "Most popular" pill.
6. **6 expandable FAQ items** — client component using `useState<number | null>(null)` to track the open index, single-open accordion behavior, chevron rotates 180° via `transition-transform`.
7. **Mobile menu** — client component, `useState<boolean>` for open/closed, full-width panel below the nav with the 3 links and a full-width CTA button.
8. **Waitlist API** — POST handler: parse JSON, validate email with a simple regex, in-memory rate limit (Map of ip → timestamps), append to `data/waitlist.json` (or insert into Supabase if `SUPABASE_URL` and `SUPABASE_SERVICE_ROLE_KEY` are present at runtime; both code paths are written, env-gated). Returns `{ ok: true }` on success.

## 7. DATA MODEL

Single entity in this build: `WaitlistEntry`.

```
WaitlistEntry {
  id: string              // crypto.randomUUID()
  email: string           // validated, lowercased, unique
  created_at: string      // ISO 8601, set server-side
  source: "hero" | "cta"  // which form submitted it
  user_agent: string      // request header, for debugging
  ip: string              // request IP, for rate limiting only
}
```

Persistence: `data/waitlist.json` (array of entries) in this build; Supabase `waitlist` table with the same schema if env vars are set. No relations, no migrations.

## 8. AUTH

**No auth on this page.** The landing page is public. The waitlist endpoint is a single POST with rate limiting — no session, no user accounts, no Supabase Auth, no NextAuth, no Clerk (explicitly excluded). The spec is explicit: this is a marketing landing page with a waitlist, not an app.

## 9. FILES

The product goal specifies these concrete files. I list them with one-line purposes; this is what gets created in the repo root.

```
package.json                       — next@14, react@18, tailwindcss, typescript, lucide-react
next.config.js                     — minimal Next config, reactStrictMode
tsconfig.json                      — Next.js TS config with @/* path alias
tailwind.config.ts                 — Tailwind config, navy/cobalt/green tokens, font family wiring
postcss.config.js                  — Tailwind + autoprefixer
app/globals.css                    — Tailwind directives + base resets
app/layout.tsx                     — Inter + IBM Plex Sans via next/font, metadata
app/page.tsx                       — assembles Nav, Hero, Features, Pricing, FAQ, CTA, Footer
components/Nav.tsx                 — sticky nav with scroll-shadow + mobile menu
components/Hero.tsx                — H1, sub, waitlist form, switchboard SVG
components/Features.tsx            — 6 feature cards in a 3-col grid
components/Pricing.tsx             — 3 pricing tiers, middle elevated
components/FAQ.tsx                 — 6-item accordion
components/CTA.tsx                 — navy waitlist section
components/Footer.tsx              — wordmark + links + copyright
components/WaitlistForm.tsx        — shared email form (light/dark variant)
components/icons.tsx               — inline SVG Lucide-style icons used by Features
app/api/waitlist/route.ts          — POST handler, validate + persist + rate limit
data/waitlist.json                 — empty array, append-only waitlist store
.gitignore                         — node_modules, .next, .env*.local
.env.example                       — documents optional Supabase env vars
README.md                          — one-paragraph product summary + run instructions
```

## 10. ACCEPTANCE

Done and working means every box below is checked:

- [ ] `npm install` completes without errors.
- [ ] `npm run build` completes without errors or type errors.
- [ ] `npm run dev` serves `/` at `http://localhost:3000` with no console errors.
- [ ] Nav is sticky, transparent border at top, gains shadow after 8px scroll.
- [ ] Nav mobile (≤768px) hamburger toggles a dropdown with Features / Pricing / FAQ / Join waitlist.
- [ ] Nav anchor links smooth-scroll to `#features`, `#pricing`, `#faq`.
- [ ] Hero H1 reads "Validate Your Startup Idea Before You Build It" at Inter 700, navy/ink.
- [ ] Hero and CTA waitlist forms POST to `/api/waitlist` and show a success message on 200, an error message on non-200.
- [ ] API route rejects malformed emails with 400, rate-limits per IP, appends to `data/waitlist.json` (and inserts to Supabase if env vars are set).
- [ ] Features section shows exactly 6 cards in a 3-col grid (desktop), 2-col (tablet), 1-col (mobile), each with an icon, title, and one-paragraph body.
- [ ] Pricing section shows exactly 3 tiers; the middle tier has a "Most popular" green pill and is visually elevated (`-mt-4`, cobalt border).
- [ ] FAQ section has exactly 6 items, single-open accordion behavior, chevron rotates on open.
- [ ] CTA section is navy background, white text, contains a working waitlist form.
- [ ] Footer shows wordmark, Product and Company link groups, and "© 2026 Uncle Inc."
- [ ] No fake testimonials, no invented customer quotes, no fake logos, no fake user counts, no fake ratings, no fake press mentions anywhere on the page.
- [ ] Tailwind colors `navy` `#1A3A5C`, `cobalt` `#4A90D9`, `green` `#22C55E` are defined in `tailwind.config.ts` and used throughout.
- [ ] Inter and IBM Plex Sans are loaded via `next/font/google` and applied to headings and body respectively.
- [ ] No Clerk, no Google/GitHub/social login buttons.
- [ ] `package.json` declares `next@14`, `react@18`, `tailwindcss`, `typescript`, and `lucide-react` only (no extras).
- [ ] `next.config.js` is minimal (no custom webpack beyond Next defaults).
- [ ] Vercel deploy completes and the live URL serves the page.

FILES: ["package.json", "next.config.js", "tsconfig.json", "tailwind.config.ts", "postcss.config.js", "app/globals.css", "app/layout.tsx", "app/page.tsx", "components/Nav.tsx", "components/Hero.tsx", "components/Features.tsx", "components/Pricing.tsx", "components/FAQ.tsx", "components/CTA.tsx", "components/Footer.tsx", "components/WaitlistForm.tsx", "components/icons.tsx", "app/api/waitlist/route.ts", "data/waitlist.json", ".gitignore", ".env.example", "README.md"]