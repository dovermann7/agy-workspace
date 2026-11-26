---
name: brand-landing-page
description: >-
  Builds a responsive, editorial brand landing page with generated image assets,
  an embedded 30-second brand film video player, interactive program tabs,
  FAQ accordion, application modal, and ZIP delivery packaging. Use when the
  user asks to create a brand, community, or service landing page (like the
  birosohouse project) with rich media assets and a packaged deliverable.
---

# Brand Landing Page & Media Packaging

A repeatable workflow for creating a single-page, zero-dependency (`HTML` / `CSS` / `JS`) brand landing page with cohesive editorial styling, visual & video assets, interactive UI components, and a ready-to-share `.zip` archive.

## Directory Structure

Create a self-contained project folder in the workspace:

```text
<project-slug>/
├── index.html
├── styles.css
├── script.js
└── assets/
    ├── hero-dining.jpg        # Hero & video poster image
    ├── cooking-together.jpg   # Program/feature image(s)
    └── <project-slug>-ad.mp4  # 30s brand film / promo video
```

## Workflow Steps

### 1. Define Brand & Design Tokens (`styles.css`)

Use CSS custom properties in `:root` for a warm, editorial aesthetic (or adapt the palette to the brand):

- **Backgrounds**: `--bg-cream: #faf6f0`, `--bg-warm: #f3ebe0`, `--bg-card: #ffffff`
- **Text**: `--text-main: #2a211b`, `--text-muted: #6c5e54`
- **Accents**: `--accent: #b85c38`, `--accent-hover: #9e4b2b`, `--accent-soft: #f5e2d8`, `--olive: #5b6d5b`
- **Typography**: Pair a serif display font (`Gowun Batang`) with a clean sans-serif body font (`Noto Sans KR`) via Google Fonts.
- **Layout**: Center content with `.container { width: min(1160px, 92%); margin: 0 auto; }` and use a sticky blurred header (`.site-header` with `backdrop-filter: blur(12px)`).

### 2. Prepare Visual & Video Assets (`assets/`)

1. **Images**: Generate or place high-resolution brand photography in `assets/` (using the `image-generator` subagent when custom visuals are needed). Ensure the hero image is also suitable as the video `poster`.
2. **Brand Film (`<project-slug>-ad.mp4`)**: Place the 30-second promo/brand video in `assets/` and embed it with native HTML5 video:
   ```html
   <video class="brand-video" controls playsinline preload="metadata" poster="assets/hero-dining.jpg">
     <source src="assets/<project-slug>-ad.mp4" type="video/mp4" />
   </video>
   ```

### 3. Build the Landing Page Sections (`index.html`)

Structure `<main>` with clear, scannable sections and anchor IDs matching the header navigation:

1. **Header & Mobile Drawer**: Brand logo (EN + KO), anchor nav links, primary CTA button (`.open-apply-modal`), and hamburger menu (`#mobileMenuBtn` / `#mobileNav`).
2. **Hero (`#hero`)**: Category badge, `h1` headline with `<em>` highlight, subtitle, CTA group (apply modal, jump to `#brand-film`, external channel link), 3-column stats bar, and framed hero image with a floating status card.
3. **Brand Film (`#brand-film`)**: 2-column grid pairing the 30s `<video>` player with brand story copy and CTAs.
4. **Brand Story (`#story`)**: 3-card value proposition grid (`.story-grid`).
5. **Programs (`#programs`)**: Accessible tab bar (`role="tablist"`, `.tab-btn[data-tab]`) switching `.program-panel` views (`#panel-<tab>`). Each panel's CTA button sets `data-program="<Program Name>"` to pre-fill the application modal.
6. **How It Works (`#journey`)**: 5-step numbered timeline (`.journey-timeline`).
7. **Media / Social Sketch (`#youtube`)**: 3-card video/shorts showcase linking to the brand's channel.
8. **Location & Official Links (`#location`)**: Address, transit directions, official channel links, and styled location card.
9. **FAQ (`#faq`)**: Accessible accordion items (`.faq-item` / `.faq-question` with `aria-expanded`).
10. **Bottom CTA & Footer**: Final conversion banner and copyright footer.
11. **Application Modal (`#applyModal`)**: Dialog with form inputs (`#applyForm`) that transitions to a confirmation summary (`#modalSuccessWrap`) on submit.

### 4. Implement Interactive Behavior (`script.js`)

Wrap all DOM bindings in `DOMContentLoaded` and implement:

- **Sticky Header**: Toggle `.scrolled` on `#header` when `window.scrollY > 12`.
- **Mobile Menu**: Toggle `.open` on `#mobileNav` and update `aria-expanded`; close drawer when any nav link is clicked.
- **Program Tabs**: Sync `.active` and `aria-selected` across `.tab-btn` and `.program-panel` (`#panel-${target}`).
- **FAQ Accordion**: Toggle `.open` on `.faq-item` and update `aria-expanded`.
- **Application Modal**:
  - Open via `.open-apply-modal`, pre-selecting `#applicantProgram` from `btn.dataset.program` when present.
  - Close via close/done buttons, backdrop click, or `Escape` key.
  - On `#applyForm` submit, prevent default, populate `#successSummary`, hide `#modalFormWrap`, and show `#modalSuccessWrap`.

### 5. Verify & Package Deliverable

1. **Verify**: Check that all relative asset paths (`assets/...`) resolve, interactive controls (tabs, FAQ, modal, mobile drawer) work without console errors, and the video plays cleanly.
2. **Package**: Create a `.zip` archive of the project folder in the workspace root:
   ```powershell
   Compress-Archive -Path <project-slug> -DestinationPath <project-slug>.zip -Force
   ```
   Confirm `<project-slug>.zip` exists and is non-empty.
