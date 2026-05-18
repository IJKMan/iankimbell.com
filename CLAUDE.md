# CLAUDE.md

Working notes for anyone (including Claude) who picks up this repo to
keep iterating on Ian Kimbell's site.

---

## What this site is

A single-page consulting funnel for Ian Kimbell. Ian spent 35+ years
inside the world's technology companies (DuPont 1987 to 1998, then SAP
since 1998), most recently as one of SAP's most-requested keynote
speakers. He now consults independently, worldwide, on storytelling
and presentation craft.

**Audience:** warm-only. The funnel is built for people Ian already
knows (35+ years of network). The site's job is to be a structured
visiting card someone can show their team after Ian has emailed them.
It is NOT optimized for cold search traffic.

**Domain:** iankimbell.com

---

## How to preview locally

No build step. Just open the file:

```
open previews/v1.html
```

That's it. All CSS is inline in one HTML file. Fonts load from
Google Fonts CDN. The headshot and any other assets live in
`assets/`.

---

## File map

```
iankimbell.com/
├── CLAUDE.md             # this file
├── README.md             # short repo intro
├── .gitignore            # excludes .DS_Store, RAW files, local archive
├── previews/
│   └── v1.html           # the working draft of the site
├── assets/
│   ├── README.md         # how Ian uploads his headshot
│   ├── ian.jpg           # hero photo (HoloLens stage shot)
│   └── press/
│       └── README.md     # where real press logos drop in
└── draft/
    └── README.md         # Ian's raw materials drop zone
    └── *.png             # screenshots, copy drafts, references
```

**Currently no `index.html` at the root.** This is deliberate. The
site is in preview until ready to go live (see "Going live" below).

---

## Page structure (top to bottom)

| Section | What it does | Section ID |
|---------|--------------|-----------|
| Nav | Sticky white bar with brand + 3 links | `nav.top` |
| Hero | Edge-bleed split: teal left panel + headshot right | `#top` |
| Proof strip | Four stat callouts (35+ yrs, 10K+ audience, etc.) | `.proof` |
| World map | Dotted continents + bright city dots, hover for name | `#stages` |
| What I do | Single-block: intro + 5 numbered activities + closing | `#services` |
| Why me | Ian's "rehearse once" pull-quote + body paragraphs | `.why` |
| Press strip | "Featured in" with CIO / SAP Press / Wiley wordmarks | `.press` |
| Kind words | 4 testimonials, two-part (headline + body + role) | `#kind-words` |
| About | Career arc stack + bio paragraphs | `#about` |
| Contact | Deep teal section, intro + email/LinkedIn channels | `#contact` |
| Footer | 4-column: brand + What I do / About / Get in touch | `footer` |

---

## Design system

### Palette (CSS variables)

```css
--bg:          #fbfaf7;       /* warm cream, page background */
--bg-soft:     #f1ede4;       /* paper, section variants */
--ink:         #18181b;       /* charcoal text */
--ink-soft:    #4a4a4f;       /* secondary text */
--mute:        #8a8a90;       /* tertiary, captions */
--accent:      #3f7975;       /* muted teal/sage */
--accent-deep: #2b5957;       /* deep teal, contact + map bg */
```

The page is intentionally monochromatic teal + cream + ink. Don't
introduce blue/purple/red without a strong reason.

### Typography

- **Display / headings:** Fraunces (serif, variable). Italic emphasis
  on one or two words inside headlines is the signature pattern.
- **Body / sans:** Inter Tight.
- **Code / mono:** none used currently.

Both fonts load from Google Fonts.

### Italic emphasis pattern

Headlines use one or two italicized words inside an otherwise upright
serif phrase. Examples currently on the page:

- "Complex ideas, told as stories that win *attention, trust, and action*."
- "From the stage, *worldwide*."
- "They need a *translator*, and someone to coach the human..."

When writing new headlines, keep this rhythm.

---

## Editorial rules

- **NO em dashes.** Project rule. Use commas, colons, periods,
  parentheses, or semicolons instead.
- **BLUF**: lead with the essential point, context after.
- **Concrete over abstract.** "Sapphire keynote, Berlin 2000" beats
  "international speaking engagements."
- **Warm-network voice.** First-person, confident but not salesy. The
  reader already knows Ian; the copy can assume warmth.
- **Specific cities and venues** beat generic "globally."

---

## Common edits (recipes)

### Add a city to the world map

Two changes in `previews/v1.html`:

1. Add a `<g class="city">` inside `<g class="cities-layer">` in
   the world-map SVG. Coordinates use equirectangular projection:
   `x = longitude + 180`, `y = 90 - latitude`. Example:

   ```html
   <g class="city" transform="translate(311.5,49.7)">
     <circle r="6" fill="transparent"/>
     <circle r="1.6" class="dot"/>
     <text y="-4" class="label">Seoul</text>
   </g>
   ```

2. Add a corresponding `nth-of-type` animation-delay CSS rule if the
   new city pushes past 24. Increment by 0.1s.

The land dots stay the same; cities float on top.

### Swap the hero photo

1. Drop new JPG to `assets/ian.jpg` (1600px+ wide, portrait crop
   ideally, under 500 KB).
2. If subject framing changes, tune `.hero .portrait` CSS:
   `background-position: <x>% <y>%;` controls the crop.

The photo caption ("Sapphire Keynote Presentation") is set in CSS at
`.hero .portrait::after`. Edit the `content:` value to change it.

### Swap a real press logo in for the text wordmark

1. Drop the SVG (or PNG) into `assets/press/` with a clear filename
   like `cio.svg`.
2. In `previews/v1.html`, find the `.press-logos` list. Replace the
   text content of the `<a>` with an `<img>`:

   ```html
   <li><a href="..." target="_blank" rel="noopener">
     <img src="../assets/press/cio.svg" alt="CIO Magazine" />
   </a></li>
   ```

3. May need a small CSS tweak: add `.press-logos img { height: 28px; }`
   or similar to constrain logo height.

### Edit copy

All copy is plain HTML in `previews/v1.html`. Find the relevant
section by its comment marker (e.g. `<!-- ABOUT -->`) and edit
directly. Refresh the browser to see changes.

### Add a new testimonial

Find `<!-- TESTIMONIALS -->` section. Each card looks like:

```html
<div class="quote">
  <p class="q">Short headline quote.</p>
  <p class="q-body">Longer body paragraph with detail.</p>
  <div class="by"><strong>Role</strong>Company (optional)</div>
</div>
```

Add another `<div class="quote">` block. Grid handles layout
automatically (2 columns on desktop, 1 on mobile).

### Add another "What I do" activity

In the `.activities` `<ol>` list inside the `.practice` section,
add another `<li>`. The numbered "0X" prefix is generated by CSS
counter, no manual numbering needed.

---

## Hosting

### Currently

The repo is on GitHub. Pushes to `main` go live in the repo only;
nothing is deployed to a public URL yet.

### When going live

The plan is Cloudflare Pages auto-deploy from `main`. Setup:

1. Cloudflare dashboard → Pages → Create → Connect to Git → select
   `IJKMan/iankimbell.com`
2. Framework preset: **None**. Build command: empty. Output directory: `/`
3. Production branch: `main`
4. After project exists, add `iankimbell.com` as a custom domain.

Once Cloudflare is wired, every push to `main` triggers a redeploy
in ~60s.

### Going from preview to live

Currently the working draft is at `previews/v1.html`. To make it the
live homepage:

```bash
cp previews/v1.html index.html
git add index.html
git commit -m "promote v1 to index.html"
git push
```

Update relative paths inside the new `index.html` if needed:
`../assets/` becomes `assets/` (since it's now at root, not nested).

---

## Git conventions

- Stage explicit paths, never `git add -A` (avoid accidental commits
  of system files or local-only stuff).
- Commit messages: concise first line in lowercase, blank line, then
  body explaining the why if non-obvious.
- No force pushes to `main` without explicit reason.
- The `_archive-personal-site-previews/` folder is gitignored. It's
  local exploration kept on disk for reference, never shipped.

---

## Open questions / things Ian might want next

These came up during the build but are deliberately left for Ian to
decide:

- **AI-augmented coaching service**: original brief had it as a third
  service; Ian's draft dropped it. Could come back as a 6th
  activity in the "What I do" list if Ian wants to keep that
  positioning.
- **Headshot**: current hero photo is from 2004 (Canon EOS-1D Mark II
  EXIF data). Still works, but a fresher portrait wouldn't hurt for
  longevity.
- **Cities still missing from the map**: no North American cities
  (NYC / SF / Chicago / Toronto), no Australia/NZ (Sydney /
  Melbourne), no Middle East (Dubai). If Ian's spoken there, add them.
- **Real press logos**: SVGs of CIO Magazine, SAP Press, and Wiley
  go in `assets/press/`, then swap the text spans for `<img>`.
- **LinkedIn URL**: currently `#` placeholder. Update to Ian's real
  profile URL when ready.
- **More testimonials**: 4 currently. Ian's network has thousands of
  people; more named quotes would compound the trust.
- **Cloudflare Pages setup**: not done yet. See "Hosting" above.

---

## Using Claude Code on this site

This file is set up so Claude Code reads it automatically every time
you start a session in this directory. You don't have to ask. The
first thing Claude does on launch is load CLAUDE.md, which means it
already knows the site, the design rules, and the conventions.

### First-time setup (Windows)

Claude Code expects a Unix-style shell to run from. Windows doesn't
ship with one, so we install **Git for Windows**, which bundles
`git` plus a bash terminal called **Git Bash**. Everything below
runs in Git Bash (not Command Prompt, not PowerShell).

1. **Install Git for Windows**: https://git-scm.com/download/win
   Take the defaults during the installer. This gives you `git`
   plus the **Git Bash** terminal you'll use from here on.

2. **Install Node.js (LTS version)**: https://nodejs.org/en/download
   Claude Code is distributed through npm, which comes with Node.

3. **Open Git Bash** (Start menu → Git Bash).

4. **Install Claude Code** (in Git Bash):
   ```
   npm install -g @anthropic-ai/claude-code
   ```

5. **Configure git with your name and email** (once per machine):
   ```
   git config --global user.name "Ian Kimbell"
   git config --global user.email "iankimbell@gmail.com"
   ```

6. **Clone the repo**:
   ```
   git clone https://github.com/IJKMan/iankimbell.com
   cd iankimbell.com
   ```

7. **Start a session**:
   ```
   claude
   ```

That's it. You're in.

> **Always run `claude` from Git Bash.** It will not work properly
> from Command Prompt or PowerShell. If you accidentally open those,
> close them and use Git Bash instead.

> Optional: if you ever want a more "real Linux" environment on
> Windows, look up **WSL2** (Windows Subsystem for Linux). It's
> smoother for serious dev work, but Git Bash is plenty for editing
> this site.

### First-time setup (Mac)

If you're ever on a Mac instead: `git` is built in. Just install
Node from https://nodejs.org, then `npm install -g @anthropic-ai/claude-code`,
then clone and `cd` and `claude`. Skip the Git Bash step entirely.

### Example first-session prompts

These are good starting points. You don't have to use these wordings,
they're just to show the kind of thing Claude can do:

- *"Show me the current site in my browser."* (Claude will open
  previews/v1.html for you.)
- *"Add Seoul to the world map."* (Claude will find the coordinates,
  insert the SVG dot, and commit.)
- *"Change the headline to say X."* (Targeted copy edit.)
- *"Replace the second testimonial with this one:"* (Paste new text.)
- *"I want to add a new section between Why-me and Press called
  'Recent talks'. Plan it first, then ask before changing anything."*
- *"Commit and push my changes."* (Claude handles the git side.)
- *"Read the CLAUDE.md and tell me what's left to do."* (Surfaces
  the open-questions list.)

### Permissions

Claude Code will ask you before:

- Editing files (the first time per session, then it remembers)
- Running git commands
- Pushing to GitHub

You can approve each prompt individually. If you want Claude to work
without stopping to ask, you can run `claude --permission-mode
acceptEdits` (but read the prompts at least a few times first so you
trust what's being done).

### Things to know

- **The site is one file.** Everything is in `previews/v1.html`. No
  coordination across components, no build step.
- **Test in the browser after every change.** Refresh
  `file:///path/to/iankimbell.com/previews/v1.html` (or just ask
  Claude to open it for you).
- **Push to GitHub when you're happy.** Once Cloudflare Pages is
  wired up, pushing to `main` deploys to iankimbell.com automatically.
- **Don't force-push to main.** If you ever get stuck, ask Claude for
  help rather than running destructive git commands.
- **For big design changes, brainstorm first.** Tell Claude what
  you're trying to accomplish, not the exact code change. It'll
  propose options, you pick, then it executes.

### When Claude gets it wrong

It will sometimes. Two helpful patterns:

- *"That's not what I meant. I wanted X, not Y."* Claude will
  correct course.
- *"Undo the last change."* Claude can revert via git.

### Quick reference for "what file"

99% of the time the answer is `previews/v1.html`. The other 1%:

- Photo issues → `assets/ian.jpg`
- Logo issues → `assets/press/`
- Repo intro → `README.md`
- This file → `CLAUDE.md`

---

## History

Built collaboratively by Mike Merrill and Claude (Opus 4.7, 1M
context) starting May 18, 2026. Initial scaffold modeled on
pathos.com.au with visual cues from orator-style speaking
consultancies. Ian's real copy and headshot integrated in the same
session. Press references mined from a 2009 CIO Magazine interview
and the SAP Press and Dummies.com author bios.
