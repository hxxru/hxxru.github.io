# personal site: architecture & vision

## the thesis

a personal website can be the primary hub for creative work and social networking if it's (a) memorable enough that handing someone a URL feels *more* interesting than a social media handle, (b) easy to find contact info and work, and (c) maintained as a living document rather than a one-time art piece. AI coding tools have made this realistic in a way it wasn't two years ago.

## high-level architecture

the site has three layers, loosely coupled:

```
haru.ink/
├── index.html          ← landing page (lightweight, loads instantly)
├── /world              ← three.js explorable environment (desktop)
├── /garden (or /feed)  ← quartz-powered digital garden / feed
└── /about              ← contact and onboarding page
```

### 1. landing page (`/`)

minimal. your name, a single ambient visual element (could be a lightweight canvas hint of the world aesthetic, not the full three.js scene). three clear paths:

- **explore** → `/world` (desktop) or `/garden` (mobile, auto-redirect)
- **feed / garden** → `/garden`
- **about me** → `/about`

design constraint: must load instantly. this is the page someone pulls up at a noisy show. they need to get a sense of you and find your contact info within 10 seconds.

### 2. the world (`/world`)

a three.js procedural environment. wasd navigation. distinct regions of the world correspond to bodies of work. artifacts (objects in the environment) are interactable—clicking opens a modal with the associated work.

**MVP scope:** poetry as the initial content type. poems surface via object interaction → modal overlay with text. the world has procedural ambient audio (evolving pads, dub techno percussion) that is environmental, not portfolio. complex compositions live on bandcamp.

**future expansion:** visual work, music artifacts, collaborative/interactive pieces. new content gets integrated into the world with AI coding assistance, using established patterns from previous integrations.

**mobile:** the world is desktop-only (or opt-in on mobile). mobile visitors get redirected to the garden/feed view.

### 3. the garden (`/garden`)

a quartz build (static, markdown-based, github pages compatible). this is the "easy publish" layer—adding new work means writing a markdown file and deploying. quartz provides full-text search, graph view (lightweight "discover connections" for the 2D experience), wikilinks, backlinks, and popover previews out of the box.

the feed is the living document. the world is the curated gallery. new work appears in the feed first; the world gets updated periodically when there's creative energy for spatial curation.

**customization needed:** quartz's default aesthetic reads as "developer PKM tool." fonts, colors, and chrome need to be adjusted to feel like an artist's space rather than a wiki.

### 4. about / onboarding (`/about`)

serves double duty:

- **practical:** name, bio, contact (signal, email, whatever), links to bandcamp / soundcloud / other platforms.
- **meta-awareness:** a brief, light explanation of the site's philosophy and why it exists. tone: charming-principled, not manifesto-brained. the goal is to make visitors think "huh, that's kind of cool."

this is the onboarding page—the thing you point people to when they ask how to find you.

## technical decisions

| decision | choice | rationale |
|---|---|---|
| hosting | github pages | familiar ecosystem, free, static |
| feed engine | quartz v4 | markdown-based, graph view, obsidian-compatible, static output |
| 3D engine | three.js | already proficient, proven with planetarium project |
| repo structure | multi-repo | three repos: main site (landing + about), `world` (three.js), `garden` (quartz). independent build/deploy pipelines. main repo is github pages user site with custom domain; world and garden are project repos deploying to subpaths |
| domain | haru.ink | short, speakable, memorable. .ink fits a creative practice |
| shared styling | EB Garamond + Noto Serif KR, dark mode only, shared color tokens | see aesthetics section. consistent across all three repos |
| mobile strategy | garden as default on mobile, world as desktop experience | three.js wasd on a phone is miserable |

## aesthetics

**color:** dark mode only. black background, white/grey text. no light mode toggle.

**typography:** EB Garamond as the primary serif face (400/500/600 weights, italic variants). for korean text, Noto Serif KR at extralight (200) weight, applied via `:lang(ko)` selector. antialiased rendering across all layers.

```scss
/* shared font imports */
@import url('https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500;1,600&display=swap');

@font-face {
  font-family: 'Noto Serif KR';
  font-style: normal;
  font-weight: 200;
  src: url('https://cdn.jsdelivr.net/gh/moonspam/NanumBarunGothic@latest/NotoSerifKR-ExtraLight.woff2') format('woff2'),
       url('https://cdn.jsdelivr.net/gh/moonspam/NanumBarunGothic@latest/NotoSerifKR-ExtraLight.woff') format('woff');
  font-display: swap;
}

:lang(ko) {
  font-family: 'Noto Serif KR', 'EB Garamond', serif;
  font-weight: 200;
}

html {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
```

these font imports and color tokens should be consistent across all three repos. the landing page and garden use them directly in CSS; the world uses them for modal overlays and any HUD/UI elements.

## content taxonomy (evolving)

**phase 1 (MVP):**
- poetry → world artifacts + garden entries
- music → bandcamp links (referenced from garden, ambient audio in world is procedural/separate)
- visual work → garden entries

**phase 2:**
- music artifacts in the world (listening stations? ambient zones?)
- visual work embedded in world regions
- interactive/generative pieces as their own world zones

## open questions

- **world ↔ garden linking:** can you click an artifact in the world and land on its garden page? bidirectional linking would unify the two layers.
- **update workflow for the world:** how standardized can the "add a new artifact" process become? templating system?
- **analytics:** do you care about knowing who visits and what they explore? privacy-respecting options exist (plausible, umami).
- **RSS:** quartz supports RSS. worth enabling as another passive-exposure channel for the subset of people who use feed readers.

## the social strategy

when someone asks how to find you:
1. give them the URL: **haru.ink**
2. they visit, see the landing page, explore or browse
3. for DMs: signal / email / whatever, linked from `/about`

the passive exposure problem (social feeds surfacing your content to followers) remains partially unsolved. the garden's RSS feed covers technical users. for everyone else, the bet is that word of mouth + a memorable site > algorithmic feed placement for an independent artist's actual audience of 50-100 people.
