# Flay Golf: handoff

Where things stand as of November 10, 2026, for whoever picks this up next.

**Repo:** `TraegerRuhter/FlayGolf`. Everything up to PR #6 is merged to `main`,
including three rounds of research, the site sketchbook and this file.

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
  Pirata One headings.
- The waitlist posts to Kit once `PUBLIC_KIT_FORM_ID` is set. Until then the
  form is switched off and says signups open soon.

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

- Build Plate I on the real site
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
- Git: the old branch's PRs were squash-merged. Merge `origin/main` into the
  branch before new work, because force-push gets blocked.
- `astro preview` reuses an existing server on port 4321.
- For long research runs, save often: the owner asked for frequent commits
  and pushes, and a session can stop at a usage limit.
