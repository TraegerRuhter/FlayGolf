# Flay Golf: handoff

Where things stand as of November 10, 2026, for whoever picks this up next.

**Repo:** `TraegerRuhter/FlayGolf`. Everything up to PR #7 is merged to `main`:
three rounds of research and the site sketchbook (#6), then the home page
fixes below (#7).

## Read first

- `docs/flay-golf-plan.md`: the business plan, research, and open questions
- `docs/brand/brand.md`: voice, look, palette, type and logo
- `docs/brand/logo-brief.md` and `logo-reference.png`: the illustrator brief
  and reference image
- `site/README.md`: how to run the site, connect Kit, and deploy to Cloudflare
  Pages
- `CLAUDE.md` and `docs/agents/`: the issue tracker (GitHub Issues), triage
  labels, domain docs

## Research

Each report in `reports/` has its sourced notes in a matching folder under
`research_notes/`. Several sources were blocked during research (NGF, USPTO,
FTC, NAD, Reddit, some forums), so figures marked "search summary" or
"unverified" must be checked before they go into anything public.

- **`reports/Flay Golf ideas and market gaps.md`:** 160 ideas triaged into
  Do now / Next / Later / Big bets / Skip, plus comps from other sports and the
  market's white space. Main finding: no golf brand owns the home-sim and
  garage golfer, and nobody does dark, antique-style golf goods.
- **`reports/Flay Golf web design ideas.md`:** 10 site concepts. The
  recommendation is to build "Plate I" (home page as an anatomical plate whose
  lettered legend is the menu), then borrow pieces of the others. It also
  lists fixes for the current page. Hover and placeholder contrast and
  self-hosted fonts are done; still open are no blackletter on the button, a
  visible button edge, and replacing the AI logo image.
- **`docs/design/site-sketches.html`:** the 10 concepts as clickable mockups.
  Published privately as the Flay Site Sketchbook artifact:
  https://claude.ai/artifact/3h5DV31PBKBD7pb3kgov5P
- **`reports/Golf snake oil satire strategy.md`:** the satire angle. Brand
  promise "Works as labeled. Cures nothing." An honest core (plain, dated,
  tested claims) in a patent-medicine costume (jokes only in names, labels and
  obviously impossible novelty items). It includes a voice guide, 55
  executions, a 90-day plan and legal guardrails.

## What exists

- Matt Pocock's skills plugin is enabled through `.claude/settings.json`.
- A pre-launch home page in `site/`, built with Astro 7.4. It has the logo,
  pitch, waitlist form and a "What Flay is" section, on a dark palette with
  Pirata One headings. It ships no JavaScript.
- The waitlist posts to Kit once `PUBLIC_KIT_FORM_ID` is set. Until then the
  form is built switched off and says signups open soon. The ID is read at
  build time, so redeploy after setting it; anything but a plain number fails
  the build.
- Fonts are self-hosted through `@fontsource`; nothing loads from Google.
- Link previews and the tab icon are generated from `src/assets/logo.png` at
  build time (canonical link, `og:image`, `og:url`, favicon).

## Changed in PR #7 (November 10, 2026)

A code review of the whole repo found ten issues in the home page and docs.
All ten are fixed:

- The waitlist no longer depends on a script to stop submissions when Kit
  isn't connected
- The Kit form ID is trimmed and checked at build time
- Button hover and input placeholder now pass contrast (5.7:1 and 7.1:1)
- Added the share image, canonical link, `og:url` and favicon
- The logo image picks the right size on tablets and loads first
- Fonts moved off Google Fonts to self-hosted files
- Footer drops the copyright year, which was frozen at build time
- Removed the unused `@astrojs/markdown-remark` dependency
- This handoff and `site/README.md` brought up to date

Not tested against a real Kit form yet. The share image keeps the logo's
proportions (1200 × 998), so some platforms crop it in previews; make a
1200 × 630 version once the final art exists.

## Waiting on the owner

- A Cloudflare Pages account connected to the repo
- A Kit account and form, then send the form ID
- Registering flaygolf.com
- Switching the GitHub default branch to `main` (it may still be
  `claude/install-external-skills-tbjvf3`)
- Hiring an illustrator with the brief. Tattoo artists are the first place to
  look.
- Testing the tee to find out which heights and clubs make it come off the mat
- A patent consult before the tee is shown, sold or offered anywhere
- A trademark check on "Dr. Flay" (the Bobby Flay association)

## Alternate logo in progress

A second mark: a grinning flayed golfer strapped to the X board, with a
newsboy cap, one golf glove, wingtip shoes, a club in each hand pointing down,
and a blackletter F cut into his chest with a teal glow. It was iterated in
Gemini; the latest direction uses leather straps instead of nails, a happy
grin and a crisp dark F. A human figure on an X board is close to House
Bolton's sigil, so keep it upright, earth-toned and clearly golf, and have a
trademark attorney look before printing. The final art must be drawn by a
person.

## Next up

The owner hasn't picked the order yet. Candidates:

- Build Plate I on the real site. Still open from the design report's fixes:
  move the button off blackletter, and give the button a visible edge
- The "Do now" items from the ideas report: waitlist live, the "why your mat
  tee keeps flying" guide, the tee height chart, haunted golf folklore shorts
- The satire 90-day plan, which aims at Black Friday (Nov 27) and Jan 30
- Another research round: verify blocked figures, ceiling and tee height data,
  merch costs, illustrator shortlist

## Key decisions

- Side business. Under $500 to start, 5–10 hours a week.
- No Shopify yet: custom site plus Stripe, moving to Shopify at about 10+
  orders a month.
- Don't publish how the tee works until the patent question is settled.
- Brand: DIY plus anti-country-club, beautiful-dark, earth tones with a teal
  accent.
- Logo: a flayed golf ball on an X-shaped flaying board, with an F-shaped wound
  showing a teal core.
- Satire stays in costume; real claims stay plain and testable. No edible or
  rub-on "remedies", no naming small competitors.

## Working notes

- The owner prefers one question at a time, with a recommended answer.
- Git: PRs are squash-merged. Merge `origin/main` into the branch before new
  work, because force-push gets blocked.
- Run `npm run build` in `site/` before pushing site changes; there is no CI.
- `astro preview` reuses an existing server on port 4321.
- For long research runs, save often: the owner asked for frequent commits
  and pushes, and a session can stop at a usage limit.
