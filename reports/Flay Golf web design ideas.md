# Pin the site to one specimen plate

Flay Golf's website should be unique in its **system**, useful in its **tools**, and fast underneath. The system is a numbered anatomical plate with a lettered legend, set in a 1920s clubhouse that has been taken over, and it should work as structure, not as surface decoration. The tools are free sim-golf calculators and charts that answer real search questions without asking for an email first. Underneath sits a near-zero-JS Astro page that loads in under 2.5 seconds on a phone. No golf brand found in the research owns an antique, engraved or museum-dark web look. The new-wave labels run fairly standard storefronts, and the dark golf goods that exist are cartoon skulls. That gap is Flay's opening, and the cheapest way to hold it is typographic and structural (catalogue numbers, plate captions, a ledger, a scorecard) rather than WebGL. The ten concepts below give ten different organizing metaphors, each drawn top to bottom so it can be sketched. The recommendation is to build **Concept 1, "Plate I"**, first, because it is the current site taken further, and then borrow the register sign-up ritual (7), the field-journal build log (6), the consulting-room tools hub (9), the club rule book (2) and the drop register (8). The triptych (4) and the catalogue view (10) wait for a limited drop and a real product line. At 5–10 hours a week the core takes about three months. Before any new art goes in, there are four accessibility and speed fixes the current page needs. **Method caveat:** the researchers' web fetches were blocked. Nearly every external fact below comes from search-result summaries of the cited pages. Figures marked *(search summary)* or *[unverified]* need checking before anyone relies on them. The contrast ratios were computed for this report from the site's own color tokens.

## The current page is a strong, fast base with four fixable flaws

The live page (`site/src/pages/index.astro`) has one header, a "Plate I" engraving hero, a Kit email form posting straight to `app.kit.com`, a three-item creed and a footer. It ships almost no JavaScript. That matters more than any effect added later: in a controlled A/B test, Rakuten 24's Core Web Vitals-optimized page lifted **conversion rate 33%** and **revenue per visitor 53%** with no visual changes ([web.dev](https://web.dev/case-studies/rakuten)). This is large-retailer evidence, and no data exists for tiny waitlist pages, but it is the only playful-versus-functional question with A/B evidence behind it. Google's "good" thresholds at the 75th percentile are **LCP ≤ 2.5 s, INP ≤ 200 ms, CLS ≤ 0.1** ([web.dev](https://web.dev/articles/vitals)).

Computing WCAG contrast from the tokens in `Base.astro` gives a mostly healthy palette with four problems. Bone `#e7dcc6` on night `#120d0a` is **14.2:1**. It is off-white rather than pure white, which design-system guidance prefers for readers who see halation ([Trimble Modus](https://modus-v1.trimble.com/foundations/dark-mode/)). Bone-dim body copy is 7.1:1 on night and 6.7:1 on night-2. Ochre labels are 6.4:1. Teal on night is 5.7:1. The problems:

| Issue in current code | Measured | Fix |
|---|---|---|
| Button **hover** turns the fill to `--teal` and keeps bone text | **2.48:1, fails AA** | On hover use night text on teal (5.73:1), or keep `--teal-deep` and add an underline or inset ring |
| Input placeholder `#7d705c` on night | 3.99:1, below 4.5 | Raise to `--bone-dim` (7.1:1) or drop the placeholder, since a visible label exists |
| Submit button set in `--display` (Pirata One) at 21px | Blackletter on a control | Switch to Spectral 500 or DM Mono caps. Keep blackletter at 40px and up |
| `--teal-deep` button edge against night | 2.45:1 | Add a 1px bone border so the control's boundary reaches 3:1 (WCAG 1.4.11) |

Two traps for future art: **teal on the paper plate is only 2.93:1 and ochre on paper is 2.63:1**, so callout text drawn on the aged-paper background must use `--teal-deep` (6.86:1) or night ink. Rust on night is 3.2:1, so use it for large type and rules only.

Speed has three gaps. Fonts load from Google through a third-party stylesheet: four families, seven or more files. Self-hosting them via Astro's Fonts API or `@fontsource`, preloading only Pirata One and Spectral 400, removes two outside connections ([Astro docs](https://docs.astro.build/pl/guides/fonts/)). Whether the Fonts API is stable in 7.4 is unconfirmed. The hero `<img>` is `loading="eager"` but has no `fetchpriority="high"`. Its 1250w WebP is **220 KB**, slightly over a 200 KB hero budget. Last, the hero art is the AI-generated logo reference. The brand doc says the final mark must be drawn by a person, so every concept below assumes commissioned art for anything Flay needs to own.

## Ten guardrails decide whether play helps or hurts

The research agrees on one rule: **the core task (read, sign up, buy) must never depend on the playful layer.** Play helps when it gives feedback, rewards curiosity off the main path, or *is* the useful thing. It hurts when it takes away control or hides meaning. NN/g's usability testing found that scrolljacking harms "user control and freedom, discoverability, attention, efficiency, and task success". On one fully scrolljacked page, **every participant was disoriented** ([NN/g](https://www.nngroup.com/articles/scrolljacking-101/), search summary). NN/g separates this from *scroll fading*, where elements animate in as they arrive and the user keeps control ([NN/g](https://www.nngroup.com/articles/scroll-fading-101/)). Every concept below fades and never hijacks.

The guardrails, in priority order:

1. **Speed budget.** First load of the home page is about 600–900 KB. HTML and CSS stay under 50 KB, first-view JS is 0 KB, fonts stay under 150 KB, and the hero image under 200 KB. These are researcher judgments, not standards.
2. **No scrolljacking, no preloader, no intro animation, no autoplay sound, no icon-only nav.**
3. **CSS first.** Paper grain is one pre-baked tiling WebP (5–20 KB) with `mix-blend-mode`, not a live filter. Animated `feTurbulence`/`feDisplacementMap` is sluggish on iOS and runs in software on Firefox ([GreenSock forum](https://greensock.com/forums/topic/33075-gsap-and-feturbulence-mobile-performance/); [Mozilla bug 1845890](https://bugzilla.mozilla.org/show_bug.cgi?id=1845890)).
4. **Scroll reveals use `animation-timeline: view()` inside `@supports`.** web.dev reports Firefox 159 made this Baseline across all three engines in October 2026 ([web.dev](https://web.dev/blog/web-platform-10-2026)). An earlier source showed it still behind a flag in June, so feature-detect anyway.
5. **Reduced motion: author the finished state as the default.** The site's global `prefers-reduced-motion` rule sets `animation: none`. If an element's base style is `opacity: 0` and an animation brings it in, that rule leaves it **invisible**. Put the "from hidden" styles inside `@media (prefers-reduced-motion: no-preference)`. W3C technique C39 makes the media query a sufficient technique for motion from interactions ([W3C C39](https://www.w3.org/WAI/WCAG22/Techniques/css/C39.html); [W3C 2.3.3](https://w3.org/WAI/WCAG21/Understanding/animation-from-interactions)).
6. **Blackletter limits.** Pirata One is for the wordmark, H1 and section titles at 40px or more, and never for buttons, labels, inputs, prices or numbers. EEG work found the brain "works harder" with embellished display faces, including a blackletter ([Visible Language](https://journals.uc.edu/index.php/vl/article/view/8855)).
7. **Every hover has a focus and a tap twin.** Hover-only content is unusable on touch ([Envato Tuts+](https://webdesign.tutsplus.com/everything-you-need-to-know-about-the-css-cursor-rule--webdesign-9441a)). About 62% of global web traffic is mobile *(search summary of StatCounter via [Statista](https://www.statista.com/statistics/277125/share-of-website-traffic-coming-from-mobile-devices/))*, and social-referred traffic is likely higher. Desktop-only flourishes go inside `@media (hover: hover) and (pointer: fine)`.
8. **No global custom cursor.** Custom cursors ignore the OS cursor-size setting in some browsers ([Mozilla bug 1609827](https://bugzilla.mozilla.org/show_bug.cgi?id=1609827); [W3C CSS WG](https://lists.w3.org/Archives/Public/public-css-archive/2025Oct/0604.html)). A scalpel or nib cursor may appear over one illustration on fine pointers, never site-wide.
9. **House Bolton distance.** No heraldic framing (banners, shields, "house" language), no red or pink field behind the X board, no human figure with the board. Flaying stays on balls and objects. Human anatomy plates can be margin texture but never sit next to the X board.
10. **The tee is never explained.** No patent-drawing style for it, since a patent sheet exists to disclose how something works ([USPTO archived guide](https://webarchive.library.unt.edu/web/20120928002250/http://www.uspto.gov/main/ccpubguide.htm)). Show only an exterior silhouette or a redaction bar. The market report also holds all public tee content until counsel has cleared the filing question, so tee entries wait even in redacted form.

One positioning caution covers several concepts. Liquid Death already runs a macabre "Country Club" whose sign-up is a soul contract, a bill of sale for your soul, reportedly signed by 145k–200k+ people (figures conflict) ([Liquid Death](https://liquiddeath.com/en-es/pages/soul-contract); [Faster Than Normal](https://fasterthannormal.co/businesses/liquid-death)). Flay's club and ledger jokes must lean on golf (scorecards, dress codes, specimen registers), not on soul-selling.

## Ten sketchable site concepts

Effort ratings map to the 27-item feasibility table in the technical notes. *Easy* means CSS, HTML or a copy-paste snippet. *Medium* means some JS or config plus testing. *Hard* means a backend or custom shaders. Perf cost runs None → Low → Med → High by JS weight and ongoing paint. Every concept keeps the same single teal CTA, the Kit form as a no-JS fallback, and Spectral body text.

### 1. Plate I — the annotated specimen plate

*The home page is one anatomical engraving, and its lettered legend is the site's navigation.*

**Metaphor and source.** The anatomical atlas plate: Vesalius's *Fabrica* (1543), Albinus's *Tabulae sceleti et musculorum* (1747, engraved by Jan Wandelaar) and Gray's (1858), with lettered callouts, plate numbers and a legend below the figure. Wellcome holds a digitized 1747 Albinus marked with the Public Domain Mark ([Wellcome](https://wellcomecollection.org/works/a5y4tvqy); [CPP Digital Library](https://cppdigitallibrary.org/items/show/2420)). The brand doc already describes the logo as "a specimen plate hung on a dark wall," which makes this the native choice.

**Home page, top to bottom.**
1. **Top bar.** "Flay Golf" wordmark at left in Pirata One. At right, a DM Mono ochre status line, "Plate I · First drop on the bench", linking to the latest log entry.
2. **Hero plate.** About 60% of desktop width, on the paper ground with the existing shadow. Four leader lines run from the engraving to letters in the margin: **A** to the dimpled hide, **B** to a nail, **C** to the teal core, **D** to the paper margin. Fell italic caption beneath: "Plate I. The ball, laid open on the flaying board."
3. **Legend**, set directly under or beside the plate in DM Mono, as a real `<ol>` of links: "A. The hide: what Flay is · B. The nails: how we build it (Build Log) · C. The core: get the first drop · D. The margin: tools for the sim bay."
4. **Pitch column.** H1 "Flay Golf", tagline "Built, not bought. No dress code.", lede, form label "Get the first drop", email field, teal button **"Join the waitlist"**, fine print "One email when it's live."
5. **"Further figures" strip.** Three small plate cards: "Plate II. Of the Proper Elevation of the Ball" (tee height chart), "Fig. 3. Latest from the bench" (log), "Plate III. What Can I Swing?" (ceiling calculator).
6. **"Explanation of the Plate."** The three creed items, numbered i–iii.
7. **Footer "Colophon."** Art credits and licences, accessibility statement, © line.

**Navigation.** The legend is the primary nav on the home page. Inner pages carry a compact top nav that keeps the letters (A Story · B Log · C Drop · D Tools), so the system persists. Every inner page is a numbered plate.

**Signature interaction.** Hovering, focusing or tapping a legend letter highlights its leader line and the matching region of the plate (a CSS class on an inline SVG overlay). Hovering a letter on the plate highlights its legend line. Each letter's popover gives one brand truth ("C. The teal core. The only bright thing in here. It's what you're waiting for."), then links on.

**Useful payoff.** The engraving explains the brand and routes visitors without a wall of text. The waitlist sits one tap from the hero. The "further figures" strip pushes tool traffic.

**Mobile.** The plate goes full width. Callout letters on the image become 44px tap targets. The legend renders as a full-width list directly under the image, so mobile users get a plain nav list, and the form follows.

**Accessibility.** Callouts are `<button>`s or links with text labels, never canvas. The legend list is the text alternative. Alt text describes the image instead of repeating the caption. Callout text on paper uses `--teal-deep` or night ink.

**Effort and feasibility.** Easy–Medium. Paper grain (item 1), line draw-on (3), `popover`/`<details>` (14) and `<Picture>` (17) are all Easy. The overlay plus syncing hover and focus is a Medium evening or two. Perf cost: **Low**.

**References.** The UC Libraries "Illustrated Human" Vesalius exhibit ([UC](https://libapps.libraries.uc.edu/sites/s/illustrated-human/page/galleries-and-media)). The Public Domain Review's engraving catalogue, browsable by style and subject ([PDR](https://publicdomainreview.org/shop/fine-art-prints/style/engravings/)). Sacred Bones' fixed template with catalogue numbers ([Fonts In Use](https://fontsinuse.com/uses/15851/sacred-bones-records)).

**Risks.** Anatomical framing is the brand's strongest asset and its biggest Bolton exposure, so callouts point at the ball and never celebrate the X board. Nobody may annotate the tee mechanism. The current AI-generated plate must be replaced by commissioned art.

### 2. The Card — the scorecard as site map

*A 1920s club scorecard, "amended" by the people who were never let in, is the navigation and the club rule book.*

**Metaphor and source.** 1920s clubhouse ephemera: scorecards, local rules and printed dress codes. Notre Dame's archive lists scorecards from the California Golf Club of San Francisco (1918) and St. Augustine Links (1920), but their digitization status is unknown ([ND ArchivesSpace](https://archivesspace.library.nd.edu/repositories/3/archival_objects/2242457)). No public-domain scorecard archive with clear rights was found, so the layout should be **redrawn in brand type**, not scanned.

**Home page, top to bottom.**
1. **Top bar.** Wordmark, plus small DM Mono "Card No. 0001 · Pre-launch."
2. **Hero scorecard.** Full-width table on bone card stock with columns *Hole · Name · Par · Status*. Front nine ("OUT"): 1 The First Drop (waitlist), 2 The Build Log, 3 Tee Height Chart, 4 What Can I Swing?, 5 Viewing Room, 6 The Shop ("Ground under repair"), 7 Rules of the Club, 8 About, 9 Contact. The back nine ("IN") is shaded with a stamp, "Back nine opens with the first drop."
3. **Amended rule.** Printed "Rule 1. Collared shirts and tailored trousers are required on the course" with a strike-through, and a rust rubber stamp over it: "AMENDED: NO DRESS CODE."
4. **Pitch and form**, laid out as the card's *Marker / Attest* block. Visible label "Email". Teal button **"Sign the card"**, with "(joins the waitlist)" in small type beside it.
5. **"Local rules"**: the creed as three numbered rules.
6. **Footer** with "Card printed in the workshop. Scores are not kept."

**Navigation.** The scorecard rows are the site map. A slim scorecard strip of hole numbers (1–9 with short labels) repeats as the top nav on inner pages. The FAQ, shipping and returns page is "Rules of the Club", with numbered rules and amendments struck through.

**Signature interaction.** "Keep your card." Each visited section gets a pencil tick in its row, stored in localStorage, and the total row updates: "You've played 4 of 9. Par is 9." Playing all nine reveals a "hole-in-one" line linking to a hidden lore page.

**Useful payoff.** It is a whole-site map at a glance, and it is honest about what doesn't exist yet: "Ground under repair" beats a dead "Shop" link. The struck-through dress code states the anti-country-club position in one image.

**Mobile.** The table collapses into stacked rows (hole number, name, status) with full-width tap targets. There is no horizontal page scroll. The ticks still work.

**Accessibility.** A real `<table>` with `<th scope>` on desktop. Ticks carry visually hidden text ("visited"). The struck-through rule uses `<del>`/`<ins>` so screen readers announce the amendment.

**Effort and feasibility.** Easy. It is HTML and CSS, plus about 20 lines of JS for ticks. Perf cost: **None–Low**.

**References.** Random Golf Club leads with the club and its stories, with the shop on a subdomain ([RGC](https://randomgolfclub.com/)). Liquid Death's Country Club is the adjacency to avoid ([Liquid Death](https://liquiddeath.com/en-es/pages/soul-contract)).

**Risks.** Club and membership jokes sit close to Liquid Death. Keep the content golf-literal. Eighteen holes with nine closed can look empty, so promote holes as they open. The concept is witty but light on art, so the haunting has to come from drolleries in the margins.

### 3. The Cabinet — drawers of specimens

*The home page is a curiosity cabinet. Each drawer is a section, and nothing sold out ever leaves the collection.*

**Metaphor and source.** The 16th–17th-century Wunderkammer, with drawers, vitrines, handwritten labels and nature mixed with artifice *[vocabulary from background knowledge, unverified]*. Natural-history specimen cards add the component system: accession numbers, collector, locality, date.

**Home page, top to bottom.**
1. **Top bar.**
2. **Short masthead.** H1 "Flay Golf" (smaller than today), Fell italic "A collection, still being collected."
3. **The cabinet.** An umber-wood grid of six drawers, 3×2 on desktop, each with a brass label plate in DM Mono: "I. The First Drop" · "II. The Build Log" · "III. Implements (free tools)" · "IV. The Viewing Room" · "V. Oddities" · "VI. Deaccessioned".
4. **Open drawer.** Opening one slides it out with a short translate and shadow. Below it unfolds the interior: three specimen cards (thumbnail, "Specimen No. FG-0007", one line) and "Open the drawer →".
5. **Waitlist as a catalogue card.** Typed fields: "Collector: [email]". Teal button **"Join the waitlist"**. Fine print: "We'll write when Drawer I is full."
6. **Footer** with "Cabinet catalogued by hand."

**Navigation.** The drawers are the nav. Inner pages carry a breadcrumb in label style: "Cabinet › III. Implements › Tee Height Chart". Drawer interiors use ordinary filters with antique labels (Specimen / Implement / Plate; status), as the Public Domain Review does.

**Signature interaction.** The drawer pull uses native `<details>`, with no JS needed, and sold-out or failed items stay in place stamped **"DEACCESSIONED."** Drawer VI is the "Ossuary" of broken prototypes, with deadpan captions ("Fig. 9. Third print, PLA. Failed at 40 swings.").

**Useful payoff.** Visitors can browse everything in one screen. Scarcity becomes part of the collection instead of a 404, and the failure gallery turns testing into credibility.

**Mobile.** The drawers become a single column of full-width labeled bars, an accordion that opens in place. The touch targets are large and the labels are always visible.

**Accessibility.** `<details>`/`<summary>` gives keyboard and screen-reader support for free. Stamps are decorative (`aria-hidden`), and the status text sits beside them. Drawer labels are text, not images.

**Effort and feasibility.** Easy–Medium (item 14 is Easy). The cost is art: a cabinet front and per-specimen photos with engraving treatment. Perf cost: **Low**, with drawer interiors lazy-loaded.

**References.** The National Museum of Ireland's Wonder Cabinet identity, a graphic system unique to each specimen ([100 Archive](https://www.100archive.com/projects/nmi-wonder-cabinet)). Awwwards' "Cabinet of Wonders Storyteller", visual style not reviewed ([Awwwards](https://www.awwwards.com/sites/cabinet-of-wonders-storyteller)). PDR's faceted antique taxonomy ([PDR](https://www.publicdomainreview.org/collections/all/style/engraving/)).

**Risks.** Six drawers before launch are mostly empty, so start with four. The Ossuary can read as a quality problem: frame it as "the testing that protects the buyer." Tee prototypes stay out of the Ossuary until counsel clears it.

### 4. The Triptych — shutters that open on the club, the garden and the rough

*A grey, respectable club door swings open into a full-color Bosch-style world of three panels.*

**Metaphor and source.** Bosch's *Garden of Earthly Delights*. The closed wings are grisaille (grey monochrome), and opened, the panels run Eden, garden, hell *[structure from background knowledge, unverified]*. Bosch is long out of copyright. The grey-closed, color-open move maps onto the brand's ash base and earth tones.

**Home page, top to bottom.**
1. **Minimal bar.** Wordmark, plus a "Skip to the waitlist" link that is visible on focus and also as a small text link.
2. **Closed shutters**, about 80vh. Two grisaille panels painted as a 1920s clubhouse door. On them: H1 "Flay Golf", Fell italic "Members only.", and in small text beneath, a struck-through "only". Bone-outlined button **"Open the doors"**.
3. **Open triptych.** The panels rotate on CSS hinges (`rotateY`, about 600ms), grisaille turning to color, to reveal three panels. **Left, "The Club"**: About and the creed. **Centre, "The Garden"**: the first drop, with the waitlist form and teal **"Join the waitlist"**. **Right, "The Rough"**: build log and darker limited drops. Each panel has a heading plus two links.
4. **Below the triptych**: the three panels' content as ordinary page sections, so nothing lives only inside the animation.
5. **Footer.**

**Navigation.** Three top-level sections: Club / Garden / Rough. Inner pages show a tiny three-panel glyph with the current panel lit, plus the text labels.

**Signature interaction.** Opening the doors. One click, never a scroll trigger. The open state persists for the session. A later high-effort version adds deep zoom into a commissioned pastiche painting with hidden products, the way the Bosch documentary used Micrio.

**Useful payoff.** Mostly brand impact: a memorable first impression and a natural stage for a limited-drop reveal. The three-section split does give a clean IA.

**Mobile.** The doors become top and bottom halves that slide apart, or the page starts open under reduced motion. The panels stack as three cards with a sticky three-tab switcher.

**Accessibility.** Without JS or under reduced motion, render the page **already open**. The button has `aria-expanded`. Content is never hidden behind the shutters for screen readers. Don't animate filters over the full viewport; crossfade two pre-made images instead.

**Effort and feasibility.** Medium. CSS 3D rotation is Easy–Medium (item 15). The real cost is commissioning a triptych-scale painting. Deep zoom is Hard. Perf cost: **Low–Med**, from large art images: use AVIF at several widths and lazy-load the inner panels.

**References.** NTR, Q42 and Fabrique's interactive *Garden of Earthly Delights*, about 40–50 narrated scenes with deep zoom ([Q42](https://www.q42.nl/en/work/garden-of-earthly-delights); [Colossal](https://www.thisiscolossal.com/2016/02/interactive-bosch/)).

**Risks.** A door before the content is a soft gate, which is why the skip link and the visible waitlist matter. Hell-panel imagery can tip past "beautiful-dark." Art cost is the largest of any concept. This is best kept for a limited-drop microsite (see the recommendation).

### 5. The Course of Flay — a map you play through

*The site is a nine-hole course map that drifts from parkland into Bosch country, and each hole is a section.*

**Metaphor and source.** Vintage hand-inked course plans and yardage books, for example the NYPL's "Rand McNally official motor guide to golf links, New York and vicinity" ([NYPL](https://digitalcollections.nypl.org/items/5b505400-b9ea-0131-fe37-58d385a7b928), rights not checked).

**Home page, top to bottom.**
1. **Top bar** with a "Yardage book" link (the plain list nav).
2. **Hero map**: a top-down ink SVG of nine holes. Holes 1–4 are ordinary parkland, while 5–9 sink into ink lakes, giant birds and burning trees. Each green carries a numbered flag with a visible text label. Overlaid top-left are H1 "Flay Golf" and Fell italic "Nine holes. The back half is haunted."
3. **CTA**: teal **"Tee off at Hole 1"**, which jumps to Hole 1.
4. **Hole cards, in normal scroll order.** Each card has a mini hole diagram, "Hole N · Par N · Yds", a title and a body. Hole 1 "The First Drop" holds the waitlist form, with the button **"Join the waitlist"**. Hole 2 is the Build Log, Hole 3 the Tee Height Chart, Hole 4 "What Can I Swing?", Hole 5 the Viewing Room, and Holes 6–9 cover the shop, the rules, the about page and "The Bog" (lore).
5. **Footer as "The 19th Hole"**: contact and about.

**Navigation.** The map flags plus a text "Yardage book" list, which is a requirement and not a fallback. Inner pages show "Hole 3 of 9" with Previous and Next hole links.

**Signature interaction.** Focusing or hovering a hole on the map highlights it and its card, and a dotted ball flight draws to the green (stroke-dashoffset). Hazards hold easter eggs: tap the bog five times to find the lost ball.

**Useful payoff.** Golfers already know how to read a course map, and it makes the tools feel like part of a round instead of utilities. Prev/next hole links create a guided tour through the content.

**Mobile.** The map shrinks to a thumbnail with a "View course" button. The hole cards become a native scroll-snap yardage book, swiped one hole at a time inside its own component, with a vertical list alternative. The page itself never scrolls sideways.

**Accessibility.** The map is an inline SVG with each flag as a labeled `<a>`. Small targets on a big image are the risk, so the yardage-book list is the primary nav for keyboard and screen-reader users. Easter eggs never trap focus or flash.

**Effort and feasibility.** Medium. The SVG map is a commissioned illustration, and making it interactive is Easy–Medium (item 3). Scroll-snap is CSS. Perf cost: **Med**, because a detailed SVG map is heavy. Simplify paths and keep it under about 150 KB gzipped.

**References.** Ponpon Mania (Codrops), an example of illustration-led web storytelling ([Codrops](https://tympanus.net/codrops/2025/12/29/2025-a-very-special-year-in-review/)).

**Risks.** It is the most "golf" metaphor, but sim golfers play indoors, so the course has to be a joke about that, not a nostalgia piece. Map navigation has small targets. The concept is illustration-heavy.

### 6. The Field Journal — the build log is the home page

*The site opens on the latest dated notebook spread from the workshop, and everything else hangs off the log.*

**Metaphor and source.** Naturalists' field notebooks and lab notebooks: dated entries, taped-in photos, sketches with measurements, crossed-out failures *[vocabulary from background knowledge]*. It doubles as the sim community's native format, the garage build log ([GSF](https://golfsimulatorforum.com/forum/golf-simulator-forum/simulator-builds/335070-my-diy-garage-build-summary)).

**Home page, top to bottom.**
1. **Top bar**: "Flay Golf · Field Journal, Vol. I".
2. **Hero spread**, two ruled pages on paper. The **left page** is the latest entry: date in DM Mono, a Pirata One title ("Entry 12. Cold garage, warm printer"), two lines of text, a taped photo (CSS rotated, masking-tape pseudo-element) and a stamp: HELD / FAILED / CRACKED. The **right page** is "State of the bench", a short checklist with a progress line ("Trial 7 of ~10"), then the waitlist: "Want the entry that says DONE?" and teal **"Join the waitlist"**.
3. **Entry index.** A list of entries with date · entry no. · title · stamp, filterable by tag (Workshop, Sim bay, Tools, Drops).
4. **"Worked out on the back pages."** Margin sketches linking to the tee height chart and the ceiling calculator.
5. **Footer.**

**Navigation.** Chronological first: newest entry, previous and next entry, and the index. A short top nav (Journal · Tools · About · Waitlist) covers the rest.

**Signature interaction.** Stamps "thump" in with a CSS scroll fade. Any tee-related section shows a black **redaction bar** with a DM Mono note, "[withheld pending]", and focus or hover reveals only "Nice try." The bar is a joke, never a hiding place for real content.

**Useful payoff.** Return visits, long-tail search traffic, ready-made newsletter content, and proof of the DIY claim. It follows Tracksmith's "a story for every product" model, which runs on an **Astro front end over Shopify checkout**, the same framework Flay uses ([ecomm.design](https://ecomm.design/site/tracksmith/)).

**Mobile.** The spread stacks: the entry first, then the state of the bench and the form. Taped photos lose their rotation below 400px for readability.

**Accessibility.** Entries are real articles with `<time>`. Stamps have text equivalents. Redaction bars are `aria-hidden`, with a plain "Details withheld" line for screen readers.

**Effort and feasibility.** The build is Easy: a content collection, Markdown with front matter (item 16). The ongoing posting cadence is Medium. Perf cost: **Low**.

**References.** Tracksmith (story per product, Astro over Shopify), Metalwood Studio's "part club repair shop" identity ([hhv journal](https://hhv-journal.com/en/allgemein-de/metalwood-disrupting-the-links/)), and Sim community build logs ([GSF](https://golfsimulatorforum.com/forum/golf-simulator-forum/simulator-builds/335070-my-diy-garage-build-summary)).

**Risks.** A stale journal is worse than none, so generate the status line from the latest post's front matter. Leaking the tee is the largest risk here, and the market report holds all tee entries until the filing question is settled. Early entries cover the caddy, the stickers, cold garages and ceilings.

### 7. The Register — signing is the whole page

*An open club register where the visitor's single act is to add their name, then receive a numbered specimen tag.*

**Metaphor and source.** Clubhouse member registers and visitors' books, crossed with the museum accession register. It is a ritualized sign-up of the kind Liquid Death proved works as "just a form with flavour." Here the flavour is a golf register, not a soul.

**Home page, top to bottom.**
1. **Top bar.**
2. **Open ledger, centered on a dark wall.** Pirata One heading "The Register of Flay Golf", Fell italic "Established 2026. No dress code. No handicap required." Above the signing line, an old printed rule: "Members shall wear collared shirts," overstamped **"VOID."**
3. **The signing line.** It holds the form: visible label "Email", field and teal button **"Sign the register"**, with fine print "One email when the first drop is live."
4. **The register columns** below: *No. · Name · Joined · Habitat* (basement, garage, apartment). Before a backend exists, show only the founder's row ("No. 0001 · The founder · Basement") and empty ruled lines.
5. **Creed** as "Bylaws (abridged)."
6. **Footer.**

**After signing**, on a `/joined` page: a numbered brass-tag card reading "Entered in the register. Specimen No. 0142.", a share button (`navigator.share`), and later "Bring three friends: a numbered sticker sheet ships with your first order."

**Navigation.** Minimal. The register is the page, with a small text nav (Journal · Tools · About) in the top bar. It is the most conversion-focused concept.

**Signature interaction.** On submit, your line "inks in" (SVG stroke-dashoffset on a pen-stroke underline) and the number tag swings in. The number is an honest sign-up sequence number, never a fake ranking. A Robinhood-style position number only motivates when moving up changes something real ([Viral Loops](https://viral-loops.com/blog/how-robinhood-got-1-million-users/), vendor blog).

**Useful payoff.** A clear sign-up path and a shareable success state. A single honest reward tier copies Harry's mechanics at one-person scale: Harry's collected almost **100k emails in the week before launch** with tiered referral rewards ([Tim Ferriss blog](https://tim.blog/2014/07/21/harrys-prelaunchr-email/)).

**Mobile.** The best of the ten: one column, one field, one button, a success card built to be screenshotted.

**Accessibility.** A plain form with a visible label and an `aria-live` note, as now. The ink animation is decorative and skipped under reduced motion. Names in the register appear only with opt-in, first name only.

**Effort and feasibility.** The copy and layout are Easy (item 22). The **real specimen number is Hard for a non-developer** (item 23). The form posts to a Cloudflare Worker, D1 assigns an autoincrement id, and the Worker upserts the subscriber in Kit through API v4 with a `waitlist_no` custom field ([Kit API](https://developers.kit.com/api-reference/subscribers/create-a-subscriber)), plus Turnstile against bots. Kit's own referral system is Pro-only, at about **$79/mo** *(third-party price)* ([Kit pricing](https://kit.com/pricing); [Mailmeteor](https://mailmeteor.com/blog/kit-pricing)). Kit Free covers 10,000 subscribers ([Kit Help](https://help.kit.com/en/articles/16627071-the-kit-free-plan)), and Workers Free allows 100k requests a day ([Cloudflare](https://developers.cloudflare.com/workers/platform/limits)). Perf cost: **None** on the front end.

**References.** Liquid Death's Soul Contract, as the cautionary twin. Robinhood's position-in-line page and Harry's tiered referrals.

**Risks.** Too close to Liquid Death if the copy drifts toward contracts and souls. Displaying sign-ups raises privacy concerns. Fake numbers or fake scarcity are off the table. On its own the concept is thin, so it works best as a component inside another concept.

### 8. The Almanack — the site as a year of drops and seasons

*A monthly almanac page tells visitors what's due this month in the sim bay and when the next drop is foretold.*

**Metaphor and source.** Farmers' and patent-medicine almanacs, with moon phases and month pages, and the medieval Book of Hours with its "labours of the months" *[from background knowledge, unverified]*. Wellcome is the likely source of almanac imagery, with per-item licences checked ([Wellcome guidance](https://docs.wellcomecollection.org/collections-information/lQ0fMyP8xr8hWWJiQkfl/enquiries/images-and-copyright/copyright-images)).

**Home page, top to bottom.**
1. **Top bar**: "Flay's Almanack for 2026–27".
2. **This month's page.** Pirata One "November", Fell italic "The Month of the Cold Garage". A moon-phase strip runs across the top, beside a labours-of-the-month vignette (a 1920s golfer in a flat cap, chipping in a frosty garage).
3. **"What's due"**, three ruled entries. "Due: The cold-garage survival guide" (link). "Foretold: Drop No. 001, The First Implement · Run of ? · Status: on the bench." "Gathering: the Séance Scramble" (shown only once a real date exists).
4. **Waitlist**: "Be told the moment the date is fixed." Teal **"Join the waitlist"**.
5. **The year strip.** Twelve month tiles. Past months link to their archive posts, and future months read "to be foretold."
6. **Footer.**

**Navigation.** By time: the current month, the year strip, and a drop register table in DM Mono (No. · Name · Run · Status). Topic nav sits in the footer.

**Signature interaction.** A countdown appears only when a date is real, as about 20 lines of vanilla JS over a server-rendered date (item 24). After 9pm local time, or in Halloween week, the page gets candlelight vignetting, with layout and contrast left unchanged.

**Useful payoff.** It ties content to the sim season, which runs November to March. The cold-garage guide is a market-report idea for exactly this month. The concept builds a return ritual and makes the drop schedule legible. MSCHF's app is a chronological list of numbered drops plus a timer to the next one ([Bischoff](https://matthewbischoff.com/mschf-app/)). Swag announces dated drops of about 100–500 pieces that never restock ([Golfer's Authority](https://golfersauthority.com/swag-golf-headcovers/); [Swag FAQ](https://swag.golf/pages/faq); sell-out times are self-reported).

**Mobile.** The month page is one column. The year strip becomes a 3×4 grid of small tiles, and the moon strip shrinks to five phases.

**Accessibility.** The countdown text is static for screen readers (no live announcements each second). Moon phases are decorative. The month tiles are a real list of links.

**Effort and feasibility.** Easy to build. Keeping it current is Medium, because an empty month looks dead. Perf cost: **Low**.

**References.** MSCHF's numbered drops with a countdown. Playdate's pre-order at "exactly 10am PT" after a list, when **20,000 units sold in under 20 minutes** ([Game World Observer](https://gameworldobserver.com/2021/07/30/panic-sells-over-20000-playdate-handhelds-for-indie-games-in-less-than-20-minutes)). Sleep Token's seven-day countdown site ([Kerrang](https://www.kerrang.com/sleep-token-have-started-teasing-something-via-a-mysterious-new-website-take-me-back-to-eden-vessel-download-festival)).

**Risks.** It needs monthly upkeep at 5–10 hours a week. "Foretold" language must not promise dates that aren't real. Almanac ornament runs louder than the brand's beautiful-dark baseline.

### 9. Dr. Flay's Consulting Room — the sim bay diagnosed

*The home page asks "What ails your bay?" and answers with free tools, written as an old-fashioned consultation.*

**Metaphor and source.** The 19th-century patent-medicine counter and physician's intake: ornate labels, "remedies," almanac testimonials *[vocabulary from background knowledge]*, plus apothecary bottle labels from Wellcome's medicine-history collections (per-item licence checks needed).

**Home page, top to bottom.**
1. **Top bar**: "Flay Golf · Consulting hours: always."
2. **Hero.** Pirata One H1 "What ails your bay?" Under it, three large apothecary-label buttons in a row, each with an engraved bottle and a plain-type label. "My ceiling is too low" leads to the What Can I Swing? calculator. "My tee won't stay on the mat" leads to an explainer on every fix, ranked, with no Flay tee details. "My launch monitor lies" leads to the black tee vs white tee explainer and the tee height chart.
3. **Intake form**, a "Patient's particulars" card in DM Mono with three questions (ceiling height, mat type, launch monitor type) and the button "Write my prescription". It returns a "Prescription" card with three links and the line "Take as needed."
4. **Optional extras, never a gate**: "Post me the printable chart" with an email field. Kit tags the subscriber by their answers.
5. **Waitlist**: "Dr. Flay's First Remedy is still being tested." Teal **"Join the waitlist"**.
6. **Footer**, with a disclaimer in the voice: "Not a doctor. Measure your own swing."

**Navigation.** Task-first: Ailments (tools) · Remedies (shop, later) · Case notes (build log) · About.

**Signature interaction.** The intake form turns three answers into a prescription card linking to the right tools and posts. The result URL is shareable (`/rx?ceiling=8-2&mat=thick`), and the page can later generate a share image.

**Useful payoff.** The most useful concept of the ten. It meets the top sim-golf search intents head-on. Existing ceiling calculators are thin ([example](https://mikenvogt-dev.github.io/golf-sim-calculator/golf-sim-calculator.html)), and search for the mat-tee-flying problem returns only forum threads and one retailer blog ([GSF](https://golfsimulatorforum.com/forum/golf-simulator-forum/simulator-builds/298033-tee-or-not-to-tee); [Carl's Place](https://www.carlofet.com/blog/best-tees-for-a-golf-simulator)). Interact reports a **40.1% start-to-lead rate** for quizzes. That is the vendor's own data, counted only among people who started ([Interact](https://www.tryinteract.com/blog/quiz-conversion-rate-report/)). The tag segmentation sets up launch emails.

**Mobile.** The three ailment buttons stack full width. The intake form uses large steppers with units (ft/in and cm), and the prescription card fits one screen.

**Accessibility.** Plain HTML forms with `<output>` results, readable without JS through static fallback tables. No blackletter on inputs or numbers. Visible units.

**Effort and feasibility.** Medium overall. The calculator is Easy–Medium (item 19), the chart Easy (item 20), the finder Medium (item 21). Perf cost: **Low**, with tools hydrating `client:visible` or written as vanilla scripts.

**References.** Thin existing sim calculators ([GitHub calc](https://mikenvogt-dev.github.io/golf-sim-calculator/golf-sim-calculator.html); [AgentCalc](https://agentcalc-cloudflare-public.pages.dev/indoor-golf-simulator-build-cost-and-swing-space-planner)). Brandmark's free name generator, reportedly about 40,000 backlinks, a single anecdote ([WooRank](https://www.woorank.com/en/blog/6-linkable-assets-that-will-actually-get-you-high-quality-backlinks)).

**Risks.** Wrong clearance advice could mean a club through the drywall, and the swing-arc formulas still need sourcing or owner testing. Medical parody must stay obviously a joke: no fake testimonials, and no "cures" beyond golf puns. The patent-medicine voice is the loud register, so keep it to this page.

### 10. Catalogue No. 1 — the mail-order listing

*One dense, engraved, numbered page lists everything Flay makes, tests or gives away, like an old mail-order catalogue.*

**Metaphor and source.** Late-19th and early-20th-century mail-order catalogues (Sears, Montgomery Ward): dense columns of engraved product cuts, item numbers, price columns and an order form *[from background knowledge; Internet Archive scans not verified]*. Mail order was the anti-boutique way to buy, which fits "built, not bought."

**Home page, top to bottom.**
1. **Top bar**: "Catalogue No. 1 · Autumn 2026", with a toggle at right, "Plate view / Catalogue view".
2. **Compact masthead.** H1 "Flay Golf", tagline, and one honest ordering note: "Nothing is for sale yet. Everything below is being made, tested or given away."
3. **Inline waitlist**: "Send me the catalogue when orders open." Teal **"Join the waitlist"**.
4. **The listing**: two or three columns on desktop. Each entry has a small engraved cut, an item number (FG-T01), a name, one line and a status. Example rows: "FG-T01 Tee Height Chart · Free · Printable", "FG-T02 What Can I Swing? · Free", "FG-001 The First Implement · On the bench", "FG-002 Tee Reliquary Caddy · Coming", "FG-M01 Sticker Sheet · Coming". The price column reads "—".
5. **"Order form"** at the bottom: interest checkboxes ("Tell me about: ☐ The First Implement ☐ Bay accessories ☐ Prints") that map to Kit tags, an email field and **"Join the waitlist"**.
6. **Footer**: "Our guarantee" (honest returns once the shop exists) and the colophon.

**Navigation.** The item numbers and section heads (Implements · Tools · Prints & Merch) are the nav, with an A–Z jump list. Search engines and screen readers see a well-structured list.

**Signature interaction.** The Plate/Catalogue toggle swaps the same content between an airy plate layout and the dense print layout, with a cross-document view transition between them where supported ([Chrome for Developers](https://developer.chrome.com/docs/web-platform/view-transitions/cross-document)). The choice is remembered.

**Useful payoff.** The most scannable concept. It is shop-ready: the listing becomes Stripe Payment Links later, with a payment cap for numbered runs ([Stripe docs](https://docs.stripe.com/payment-links/customize)). Interest checkboxes segment the list before launch.

**Mobile.** A single column of item rows with the cut at left and text at right. The toggle stays, defaulting to catalogue view on small screens because it is easier to scan.

**Accessibility.** A semantic list or table with real headings. Engraved cuts get short alt text. The toggle is a pair of labeled buttons with `aria-pressed`.

**Effort and feasibility.** Easy to build. Making many small engraved cuts is Medium (vectorized SVG, or a halftone pass on photos). Stripe capped links are Easy (item 25). Perf cost: **Low–Med**, from many images, lazy-loaded below the fold.

**References.** The Public Domain Review shop ([PDR](https://publicdomainreview.org/shop/fine-art-prints/style/engravings/)). Sacred Bones' visible catalogue numbers ([Village Voice](https://www.villagevoice.com/?p=396231)). MSCHF's numbered drops ([Wikipedia](https://en.wikipedia.org/wiki/MSCHF)).

**Risks.** Before launch it is a page of "Coming," and dense layouts can look like generic e-commerce. It needs enough real items (five or more) to justify itself. Avoid the word "house."

### How the ten compare

Scores are the author's judgment from the notes (1 = low, 5 = high). Uniqueness means distinctiveness against golf and DTC sites. Usefulness means jobs done for visitors and signups. Effort and perf use the technical notes' scales. Risk covers brand, legal, accessibility and upkeep.

| # | Concept | Uniqueness | Usefulness | Effort | Perf cost | Risk |
|---|---|---|---|---|---|---|
| 1 | Plate I (specimen plate) | 4 | 4 | Easy–Medium | Low | Medium (Bolton framing, art swap) |
| 2 | The Card (scorecard) | 4 | 4 | Easy | None–Low | Medium (Liquid Death adjacency) |
| 3 | The Cabinet (drawers) | 4 | 3 | Easy–Medium | Low | Low–Medium (empty drawers) |
| 4 | The Triptych | 5 | 2 | Medium (art-heavy) | Low–Med | High (soft gate, art cost, gore) |
| 5 | Course of Flay (map) | 4 | 3 | Medium | Med | Medium (small targets, art) |
| 6 | Field Journal | 3 | 4 | Easy build, ongoing posts | Low | Medium (staleness, tee leak) |
| 7 | The Register | 3 | 4 (signups) | Easy, Hard for real numbers | None | Medium (Liquid Death, privacy) |
| 8 | The Almanack | 4 | 3 | Easy build, Medium upkeep | Low | Medium (upkeep, false dates) |
| 9 | Consulting Room (tools) | 4 | 5 | Medium | Low | Medium (advice liability, voice) |
| 10 | Catalogue No. 1 | 3 | 4 (post-launch) | Easy build, Medium art | Low–Med | Low (thin pre-launch) |

Read the table as a portfolio, not a ranking. No single concept scores high on both uniqueness and usefulness at low effort, but 1, 2, 6, 7 and 9 are complementary. Each organizes a different job (brand, site map, story, signup, tools), so one home metaphor can host the others as sections.

## Build Plate I first, then borrow five pieces

**Recommendation: build Concept 1 as the home page and spine.** It is the existing page carried further, it matches the brand doc's own image, and it costs a low perf budget. Its letter legend also gives the other concepts somewhere to live: **A. The hide** (About, with the Card's struck-through dress code and "Rules of the Club" FAQ from 2), **B. The nails** (the Field Journal build log from 6, with the Cabinet's "Deaccessioned" Ossuary from 3 as one of its pages), **C. The core** (the Register's signing copy and success card from 7, with the Almanack's one-line drop register from 8), and **D. The margin** (the Consulting Room's "What ails your bay?" tool hub from 9). Hold back two concepts. The **Triptych** (4) becomes the microsite for the first dark limited drop, where a reveal is the point and the brand doc wants things darker. **Catalogue No. 1** (10) becomes the shop's alternate view once five or more real items exist. The Course map (5) shows up only as the 404 page ("Plate 404: The Lost Ball," a ball sinking into a bog, with links to tools and the form).

A phased build order for one owner at 5–10 hours a week:

| Phase | Weeks (about hours) | Build | Why now |
|---|---|---|---|
| 0. Guardrails | 1 (6–8 h) | Fix hover contrast, placeholder and button face. Add `fetchpriority`. Self-host and trim fonts. Add the pre-baked grain WebP. Restructure reduced motion so the finished state is the default. Add the 404 plate and voice micro-copy | Protects speed and accessibility before anything is added |
| 1. The plate | 2–3 (12–18 h) | Lettered callouts with legend nav, using placeholder art until the commission lands. "Further figures" strip. Register-style form copy. A "joined" page with a brass-tag card, unnumbered for now | Unique look and signups at near-zero JS |
| 2. Journal | 4–6 (15–25 h) | Content collection for the build log (Field Journal layout, stamps, redaction bars). Status line generated from the newest post. First 3–4 non-tee entries. Accessibility-statement and colophon page | Return visits, newsletter material, DIY proof |
| 3. First tools | 7–12 (30–45 h) | Plate II tee height chart as build-time SVG plus an HTML table, with an optional printable. Then "What Can I Swing?" with owner-tested formulas. Then the three-ailment hub page | The most linkable, people-first assets |
| 4. Launch mechanics | When a date and counsel's answer exist (20–30 h) | Worker + D1 + Kit custom field for real specimen numbers, plus Turnstile. Almanack drop register and an honest countdown. Stripe Payment Link capped to the run size, plus a webhook Worker for edition numbers. Video "Viewing Room" with facade embeds | Each needs a real date or real stock to be honest |
| 5. Optional | Later | Triptych drop microsite. Catalogue view. Prescription finder with Kit tags. Share cards. Scorecard nav once there are more than six sections. Opt-in sound | Only once traffic justifies the work |

The whole stack stays at about **$0/month** at launch scale. Cloudflare Pages allows 500 builds a month and describes static bandwidth as unlimited ([Cloudflare Pages limits](https://developers.cloudflare.com/pages/platform/limits)). Kit is free to 10,000 subscribers, and Stripe charges per transaction. Upgrades come only with Kit Pro referrals (about $79/mo, third-party figure) or traffic past Workers' free 100k requests a day. One real unknown for Phase 1: whether the direct-to-Kit form can redirect to Flay's own `/joined` page. Kit form settings likely allow a redirect URL *[unverified]*. If not, the custom success page waits for the Phase 4 Worker.

## Playful patterns worth borrowing, and the ones to skip

The evidence puts delight **after** the conversion moment (success states, the 404) or in places people choose to explore (hover and focus on plates, easter eggs), never in front of the form. No controlled study measures easter eggs' effect on conversion, so they are justified by lore and shareability, not by data.

Worth borrowing, roughly by value per hour. **Plate numbering and Fell captions** on every image ("Fig. 2. Prototype, third print, PLA, failed at 40 swings."), with alt text that still describes the picture. **Voice micro-copy** that gives the plain instruction first and the joke second ("That email seems to have been buried. Try again."). **Rubber stamps** (VOID, NO DRESS CODE, DEACCESSIONED, TRIAL FAILED): rotated, multiply-blended and noise-masked. **A teal wax-seal CTA** as the one call to action, pressing to 0.96 scale. **Scroll-faded creed items** via CSS view timelines. **A secret handshake.** On desktop that is ↑↑↓↓←→←→BA, and on mobile, tapping the teal core seven times. It turns the page "after hours" and reveals a capped launch code. **Seasonal haunting** after 9pm or in Halloween week, as CSS only, with contrast and layout unchanged. **A labeled "Members' entrance" footer link** to a lore and manifesto page. **Drollery margin creatures** on long reads that open footnotes. Drolleries are marginal images popular from about 1250 into the 15th century, and they include knights fighting snails ([Stephens College](https://blog.stephens.edu/arh101glossary/?p=2261)). **A "draw a card" randomizer** using a Flay tarot (The Duffer, The Mulligan, The Water Hazard) that serves a tip or tool. Rider-Waite-Smith (1909) is public domain in the US, but US Games claims the trademark and its 1971 recolorings are separate ([Wikipedia](https://en.wikipedia.org/wiki/Rider%E2%80%93Waite_Tarot)). **A Holbein-style danse macabre frieze** in the footer, with Death tapping the banker, the member and the caddie. **Opt-in sound** last: a speaker toggle that starts off, creates the AudioContext only on click, and remembers the choice ([Chrome autoplay policy](https://developer.chrome.com/blog/autoplay)).

Skip these: a global custom cursor, a scrolljacked "dissection" hero, any preloader or intro animation, autoplay audio, icon-only nav, puzzle-gated sign-up, gating calculator answers behind email, fake waitlist numbers or fake scarcity, blackletter on buttons, inputs or prices, and anything that flashes more than three times a second. Sleep Token's puzzle sites work because thousands of fans are waiting to solve them ([Loudwire](https://loudwire.com/sleep-token-fan-reaction-mysterious-new-puzzle-site-2025/)). A brand with no audience gets one hidden lore page at most.

## Useful tools carry the site's search and link value

Tools are where "fun" pays rent. A working calculator answers sim-golf intent better than an article, and **gating the answer kills link-worthiness**, because people won't link to a wall. Google's people-first guidance points the same way ([Google Search Central](https://developers.google.com/search/blog/2022/08/helpful-content-update)). The candidates come from the market report and the UX notes, ordered by build sequence.

The **tee height chart** ("Plate II: Of the Proper Elevation of the Ball") comes first because it is small and carries no IP exposure. Tee height affects launch-monitor readings ([GSF](https://golfsimulatorforum.com/forum/skytrak/119963-tees-effecting-distance)). It should render as build-time SVG from a data file, with the real numbers in an HTML table and an optional printable PDF. **"What Can I Swing?"** comes next as the flagship. It takes ceiling height, player height and club, and returns an OK / choke down / no verdict with an engraved figure whose teal arc scales with the inputs. It is about 50 lines of vanilla JS with `<output>` elements. Common guidance puts the ceiling minimum at 9–10 ft ([Rapsodo](https://rapsodo.com/blogs/golf/recommended-ceiling-height-for-golf-simulator)), and forum builders report going lower, so the tool must state its assumptions and safety margin. Its formulas are **not yet sourced**. Two pillar explainers follow. **"Why your mat tee keeps flying: every fix, ranked"** covers generic fixes only, never Flay's mechanism. **"Black tee, white tee"** explains why white tees confuse camera launch monitors ([Carl's Place](https://www.carlofet.com/blog/best-tees-for-a-golf-simulator)). A **mat-tee finder** asks three questions and routes to a recommendation and a log post. It may honestly recommend competitors before Flay has stock, and it tags Kit subscribers. Later builds, each Large: the **budget planner / launch-monitor-for-my-room chooser**, which feeds a disclosed affiliate page; the **bay builder**, a room-plan configurator; and **share cards** for tool results, which start as a prefilled share URL. Seasonal guides (cold garage, apartment or renter sims) slot into the Almanack months. No published data on sim-golf calculator traffic or backlinks was found, so the expected payoff is reasoned, not measured.

## Public-domain art is plentiful, but no archive is all-clear

For a commercial brand, the safe pattern is **CC0 or Public Domain Mark items only**. Keep a simple asset register (URL, licence, date retrieved) and credit CC BY items in the footer colophon, which also fits the plate metaphor. Use public-domain scans as texture, borders and marginalia, and commission the hero art so Flay owns it. Purely AI-generated images generally can't be copyrighted, per the brand doc.

| Source | What's there | Licensing caution |
|---|---|---|
| Wellcome Collection | Anatomical atlases (Albinus 1747, PDM), apothecary and almanac material | A mix of CC0/PDM, CC BY 4.0 and **CC BY-NC**. The 1923–36 RAMC Golfing Society reports are CC BY-NC, so **not commercial** ([Wellcome guidance](https://docs.wellcomecollection.org/collections-information/lQ0fMyP8xr8hWWJiQkfl/enquiries/images-and-copyright/copyright-images); [Wellcome search](https://wellcomecollection.org/works/ejtcpq2r/items?page=2)). Commons found metadata mismatches |
| Biodiversity Heritage Library (Flickr) | More than 250,000 natural-history images, including botanicals for "the rough" | "Most" are PD. Some are CC by permission, so check each rights field. Uploads reportedly paused around 2022 ([Illinois guide](https://guides.library.illinois.edu/BHL/flickr); [BHL blog](https://blog.biodiversitylibrary.org/2021/02/250000-free-nature-images-flickr)) |
| Rijksmuseum | 700k+ works under CC0, strong on Dutch vanitas | Sources conflict on whether the museum asks people to avoid commercial use, so check the current terms ([ianvisits](https://www.ianvisits.co.uk/blog/2021/01/21/over-700000-paintings-from-the-rijksmuseum-online-copyright-free/); [Lines and Colors](https://linesandcolors.com/?p=6622)) |
| Cleveland Museum of Art | Open-access items, including manuscript leaves with drolleries | Marked PD: "copy, modify, and distribute" ([CMA](https://www.clevelandart.org/art/2011.55)) |
| Wikimedia Commons | Drolleries category, high-res Bosch scans | Licence varies per file. The US treatment of photos of 2D public-domain works is *[unverified]* ([Commons](https://commons.wikipedia.com/wiki/Category:Drolleries)) |
| Old Book Illustrations | Engraved book plates | Treats US works before 1928 and EU works before 1890 as PD, but "accept[s] no responsibility" and gives no guarantee. Its own texts are CC BY-NC-SA ([OBI terms](https://www.oldbookillustrations.com/terms-of-use/)) |
| NYPL, Library of Congress, Internet Archive | Golf motor guides, early golf books | Rights **not checked** item by item ([NYPL](https://digitalcollections.nypl.org/items/5b505400-b9ea-0131-fe37-58d385a7b928)) |
| US patents | Old golf patents as texture | Generally not under copyright, with notice-based exceptions ([USPTO archived](https://webarchive.library.unt.edu/web/20120928002250/http://www.uspto.gov/main/ccpubguide.htm)). Never use the patent format for the tee |

Two brand-specific cautions apply. **Beksiński and the album covers in the brand doc are protected**, so use them for mood only. **No public-domain tattoo flash source was found**, so a flash-sheet merch index means commissioned work.

## Conclusion

The research changes the question. It is less "which wild effect makes Flay memorable" and more "which old **indexing system** can the site borrow." Plates have legends, scorecards have holes, cabinets have drawers, journals have dates and registers have numbers. Each of these turns a navigation chore into brand voice for close to zero runtime cost. The award-winning dark sites spend on WebGL, while the dark brands people remember spend on a system: Sacred Bones' catalogue numbers, MSCHF's numbered drops. That route is open to a one-person, near-zero-budget brand, and Flay's existing "Plate I" caption already takes the first step.

The constraints work in Flay's favor. The ban on describing the tee produces the redaction bar, one of the brand's best jokes. The Bolton caution keeps the flaying clinical, which reads as more museum-grade. The 5–10 hour week forces tools-before-spectacle, the order that compounds through search and sharing. The one thing the research could not settle is whether golfers respond to "haunted" at all. Test that with the waitlist itself, for example with two versions of the success copy, before spending on triptych-scale art.
