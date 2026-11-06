# Technical Feasibility: art-heavy playful site on Astro + Cloudflare Pages (WORK IN PROGRESS)

Note: WebFetch blocked in this session (gsap.com, developers.cloudflare.com, caniuse.com unreachable); findings come from search-result snippets. Verify against primary docs.

## Current setup (read from repo)
- site/package.json: astro 7.4.0, @astrojs/markdown-remark 7.3.3, sharp 0.35.5. No UI framework, no MDX, no adapter. Static output.
- astro.config.mjs: only `site: "https://flaygolf.com"`.
- src: layouts/Base.astro (Google Fonts: Pirata One, IM Fell English (ital), Spectral 400/500/ital, DM Mono; global prefers-reduced-motion kill switch), pages/index.astro (astro:assets <Image> of logo.png, Kit form POST to app.kit.com/forms/{PUBLIC_KIT_FORM_ID}/subscriptions), assets/logo.png.

## Kit referral
- Kit referral system is Pro-only (~$79/mo at 1k subs; $790/yr), bundled with SparkLoop free plan; free Newsletter plan up to 10,000 subs; Creator $39/mo — third-party (https://www.learningrevolution.net/kit-review/, https://mailmeteor.com/blog/kit-pricing). Unverified vs kit.com/pricing.

## Raw findings batch 1
- GSAP: all plugins (SplitText, MorphSVG, etc.) free incl. commercial since 3.13 (Apr/May 2025) after Webflow acquisition; gsap-trial deprecated; standard license "no charge"; old clause re no-code animation tools competing with Webflow — https://gsap.com/blog/3-13/ , https://unpkg.com/gsap@3.13.0/README.md , https://tympanus.net/codrops/2025/05/14/from-splittext-to-morphsvg-5-creative-demos-using-free-gsap-plugins/ , https://discourse.webflow.com/t/webflow-makes-gsap-100-free/319967
- CF Workers free: 100k req/day (UTC reset, Error 1027 over), 10 ms CPU/req, 1000 req/min burst; KV free 100k reads/day, 1k writes/day, 1 GB; D1 free ~5M rows read/day, 100k rows written/day, 5 GB (third-party) — https://developers.cloudflare.com/workers/platform/limits , https://developers.cloudflare.com/kv/platform/pricing , https://eastondev.com/blog/en/posts/dev/20260526-cloudflare-free-limits/ ; D1 free-limit enforcement report https://tproger.ru/news/cloudflare-nachala-otklyuchat-d1-na-besplatnom-plane-pri-prevyweni
- CF Pages free: 500 builds/mo, 1 concurrent build, 100 custom domains/project, 20 min build timeout; static requests/bandwidth unlimited; Functions count against Workers quota — https://developers.cloudflare.com/pages/platform/limits ; Workers Static Assets migration — https://kahwee-com.pages.dev/2025/migrating-from-cloudflare-pages-to-workers
- Scroll-driven animations: web.dev Oct 2026 says Firefox 159 enables by default -> Baseline Newly available — https://web.dev/blog/web-platform-10-2026 ; earlier flag status https://www.buildmvpfast.com/blog/css-scroll-driven-animations-replace-js-2026
- Cross-document view transitions: Chrome/Edge 126+, Safari 18.2+; Firefox status conflicting (147 listed for pageswap/pagereveal) — https://developer.chrome.com/docs/web-platform/view-transitions/cross-document
- Stripe Payment Links: "Limit the number of payments" auto-deactivates link; API restrictions[completed_sessions][limit]; custom inactive_message — https://docs.stripe.com/payment-links/customize
- Cloudflare acquired Astro Jan 16 2026; MIT, platform-independent — https://www.cloudflare.com/press/press-releases/2026/cloudflare-acquires-astro-to-accelerate-the-future-of-high-performance-web-development/ ; Astro 6 Vite Environments dev server — https://bytes.dev/archives/469 ; Astro 7 Rust compiler/Vite 8/Node 22.12+ (single German blog, unverified) https://nevercodealone.de/frontend-development/astro-js-static-site-generator-website-performance
- Rive vs Lottie: Rive claims .riv 10–15x smaller than Lottie JSON (240KB->16KB example) — https://rive.app/blog/rive-as-a-lottie-alternative ; dotLottie ~80% smaller than JSON, has state machines — https://lottiefiles.com/blog/lottie-animations/lottiefiles-or-rive

## Raw findings batch 2
- Astro Fonts API: <Font /> component with preload prop; optimizedFallbacks auto-generates metric-matched fallback; was under experimental-flags path — https://docs.astro.build/pl/guides/fonts/ , https://docs.astro.build/de/reference/experimental-flags/fonts
- Rive web runtime sizes (official): canvas 567KB brotli; canvas-lite 222KB brotli; webgl2 648KB brotli; v2 note says ~261kb brotli default wasm — https://rive.app/docs/runtimes/runtime-sizes , https://rive.app/docs/runtimes/web/migrating-from-v1-to-v2
- OGL ~6KB core + 7KB math + 7KB extras gzipped (~20KB) — https://github.com/oframe/ogl (README mirror https://github.com/luruke/ogl); three.js ~140–160KB gzipped (forum, older) — https://discourse.threejs.org/t/bloated-js-file/16176
- Lenis: respectReducedMotion default true since 1.3.26; data-lenis-prevent for nested scrollers; wraps native scroll so sticky/anchors work — https://github.com/darkroomengineering/lenis/blob/main/README.md ; third-party advice to skip on touch — https://skill-wiki.github.io/atom/frontend-design/template-lenis-smooth-scroll
