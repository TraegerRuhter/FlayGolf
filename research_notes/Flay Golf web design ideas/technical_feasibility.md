# Technical Feasibility: a playful, art-heavy Flay Golf site on Astro + Cloudflare Pages

Research date: 2026-11-06. Scope: techniques, libraries, performance budgets, costs, and a feasibility rating for 20+ effects. Topics not covered: how the tee mechanism works.

**Method caveat:** WebFetch was blocked in this session. gsap.com, developers.cloudflare.com and caniuse.com did not resolve, so every finding below comes from search-result summaries of the cited pages, not full-page reads. Items drawn from the researcher's prior knowledge and not re-verified this session are labelled **[prior knowledge, unverified]** and appear only under Inferences or Gaps. Check prices and free-tier numbers on the live pages before committing.

**Current repo state (read directly):**
- `site/package.json`: `astro 7.4.0`, `@astrojs/markdown-remark 7.3.3` and `sharp 0.35.5`. There is no UI framework, no MDX integration and no Cloudflare adapter, so the site is purely static output.
- `site/astro.config.mjs`: contains only `site: "https://flaygolf.com"`.
- `src/layouts/Base.astro`:
  - Loads 4 Google Fonts through a third-party stylesheet link: Pirata One, IM Fell English (regular and italic), Spectral (400, 500 and italic) and DM Mono.
  - Defines palette tokens: `--night #120d0a`, `--bone #e7dcc6`, `--ochre`, `--rust`, `--paper #feecde`, `--teal #2a9aa1`.
  - Has a global `prefers-reduced-motion` rule that kills every animation and transition.
- `src/pages/index.astro`:
  - Uses `astro:assets` `<Image>` for `logo.png`, with widths 480, 760 and 1250 and `loading="eager"`.
  - The Kit waitlist form POSTs straight to `https://app.kit.com/forms/{PUBLIC_KIT_FORM_ID}/subscriptions`.
  - Copy uses the "Plate I." figure caption device.

---

## 1. Visual effects: SVG filters, scroll-driven CSS, view transitions, GSAP, Lenis, 3D, Rive/Lottie, shaders, cursors, audio, page-turns

### Takeaway
Most of the "aged engraving" look can be done with static SVG filters, CSS and pre-processed images at near-zero JS cost. Native CSS scroll-driven animations reached all three engines in late 2026. GSAP, including all its plugins, is free. The expensive items are animated SVG filters, the Rive runtime (about 220–650 KB) and three.js. OGL (about 20 KB) is the cheap path to one 3D "specimen".

### Cited Findings
**GSAP**
- Since GSAP 3.13 (spring 2025, after the Webflow acquisition), GSAP and every formerly paid "Club" plugin, including SplitText and MorphSVG, are free for commercial use. The plugins now ship in the main `gsap` npm package and the `gsap-trial` package is deprecated. — [GSAP 3.13 blog](https://gsap.com/blog/3-13/); [gsap-trial deprecation notice](https://cdn.jsdelivr.net/npm/gsap-trial@3.13.0/README.md); [Codrops demos of the free plugins](https://tympanus.net/codrops/2025/05/14/from-splittext-to-morphsvg-5-creative-demos-using-free-gsap-plugins/)
- The GSAP README points to a no-charge "standard license" at gsap.com/standard-license. — [GSAP README on unpkg](https://unpkg.com/gsap@3.13.0/README.md)
- A Webflow forum post says the earlier standard license needed Webflow's written permission for no-code animation tools that compete with Webflow. It is unconfirmed whether that clause survives, and it does not affect a brand site. — [Webflow forum](https://discourse.webflow.com/t/webflow-makes-gsap-100-free/319967)

**Scroll-driven animations**
- web.dev's October 2026 roundup says Firefox 159 turned on scroll-driven animations (`animation-timeline`) by default, making them "Baseline Newly available" across Chrome, Safari and Firefox. — [web.dev, New to the web platform in October 2026](https://web.dev/blog/web-platform-10-2026)
- Conflict: a mid-2026 blog said the feature was still behind a flag in Firefox 152 stable (June 2026). That fits a later 159 ship, but no Mozilla release note was found. — [buildmvpfast blog](https://www.buildmvpfast.com/blog/css-scroll-driven-animations-replace-js-2026)

**View transitions**
- Cross-document (multi-page) view transitions:
  - Supported in Chrome and Edge 126+ and Safari 18.2+.
  - Opt-in is `@view-transition { navigation: auto; }` on both the outgoing and the incoming page, and it works for same-origin navigations only.
  - Firefox support is unclear. One indexed version of Chrome's page lists Firefox 147 and Safari 26.2 for the `pageswap`/`pagereveal` events, while another lists Firefox as unsupported.

  — [Chrome for Developers, cross-document view transitions](https://developer.chrome.com/docs/web-platform/view-transitions/cross-document)

**Lenis smooth scroll**
- Since 1.3.26, Lenis respects `prefers-reduced-motion` by default (`respectReducedMotion: true`) and picks up a change to the setting live. Nested scrollers can opt out with `data-lenis-prevent`. Lenis wraps native scroll, so `position: sticky` and anchor links keep working. — [Lenis README](https://github.com/darkroomengineering/lenis/blob/main/README.md)
- A third-party checklist advises skipping Lenis on touch devices (`hover: none`) and warns that `scrollTop` can desync from the visual position for screen readers. This is not official guidance. — [skill-wiki Lenis template](https://skill-wiki.github.io/atom/frontend-design/template-lenis-smooth-scroll)

**Light 3D**
- OGL's README gives core at about 6 KB, math at about 7 KB and extras at about 7 KB gzipped, so roughly 20 KB for everything. Its API is similar to three.js but with far fewer features. — [OGL README (mirror)](https://github.com/luruke/ogl)
- three.js has been reported at about 140–150 KB gzipped. That figure comes from an older forum thread, and a third-party page says about 160 KB. — [three.js forum](https://discourse.threejs.org/t/bloated-js-file/16176); [skills.sh OGL page](https://www.skills.sh/avnehsbhatia/ultraui/ogl-lightweight-webgl)
- OGL does not suit work that needs glTF loading, PBR materials, shadows or post-processing chains (third-party). — [skills.sh OGL page](https://www.skills.sh/avnehsbhatia/ultraui/ogl-lightweight-webgl)

**Rive and Lottie**
- Official Rive web runtime sizes, brotli-compressed, with WASM making up most of the weight:

  | Runtime | Size |
  |---|---|
  | `canvas` | 567 KB |
  | `canvas-lite` | 222 KB |
  | `webgl2` | 648 KB |

  — [Rive runtime sizes](https://rive.app/docs/runtimes/runtime-sizes)
- Conflict: Rive's v2 migration note gives about 261 KB brotli for the default WASM. — [Rive v1 to v2 migration](https://rive.app/docs/runtimes/web/migrating-from-v1-to-v2)
- Rive says `.riv` files are typically 10–15x smaller than equivalent Lottie JSON, with one example of a 240 KB Lottie redone at 16 KB in Rive. This is a vendor claim. — [Rive blog](https://rive.app/blog/rive-as-a-lottie-alternative)
- LottieFiles says dotLottie is about 80% smaller than Lottie JSON and now supports state machines. Rive acknowledged Lottie state machines in December 2025. — [LottieFiles, LottieFiles or Rive](https://lottiefiles.com/blog/lottie-animations/lottiefiles-or-rive)

**SVG filters (paper grain, ink displacement)**
- Animating `feTurbulence` and `feDisplacementMap` runs smoothly on desktop but sluggishly on iOS, and gets worse over large areas or long durations. Changing a filter forces a repaint, whereas animating opacity only re-composites. — [GreenSock forum, feTurbulence mobile performance](https://greensock.com/forums/topic/33075-gsap-and-feturbulence-mobile-performance/)
- `feTurbulence` cost rises with `numOctaves` (default 1). A 2023 Firefox bug, still open, found the turbulence routine running in software and costing much more CPU than in Chrome. — [Mozilla bug 1845890](https://bugzilla.mozilla.org/show_bug.cgi?id=1845890); [Mozilla bug 1787793](https://bugzilla.mozilla.org/show_bug.cgi?id=1787793)
- Animated SVG filters can degrade badly on Safari. — [Medium, SVG noise filter part IV](https://medium.com/@codebro/svg-noise-filter-bc6247ba4399)

**Web Audio**
- An AudioContext created outside a user gesture starts "suspended". Create or `resume()` it inside a click handler. — [MDN Web Audio best practices (mirror)](https://docs.w3cub.com/dom/web_audio_api/best_practices); [Chromium intent to ship, autoplay policy for Web Audio](https://groups.google.com/a/chromium.org/g/blink-dev/c/5Y1BqbGauEs)

### Inferences
- **Paper grain:** use a static `feTurbulence` grain, rendered once and never animated, or better, pre-bake a small tiling grain PNG or WebP (about 5–20 KB) and lay it on as a CSS `background-image` with `mix-blend-mode`. Either gives the aged-paper texture for close to zero runtime cost. Keep any animated displacement, such as an ink "bleed" on hover, to small elements and short bursts, and use `numOctaves` of 1–2.
- **Scroll reveals:** use CSS `animation-timeline: view()` as a progressive enhancement, wrapped in `@supports (animation-timeline: view())`, with static content as the fallback. Bring in GSAP ScrollTrigger only for choreographed sequences such as pinned "plate" reveals or SplitText ink-writing headlines. GSAP core plus ScrollTrigger is roughly 40–50 KB gzipped **[prior knowledge, unverified]**.
- **Page transitions:** cross-document `@view-transition` costs no JS and degrades gracefully, since unsupported browsers just navigate normally. Astro's `<ClientRouter />` is the JS alternative if a cross-browser animated transition is needed **[prior knowledge, unverified]**. For a mostly single-page site, the native CSS route is enough.
- **Lenis:** skip it unless scroll-linked GSAP scenes need it. Native scroll plus CSS `scroll-behavior: smooth`, turned off under reduced motion, is lighter and avoids accessibility risk.
- **Golf-ball specimen:**
  - Prefer a pre-rendered image sequence or short looping video (WebM/MP4 about 300 KB–1 MB) of a rotating engraved ball.
  - Or use an OGL sphere carrying a hatching or halftone fragment shader, loaded `client:visible`, and only after the first frame of a static poster image.
  - three.js only earns its weight if a glTF model of a product is needed.
- **Animated illustrations:**
  - dotLottie with the lightweight `@lottiefiles/dotlottie-web` player suits one or two hero animations, and is cheaper to start with than Rive's WASM runtime.
  - For simple line "drawing-on" of engraving strokes, CSS or GSAP animation of `stroke-dashoffset` on SVG paths (DrawSVG) needs no runtime at all.
- **Engraving/halftone shader:** a single fragment shader over a hero image (OGL or raw WebGL, under 25 KB) is medium difficulty. Keep it optional and pause it when off-screen with IntersectionObserver.
- **Custom cursor:**
  - The simplest version is a CSS `cursor: url(scalpel.svg) 4 4, auto;` with no JS.
  - A JS follower (a div moved by `pointermove` with `transform`) is cheap, but only enable it on `(hover: hover) and (pointer: fine)`, and keep the native cursor visible for accessibility.
- **Sound:** short Opus/WebM clips of a few KB, such as a page rustle or nail tap, played via Web Audio only after the first click. Ship an obvious mute toggle that starts OFF and remember it in localStorage.
- **Page-turn and drawer interactions:**
  - The `<details>`/`<summary>` elements, or the `popover` attribute and `<dialog>`, give accessible drawers with no JS.
  - A CSS 3D `rotateY` gives a page-flip feel.
  - A realistic curl page-turn library (e.g. StPageFlip) is heavier and fiddly on mobile **[prior knowledge, unverified]**.

### Gaps
- Exact current GSAP license text: gsap.com was unreachable.
- Firefox support for cross-document view transitions: the sources conflict.
- No independent benchmark for compressed `.riv` against dotLottie.
- No measured size for `@lottiefiles/dotlottie-web` (its WASM renderer) was found.
- Current three.js gzipped size was not verified.

---

## 2. Astro specifics: islands, view transitions, content collections (build log), MDX, images, fonts, small interactive tools

### Takeaway
Astro is a natural fit. Pages ship zero JS by default, and each interactive widget becomes an island hydrated on demand. A build log is a content collection of Markdown/MDX files. Cloudflare now owns Astro, which secures Cloudflare hosting. The repo pins Astro 7.4, and Astro 7 details are only lightly sourced.

### Cited Findings
- **Ownership:** Cloudflare acquired The Astro Technology Company on 16 January 2026. Astro stays open source (MIT) and platform-independent. — [Cloudflare press release](https://www.cloudflare.com/press/press-releases/2026/cloudflare-acquires-astro-to-accelerate-the-future-of-high-performance-web-development/); [ChannelLife](https://channellife.com.au/story/cloudflare-buys-astro-framework-pledges-open-future)
- **Astro 6** added a redesigned dev server on Vite's Environments API that runs in the same runtime as production, including Cloudflare's workerd. — [bytes.dev, Astro 6.0 goes full Cloudflare](https://bytes.dev/archives/469)
- **Astro 7** reportedly brings a Rust-based compiler, Vite 8, stricter HTML checks, a dev-server lockfile for AI agents, and requires Node 22.12+. This comes from one German blog and is not corroborated. — [nevercodealone.de](https://nevercodealone.de/frontend-development/astro-js-static-site-generator-website-performance)
- **Islands:** `client:visible` defers hydration until the component scrolls into view, so a page with one such island loads essentially no JS up front. — [pkgpulse, RSC vs Astro islands 2026](https://www.pkgpulse.com/guides/rsc-vs-astro-islands-2026)
- One benchmark reports Preact cutting client bundle size by 42% compared with React 18. — [DEV Community benchmark](https://dev.to/johalputt/preact-vs-astro-4-the-performance-battle-benchmark-in-high-scale-5a7k)
- **Fonts API:**
  - Astro's Fonts API provides a `<Font />` component with a `preload` option, which the docs say to use sparingly.
  - It generates metric-matched fallback fonts automatically (`optimizedFallbacks`).
  - Some doc paths still sit under "experimental-flags", so whether it is stable in 7.x is unconfirmed.

  — [Astro docs, Using custom fonts](https://docs.astro.build/pl/guides/fonts/); [Astro experimental fonts flag](https://docs.astro.build/de/reference/experimental-flags/fonts)

### Inferences
- **Build log:**
  - Define `src/content.config.ts` with a `log` collection (glob loader over `src/content/log/*.md`) and a schema of title, date, plate number, cover image and tags. Add `@astrojs/mdx` only if posts need embedded components, such as a live tee-height chart inside a post **[prior knowledge, unverified]**.
  - This suits a non-developer: each entry is one Markdown file with front-matter, written with AI help.
- **Island framework:**
  - Use vanilla `<script>` tags in `.astro` components for most effects: they are bundled and deduplicated automatically.
  - Add `@astrojs/preact` (about 4 KB runtime **[prior knowledge, unverified]**) or `@astrojs/svelte` only for stateful tools such as the quiz or calculator.
  - Pick one framework only, and avoid React for size.
- **Images:**
  - `astro:assets` `<Picture formats={['avif','webp']}>` produces responsive AVIF/WebP at build time with sharp, which is already installed **[prior knowledge, unverified]**.
  - Fine engraving line art is better as SVG (vectorised with potrace or vtracer) or lossless WebP or PNG at the right size. AVIF and WebP lossy compression can blur hairline hatching, so A/B test at quality 70–85.
- **Fonts:**
  - Swap the Google Fonts stylesheet link for self-hosted WOFF2 files via the Fonts API or `@fontsource` packages. This drops two third-party connections, which helps LCP, and lets Cloudflare serve fonts from the same origin.
  - Preload only the display face used in the hero `h1` (Pirata One) and maybe Spectral 400. Subset to Latin.
  - Keep `font-display: swap` and use the metric-matched fallbacks to protect CLS.

### Gaps
- No official Astro 7 changelog was read: docs.astro.build/blog was not fetched.
- Whether the Fonts API is stable in 7.4 is unconfirmed.
- No reliable Preact vs Svelte island size benchmark was found.

---

## 3. Interactive tools: ceiling-height/swing calculator, tee height chart, quiz

### Takeaway
All three are small client-side widgets that need no backend. A vanilla JS `<form>` with `oninput` handlers, or one Preact/Svelte island, is the simplest build. A chart can be static SVG generated at build time.

### Cited Findings
- `client:visible` hydration means an island lower down the page costs nothing until it is scrolled to. — [pkgpulse](https://www.pkgpulse.com/guides/rsc-vs-astro-islands-2026)
- Both the Preact and Svelte runtimes are small, so component code tends to matter more than framework choice (synthesised from search results). — [raviswdev blog on Preact in Astro](https://raviswdev.blogspot.com/2026/02/preact-in-context-of-astro-and-also-in.html)

### Inferences
- **Ceiling/swing calculator:**
  - Inputs: user height, club, and ceiling height. Output: a clearance estimate plus an engraving-style SVG figure whose arm arc scales with the inputs.
  - Build it as a plain HTML form plus about 50 lines of vanilla JS using `<output>` elements, so it is accessible by default.
  - The swing-arc formulas need sourcing or testing by the owner. They are not covered in this research.
- **Tee height chart:**
  - Render it as an inline SVG at build time from a JSON or YAML data file in a content collection. It needs zero JS and the owner can edit it.
  - Optional hover highlights in CSS.
- **Quiz** (for example "which Flay specimen are you"):
  - Plain radio-button `<form>` steps, either native `<details>` steps or one Preact island.
  - Results map to static result pages, which are shareable URLs with their own OG images.
  - It can optionally pass the result to Kit as a custom field (see section 4).
- **Countdown to a drop:** about 20 lines of vanilla JS against a fixed ISO date. Render the date as server-side text so it still works without JS.

### Gaps
- No golf-specific formula sources were researched. This was out of scope.

---

## 4. Waitlist mechanics on a static site: Kit referrals, numbered positions, countdowns

### Takeaway
Kit Free (up to 10,000 subscribers) covers the basic waitlist. Kit's built-in referral system (via SparkLoop) is Pro-only, at about $79/mo. A numbered "you are #217" position needs a small Cloudflare Worker with D1 that sits between the form and Kit. It fits comfortably in the free tier.

### Cited Findings
- **Kit Free:** $0 up to 10,000 subscribers, with unlimited forms, landing pages and broadcasts. Above 10,000 you must upgrade to keep sending. — [Kit Help, The Kit Free plan](https://help.kit.com/en/articles/16627071-the-kit-free-plan)
- **Referrals:** the newsletter referral system ("refer friends, get rewards", with a shareable link) comes with a free SparkLoop plan valued at $99/mo, and is available on Pro only, not on Free or Creator. — [Kit pricing](https://kit.com/pricing)
- **Pro trial:** Pro has a 14-day free trial with no card. A 25% first-year discount on Pro annual ended on 7 November 2026. — [Kit Pro](https://kit.com/pro)
- **Paid plan prices:** third-party sources put Creator at about $39/mo ($390/yr) and Pro at about $79/mo ($790/yr) for 1,000 subscribers, scaling with list size. Some sites list lower, apparently outdated, numbers. — [Mailmeteor, Kit pricing](https://mailmeteor.com/blog/kit-pricing); [Learning Revolution, Kit review](https://www.learningrevolution.net/kit-review/)
- **Kit API v4:**
  - `POST /v4/subscribers` upserts a subscriber by email and accepts a `fields` object keyed by custom-field key.
  - Custom fields are created with `POST /v4/custom_fields`.
  - `POST /v4/forms/{form_id}/subscribers` adds a subscriber to a form by email.
  - Webhooks include `subscriber.form_subscribe`.
  - The docs conflict on whether an unknown field key errors or is ignored with a warning.

  — [Kit API, Create a subscriber](https://developers.kit.com/api-reference/subscribers/create-a-subscriber); [Kit API, Create a custom field](https://developers.kit.com/api-reference/custom-fields/create-a-custom-field); [Kit API, Add subscriber to form](https://developers.kit.com/api-reference/forms/add-subscriber-to-form); [Kit API, Create a webhook (legacy)](https://developers.kit.com/api-reference/webhooks-legacy/create-a-webhook)
- **Workers Free:**
  - 100,000 requests a day, resetting at 00:00 UTC (Error 1027 when exceeded).
  - 10 ms CPU per request.
  - Burst limit of 1,000 requests per minute.

  — [Cloudflare Workers limits](https://developers.cloudflare.com/workers/platform/limits)
- **KV Free:** 100,000 key reads a day, 1,000 writes a day, 1 GB stored. — [Cloudflare KV pricing](https://developers.cloudflare.com/kv/platform/pricing)
- **D1 Free:** about 5 million rows read a day, 100,000 rows written a day and 5 GB storage. These come from third-party sources, not the official page. A report says Cloudflare began strictly enforcing D1 free limits on 1 September (year unstated). — [eastondev, Cloudflare free limits](https://eastondev.com/blog/en/posts/dev/20260526-cloudflare-free-limits/); [tproger](https://tproger.ru/news/cloudflare-nachala-otklyuchat-d1-na-besplatnom-plane-pri-prevyweni)

### Inferences
- **Numbered position architecture:**
  1. The form POSTs to `/api/join`, a Pages Function or Worker.
  2. The Worker runs `INSERT INTO waitlist(email) ... RETURNING id`; an autoincrement id gives a race-free sequence number.
  3. The Worker upserts the subscriber in Kit through API v4 with a custom field such as `waitlist_no`, and adds them to the form so the existing Kit incentive email fires.
  4. The Worker redirects to `/joined?n=217`.

  Kit emails can then merge the field, e.g. "You're specimen No. 217". The Kit API key lives as a Worker secret. Use D1, not KV: KV's 1,000 writes a day and eventual consistency make it a poor counter **[prior knowledge on KV consistency, unverified]**. Add Cloudflare Turnstile, which is free **[prior knowledge, unverified]**, to stop bot signups.
- **DIY referral without Kit Pro:** the Worker can give each signup a `ref` code (`/?ref=abc`). It stores `referred_by` in D1 and writes the referral count back to Kit as a custom field. A "move up the list" mechanic is a D1 query, so it costs nothing extra but means writing and maintaining code. The alternative is Kit Pro at about $79/mo, worth it only if referrals become the core growth loop.
- **Capacity:** 100k requests a day is far beyond what a launch waitlist needs. 1,000 signups a day means about 1,000 D1 writes plus 1,000 Kit API subrequests.
- The current direct-to-Kit form POST keeps working as a no-backend fallback if the Worker is not built.

### Gaps
- Exact current Kit Creator and Pro prices: kit.com/pricing was not fetched in full.
- Kit API rate limits were not checked.
- Official D1 free-tier numbers and the Durable Objects free tier were not verified, because developers.cloudflare.com was unreachable.
- Kit Creator Network or "Recommendations" features were not researched.

---

## 5. Commerce: Stripe Payment Links / Buy Buttons, limited numbered drops, Workers logic, Shopify alternatives

### Takeaway
Stripe Payment Links work on a static site with no backend. A built-in "limit the number of payments" setting switches the link off automatically, which gives a crude inventory cap for a numbered drop. Assigning each buyer a number needs a webhook-driven Worker. Shopify Starter (about $5/mo) with the Buy Button is the alternative if real inventory or shipping management is wanted.

### Cited Findings
- Payment Links can "Limit the number of payments". When the limit is reached the link deactivates automatically, and the message shown can be customised. The API equivalent is `restrictions[completed_sessions][limit]` plus `inactive_message`. — [Stripe docs, Customize checkout for Payment Links](https://docs.stripe.com/payment-links/customize); [French version with the API params](https://docs.stripe.com/payment-links/customize?locale=fr-FR)
- The cap counts payments, not stock units. For per-unit inventory, a third-party article suggests per-purchase Checkout Sessions handled through the `checkout.session.completed` webhook. — [Stripe docs](https://docs.stripe.com/payment-links/customize); [s-plugins article](https://s-plugins.com/setting-limited-quantity-for-products/)
- Stripe's no-code options are summarised here. — [Stripe no-code docs](https://docs.stripe.com/no-code/get-started)
- The Shopify Buy Button reportedly works on the $5/mo Starter plan. It is embedded with an SDK script plus a product snippet pasted into static HTML. These are third-party sources only, and one mentions an older $9 Lite plan that conflicts. — [Style Factory, Shopify Starter review](https://www.stylefactoryproductions.com/blog/shopify-starter-plan-review); [letstalkshop, Buy Button on external site](https://www.letstalkshop.com/blog/shopify-buy-button-on-external-website)

### Inferences
- **Numbered drop of N pieces (for example a "Plate II" edition of 50):**
  1. Create one Payment Link with the quantity fixed at 1 and the payment limit set to N.
  2. A Worker receives the `checkout.session.completed` webhook, verifies the Stripe signature, and inserts a D1 row with an autoincrement edition number.
  3. The buyer gets "No. 17 of 50" via the Stripe receipt metadata or a Kit tag or email.
  4. A site badge reading "31 of 50 left" can read a cached count from the Worker, or simply be updated by hand.

  Watch for: several buyers in checkout at the same moment may oversell slightly past the limit. This was not verified. Test it in Stripe test mode, or keep a few spare units.
- **Costs:** Stripe has no monthly fee and takes per-transaction fees only (standard US card pricing not re-verified this session). Workers and D1 stay within the free tier at this scale.
- **Countdown plus drop gating:** the simplest version is to keep the Payment Link button hidden or disabled until the drop time, using client JS and a rebuild. A Worker serving the link only after the drop time is more robust.
- **Platform choice:** use Shopify only if the product catalogue grows past a handful of SKUs or managed shipping and tax is needed. For a few numbered drops, Stripe Payment Links plus a Worker is cheaper and keeps the custom art direction.

### Gaps
- Stripe's oversell behaviour with concurrent sessions near the limit was not found.
- Stripe Buy Button embed specifics (`<stripe-buy-button>`) and its script weight were not researched.
- Official current Shopify Starter pricing was not checked.
- Stripe Tax and shipping-rate setup for physical goods was not covered.

---

## 6. Performance and accessibility budgets

### Takeaway
Target Google's "good" Core Web Vitals at p75: LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1. An art-heavy page can meet these if the hero plate is a properly sized AVIF/WebP or SVG, fonts are self-hosted and few are preloaded, and all motion is progressive, honouring `prefers-reduced-motion`. The site already has that kill switch.

### Cited Findings
- The "good" thresholds, judged at the 75th percentile of page loads:

  | Metric | Good | Poor |
  |---|---|---|
  | LCP | ≤ 2.5 s | > 4 s |
  | INP | ≤ 200 ms | > 500 ms |
  | CLS | ≤ 0.1 | > 0.25 |

  INP replaced FID in 2024. — [web.dev, Web Vitals](https://web.dev/articles/vitals); [web.dev, Defining Core Web Vitals thresholds](https://web.dev/articles/defining-core-web-vitals-thresholds); [web.dev, INP](https://web.dev/articles/inp)
- Preload fonts sparingly, because preloading can block other critical resources. — [Astro docs, fonts](https://docs.astro.build/pl/guides/fonts/)
- Lenis honours reduced motion by default. — [Lenis README](https://github.com/darkroomengineering/lenis/blob/main/README.md)
- Animated SVG filters are costly on mobile. — [GreenSock forum](https://greensock.com/forums/topic/33075-gsap-and-feturbulence-mobile-performance/)

### Inferences (proposed budgets; researcher judgement, not sourced standards)
- **Page weight on first load (home):** about 600–900 KB total, of which:
  - HTML and CSS under 50 KB;
  - JS under 100 KB gzipped, or 0 KB for the first view;
  - fonts under 150 KB;
  - the hero image under 200 KB.
- **Hero engraving plate:**
  - `<Picture>` AVIF plus WebP fallback, widths 480/760/1250 (already used), `fetchpriority="high"`, and explicit width and height.
  - For pure line art, test vectorised SVG; keep it under about 150 KB gzipped, or simplify paths.
  - Lazy-load below-the-fold plates.
- **Fonts:**
  - Four families at Latin subset WOFF2 come to roughly 20–40 KB per file. The current request loads 7 or more files, counting Spectral weights and italics.
  - Trim Spectral to 400 plus 400 italic if possible.
  - Self-host, preload at most 1–2 files, and use metric-matched fallbacks for CLS.
- **Motion:** CSS-first. Every JS animation checks `matchMedia('(prefers-reduced-motion: reduce)')`. Avoid animating filter or blur over large areas. Pause canvas and WebGL when off-screen or when the tab is hidden.
- **Accessibility:**
  - Contrast: bone `#e7dcc6` on night `#120d0a` is very high contrast. Teal `#2a9aa1` on night should be checked for body text, since it is likely only AA-large **[unverified, compute]**.
  - Blackletter Pirata One should be used only for display, with plain text alternatives.
  - The custom cursor must not hide the system cursor's function.
  - Sound is off by default.
  - Drawers use native `<dialog>`/`<details>`.
  - Decorative SVG gets `aria-hidden`.
- **Measurement:** Lighthouse in Chrome DevTools, PageSpeed Insights, and Cloudflare Web Analytics, which is free **[prior knowledge, unverified]**.

### Gaps
- No sourced guidance on AVIF quality settings for line-engraving art was found. A/B test it.
- WCAG contrast ratios for the palette were not computed in this session.

---

## 7. Hosting costs and free-tier limits

### Takeaway
At launch scale the whole stack can run at about $0/mo: Cloudflare Pages static hosting, Workers, D1, Kit Free and Stripe, which charges per transaction only. Upgrade triggers are Kit above 10,000 subscribers, a wish for Kit referrals (Pro), or Workers traffic above 100k requests a day. Workers Paid is roughly $5/mo **[prior knowledge, unverified]**.

### Cited Findings
- **Cloudflare Pages Free:**
  - 500 builds a month, 1 concurrent build, 20-minute build timeout, 100 custom domains per project.
  - Static asset requests and bandwidth are described as unlimited.
  - Pages Functions requests count against the Workers quota.

  — [Cloudflare Pages limits](https://developers.cloudflare.com/pages/platform/limits); [temps.sh, Cloudflare Pages free tier 2026](https://temps.sh/blog/cloudflare-pages-free-tier-limits-2026)
- **Workers Free:**
  - 100k requests a day and 10 ms CPU per request.
  - 50 subrequests per invocation.
  - The daily limit is account-wide.
  - Workers projects are capped at 100 on Free.

  — [Cloudflare Workers limits](https://developers.cloudflare.com/workers/platform/limits); [Cloudflare plans](https://www.cloudflare.com/plans.md)
- **KV and D1:** KV Free is 100k reads and 1k writes a day with 1 GB stored. D1 Free is about 5M rows read and 100k rows written a day with 5 GB (third-party). — [Cloudflare KV pricing](https://developers.cloudflare.com/kv/platform/pricing); [eastondev](https://eastondev.com/blog/en/posts/dev/20260526-cloudflare-free-limits/)
- **Pages vs Workers static assets:** Cloudflare now promotes Workers Static Assets for static sites. One blog describes migrating from Pages as a small config change. — [kahwee blog, migrating from Pages to Workers](https://kahwee-com.pages.dev/2025/migrating-from-cloudflare-pages-to-workers)
- **Kit:** Free up to 10k subscribers. Pro is about $79/mo, from third-party sources. — [Kit Help](https://help.kit.com/en/articles/16627071-the-kit-free-plan); [Mailmeteor](https://mailmeteor.com/blog/kit-pricing)
- **Shopify Starter:** about $5/mo (third-party). — [Style Factory](https://www.stylefactoryproductions.com/blog/shopify-starter-plan-review)

### Inferences
- **Monthly cost:**

  | Stage | Cost | What drives it |
  |---|---|---|
  | Launch | $0 | Domain registration about $10–15/yr extra, via Cloudflare Registrar at cost **[prior knowledge]** |
  | Growth | about $79/mo | Kit Pro, only if built-in referrals are wanted |
  | Growth | about $5/mo | Workers Paid, only if traffic spikes past 100k requests a day |

- **New project setup:** consider deploying as a Worker with static assets (`wrangler.jsonc` `assets` block) rather than classic Pages, because that is where Cloudflare is investing. Either works. 500 builds a month is plenty for a single owner pushing via Git.
- **Free-tier failure mode:** when a free limit is exceeded, Workers error (1027) instead of billing. For a waitlist endpoint, keep the direct Kit form action as a no-JS fallback so signups never fail.

### Gaps
- Official Workers Paid price and D1 or Durable Objects free numbers were not verified this session.
- Whether Pages remains a separately supported product in 2026 was not confirmed.

---

## 8. Feasibility ratings: 24 effects and features

Ratings are the researcher's synthesis of the findings above, for one non-developer owner working with AI help.
- **Difficulty:** Easy means CSS/HTML or a copy-paste snippet. Medium means some JS or config plus testing. Hard means custom shaders or a backend, with real debugging.
- **Perf cost:** None (0 KB JS, no runtime cost), Low (under 15 KB or a one-time paint), Med (15–80 KB or ongoing paint), High (over 80 KB, or continuous GPU/CPU work).

### Takeaway
Of the 24 items, 13 are Easy, 8 Medium and 3 Hard. The highest-impact, lowest-cost combination is static paper grain, SVG ink-line draw-ons, CSS scroll reveals, native view transitions, a CSS cursor, the build-log collection, a vanilla calculator and Stripe Payment Links with a cap. The Hard items are the WebGL halftone shader, the 3D specimen and the realistic page-curl; save them for later.

### Cited Findings
- See sections 1–7 for the supporting citations. Key ones: [GSAP 3.13](https://gsap.com/blog/3-13/), [web.dev October 2026](https://web.dev/blog/web-platform-10-2026), [Chrome view transitions](https://developer.chrome.com/docs/web-platform/view-transitions/cross-document), [Rive runtime sizes](https://rive.app/docs/runtimes/runtime-sizes), [OGL README](https://github.com/luruke/ogl), [Stripe Payment Links customize](https://docs.stripe.com/payment-links/customize), [Kit pricing](https://kit.com/pricing), [Cloudflare Workers limits](https://developers.cloudflare.com/workers/platform/limits).

### Inferences

| # | Effect / feature | Recommended approach | Difficulty | Perf cost |
|---|---|---|---|---|
| 1 | Aged-paper grain overlay | Pre-baked tiling WebP plus `mix-blend-mode`, or a static `feTurbulence` | Easy | None–Low |
| 2 | Animated ink-bleed / displacement on hover | `feDisplacementMap` on small elements, short duration, numOctaves ≤2 | Medium | Med (High if large or iOS) |
| 3 | Engraving lines "drawing on" | SVG `stroke-dashoffset` via CSS or GSAP DrawSVG | Easy–Medium | Low |
| 4 | Scroll-triggered plate reveals | CSS `animation-timeline: view()` inside `@supports` | Easy | None |
| 5 | Pinned / choreographed scroll scenes | GSAP ScrollTrigger island | Medium | Med (~40–50 KB, unverified) |
| 6 | Ink-written headline (split letters) | GSAP SplitText, now free | Medium | Med |
| 7 | Page-to-page transitions | Native `@view-transition { navigation: auto }` | Easy | None |
| 8 | Smooth scroll | Native CSS; Lenis only if needed with GSAP | Easy | None (Lenis Low) |
| 9 | Rotating golf-ball "specimen" | Pre-rendered loop video/WebP sequence (easy) or OGL sphere with hatch shader (hard) | Easy / Hard | Med / Med–High |
| 10 | Engraving/halftone WebGL shader on hero art | OGL or raw WebGL fragment shader, paused off-screen | Hard | Med–High |
| 11 | Animated character illustration | dotLottie player, or Rive if interactive state machines are needed | Medium | Med (Rive 222–648 KB brotli) |
| 12 | Custom cursor (scalpel / pen nib) | CSS `cursor: url()`; JS follower only on fine pointers | Easy | None / Low |
| 13 | Subtle sound (paper rustle, nail tap) | Web Audio after first click, mute default on | Medium | Low |
| 14 | Drawer / "specimen cabinet" panels | `<details>`, `<dialog>`, `popover` | Easy | None |
| 15 | Page-flip "anatomy book" | CSS 3D rotate (easy); realistic curl library (hard) | Medium / Hard | Low / Med |
| 16 | Build-log blog | Astro content collection plus optional MDX | Easy | None |
| 17 | Responsive optimised art plates | `astro:assets` `<Picture>` AVIF/WebP or vectorised SVG | Easy | Low |
| 18 | Self-hosted 4 webfonts | Astro Fonts API or @fontsource, preload 1–2 | Easy | Low |
| 19 | Ceiling-height / swing calculator | Vanilla JS form plus SVG figure (or Preact island) | Easy–Medium | Low |
| 20 | Tee height chart | Build-time inline SVG from a data file | Easy | None |
| 21 | Personality quiz | Static form steps or Preact island → result pages → Kit field | Medium | Low |
| 22 | Waitlist signup (basic) | Existing Kit form POST | Easy | None |
| 23 | Numbered waitlist position plus DIY referrals | Worker + D1 + Kit API v4 custom fields + Turnstile | Hard (for a non-dev) | Low (server side) |
| 24 | Drop countdown | Vanilla JS against an ISO date, with a server-rendered fallback | Easy | None |
| 25 | Limited numbered drop checkout | Stripe Payment Link with payment limit (easy); edition numbering via webhook Worker + D1 (medium) | Easy / Medium | None |
| 26 | Shopify alternative | Shopify Starter plus Buy Button embed | Easy | Med (SDK script, unmeasured) |
| 27 | Reduced-motion compliance | Already-present global CSS kill switch plus JS checks | Easy | None |

Suggested phasing:
1. **Launch (all Easy):** items 1, 3, 4, 7, 12, 14, 16–18, 22, 24, 27.
2. **Next:** items 19–21, 25 and 13.
3. **Later or optional:** items 2, 5, 6, 9 (video version first), 10, 11, 15, 23.

### Gaps
- Bundle sizes marked "unverified" (GSAP core plus ScrollTrigger, dotLottie web player, Shopify Buy Button SDK) should be measured with `astro build` and a bundle visualiser before committing.
- None of these ratings has been prototyped. They are desk estimates.
