# Flay Golf: handoff

Where things stand as of October 31, 2026, for whoever picks this up next.

**Repo:** `TraegerRuhter/FlayGolf`. Everything up to PR #4 is merged to `main`.

## Read first

- `docs/flay-golf-plan.md`: the business plan, research, and open questions
- `docs/brand/brand.md`: voice, look, palette, type and logo
- `docs/brand/logo-brief.md` and `logo-reference.png`: the illustrator brief
  and reference image
- `site/README.md`: how to run the site, connect Kit, and deploy to Cloudflare
  Pages
- `CLAUDE.md` and `docs/agents/`: the issue tracker (GitHub Issues), triage
  labels, domain docs

## What exists

- Matt Pocock's skills plugin is enabled through `.claude/settings.json`.
- A pre-launch home page in `site/`, built with Astro 7.4. It has the logo,
  pitch, waitlist form and a "What Flay is" section, on a dark palette with
  Pirata One headings.
- The waitlist posts to Kit once `PUBLIC_KIT_FORM_ID` is set. Until then it
  says signups open soon.

## Waiting on the owner

- A Cloudflare Pages account connected to the repo
- A Kit account and form, then send the form ID
- Registering flaygolf.com
- Switching the GitHub default branch to `main` (it may still be
  `claude/install-external-skills-tbjvf3`)
- Hiring an illustrator with the brief. Tattoo artists are the first place to
  look.
- Testing the tee to find out which heights and clubs make it come off the mat

## Next on the site

The owner hasn't picked the order yet:

- Build log (the blog)
- Video page
- About page
- A locked "coming soon" shop using Stripe Payment Links

## Key decisions

- Side business. Under $500 to start, 5–10 hours a week.
- No Shopify yet: custom site plus Stripe, moving to Shopify at about 10+
  orders a month.
- Don't publish how the tee works until the patent question is settled.
- Brand: DIY plus anti-country-club, beautiful-dark, earth tones with a teal
  accent.
- Logo: a flayed golf ball on an X-shaped flaying board, with an F-shaped wound
  showing a teal core.

## Working notes

- The owner prefers one question at a time, with a recommended answer.
- Git: the old branch's PRs were squash-merged. Merge `origin/main` into the
  branch before new work, because force-push gets blocked.
- `astro preview` reuses an existing server on port 4321.
