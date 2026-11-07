# Visual metaphors for a "country club, haunted" Flay Golf site

Scope: historical and thematic visual systems that could act as the *structural* metaphor for the Flay Golf website (navigation, page model, interactions), not only as surface decoration. Brand inputs are from `docs/brand/brand.md`: country club, haunted; a 1920s golfer walking into Bosch; bone/ash/ochre/rust/sepia/umber plus a teal accent; Pirata One, Spectral, IM Fell English, DM Mono; the logo is a flayed ball on an X board drawn as an anatomical engraving; stay clear of House Bolton.

Notation: **[unverified]** marks claims that come from background knowledge and were not checked against a source in this session. The "Flay use" lines are design proposals, not findings. The research ran on a budget of about 16 tool calls, so many web-example claims are leads to check, not confirmed facts.

## Q1. Which metaphors fit, and how could each one structure the site?

### Takeaway
The strongest candidates are the ones that already come with a built-in *indexing system* that can become navigation. The anatomical specimen plate has lettered callouts and a legend; the 1920s clubhouse has a scorecard with 18 holes and a member ledger; the cabinet of curiosities has drawers; the Bosch triptych has panels that open; the field notebook has dated entries. The best direction is probably a hybrid: an **anatomical plate + clubhouse ephemera** spine (the logo already sits "like a specimen plate hung on a dark wall", per the brand doc), with the triptych as a one-off hero or drop mechanic, and drolleries/bestiary as the decorative margin layer.

### Cited Findings

**1. Anatomical atlas / specimen plates (Vesalius 1543, Albinus 1747, Gray's 1858)**
- Source and vocabulary: copperplate or woodcut figures, lettered callouts (A, B, C... and fig. 1, fig. 2), plate numbers, Latin legends, hatching and stipple shading, figures posed in landscapes (Albinus's backgrounds famously include a rhinoceros) **[unverified, background knowledge]**.
- Assets: Wellcome holds a digitized 1747 Leiden edition of Albinus's *Tabulae sceleti et musculorum corporis humani* marked with the Public Domain Mark — [Wellcome works/a5y4tvqy](https://wellcomecollection.org/works/a5y4tvqy). The College of Physicians of Philadelphia digitized the atlas and credits Jan Wandelaar as engraver — [CPP Digital Library](https://cppdigitallibrary.org/items/show/2420). NLM's "Historical Anatomies on the Web" has an Albinus section — [NLM](https://sites.wip.nlm.nih.gov/exhibition/historicalanatomies/albinus_home.html).
- Web examples: the University of Cincinnati "Illustrated Human" online Vesalius exhibit — [UC Libraries](https://libapps.libraries.uc.edu/sites/s/illustrated-human/page/galleries-and-media). Armadillo Systems made a "Turning the Pages" page-turning edition of the *Fabrica* (also a British Library gallery) — [Armadillo Systems](https://armadillosystems.com/index.php/category/work/page/2/). The McGovern library's touch-screen "e-Vesalius" facsimile — [TMC Library](https://library.tmc.edu/mcgovern/?p=2027). These are scholarly facsimiles, not brand sites, so they show the source format more than a brand use of it.
- **Flay use:** every product page is a "Plate" (Plate IV: The Pageboy Cap). The product image carries lettered leader lines (A: crown, B: brim, C: stitching), and the A/B/C legend in DM Mono under the plate *is* the spec list and anchor nav: hover a letter to highlight the line, click to jump to that detail. The shop index is the "Tabulae", a numbered list of plates. Keep the flaying motif on the ball and on products only, never on a human figure (the Bolton caution).

**2. Cabinet of curiosities / Wunderkammer**
- Vocabulary: drawers, compartments, glass vitrines, specimens crowded together, handwritten labels, a mix of nature and artifice **[unverified]**.
- Web/design example: the National Museum of Ireland "Wonder Cabinet" identity was a custom logotype plus "a graphic system of geometric teeth unique to each specimen" (Detail Design Studio; AP+E Architects for the cabinet) — [100 Archive](https://www.100archive.com/projects/nmi-wonder-cabinet); [NMI](https://museum.ie/ga-IE/News/2021-(2)/Wonder-Cabinet-at-the-National-Museum-of-Ireland). Awwwards lists a "Cabinet of Wonders Storyteller" project by Lukas Pilka (an AI-curated museum explorer) — [Awwwards](https://www.awwwards.com/sites/cabinet-of-wonders-storyteller). Its visual treatment was not reviewed.
- **Flay use:** the home page is a grid of drawers. Each drawer is a category (Headwear / Balls / Tees / Oddities / Build Log), slides out on hover with a short CSS translate and a shadow, and opens a drawer-interior page. Sold-out items stay in the cabinet with a "DEACCESSIONED" label instead of disappearing. That suits a small-batch DIY brand, because scarcity becomes part of the collection.

**3. Natural-history museum catalogue cards and specimen labels**
- Vocabulary: typed index cards, accession numbers, collector, locality, date, taxonomic name, pin-labels, rubber stamps, card-catalogue drawers **[unverified]**.
- Assets: the BHL Flickr stream holds a large body of mostly public-domain natural-history illustration, searchable by scientific name — [Illinois BHL guide](https://guides.library.illinois.edu/BHL/flickr); BHL reported more than 250,000 images — [BHL blog](https://blog.biodiversitylibrary.org/2021/02/250000-free-nature-images-flickr). Wellcome holds 1920s-30s index cards (CC BY 4.0) — [search result, Wellcome](https://wellcomecollection.org/works/j5cjzeav) **[record not opened; card content unverified]**.
- **Flay use:** this is the micro-component system and pairs with metaphor 1. Each product gets a catalogue card ("Specimen No. FG-0007 · Collected: workshop, Nov 2026 · Habitat: the rough"). DM Mono suits the typed fields well. Cards can be filtered like a card catalogue (a tab-divider row: A–F, G–L...) or sorted by "accession date" = newest drops.

**4. Apothecary / patent-medicine labels and almanacs**
- Vocabulary: ornate borders, multiple competing typefaces, outrageous claims ("Cures slice and hook"), testimonials, bottle silhouettes, almanac calendars with moon phases, "zodiac man" figures **[unverified]**.
- Assets: Wellcome is the obvious source (medicine history). Its images are mostly CC BY 4.0 or public domain (CC0/PDM), but some are CC BY-NC 4.0, so check each item's licence — [Wellcome copyright guidance](https://docs.wellcomecollection.org/collections-information/lQ0fMyP8xr8hWWJiQkfl/enquiries/images-and-copyright/copyright-images).
- **Flay use:** this works best as the *voice* register for the "loud" merch line and for limited drops ("Dr. Flay's Restorative Tonic for the Weekend Duffer"). An almanac page can be the drop calendar: upcoming releases sit on a lunar calendar, which gives the waitlist a ritual. Avoid making it the main navigation, because it runs louder than the brand's "beautiful-dark" baseline.

**5. 1920s golf clubhouse ephemera (scorecards, member ledgers, locker tags, yardage books, rule books, caddie badges)**
- Assets found: NYPL Digital Collections has a "Golf Illustrated / Outdoor America" collection — [NYPL](https://rp-digitalcollections.nypl.org/collections/golf-illustrated-outdoor-america) **[rights not checked]**. Notre Dame's archive lists golf scorecards from California Golf Club of San Francisco (1918), St. Augustine Links (1920) and Brockport Golf Club — [ND ArchivesSpace](https://archivesspace.library.nd.edu/repositories/3/archival_objects/2242457) **[digitization status unknown]**. Internet Archive golf books such as *Golf for the Beginner* (digitized from LoC) and *The Gorham Golf Book* (marked PD on Commons) — [Commons mirror](https://download.osmand.net/wiki/File:The_Gorham_golf_book_(IA_gorhamgolfbook00gorh).pdf). Wellcome's RAMC Golfing Society reports (1923–36) are CC BY-NC 4.0, so they are **not** usable commercially — [Wellcome search results](https://wellcomecollection.org/works/ejtcpq2r/items?page=2).
- I found no reliable public-domain scorecard archive, so it is likely more practical to *redraw* scorecard and ledger layouts in the brand type than to reuse scans (inference).
- **Flay use (strongest structural candidate):** this is the clearest place for the anti-country-club joke, because the site becomes a club that has been "taken over". (a) The **scorecard is the main nav**: holes 1–9 "Out" (Shop, Plates, Build Log...), 10–18 "In" (About, Stockists, Contact), with par/yardage columns doubling as item counts. (b) The **member ledger** is the waitlist/newsletter: you "sign the ledger" in a handwritten-style field, and existing signatures (first names only) fill the page, with a "No dress code" stamp over the old dress-code rule. (c) **Locker tags** become order-confirmation or account cards, with a numbered brass tag. (d) The **rule book** is the FAQ/shipping/returns page, written as "Rules of the Club" with numbered rules and amendments struck through.

**6. Vintage golf course maps and hole diagrams**
- Vocabulary: hand-inked hole plans, hazards drawn as stipple, tees and greens as circles, yardage numbers, compass roses, Rand McNally-style motor guides to golf links **[partly unverified]**. NYPL holds a "Rand McNally official motor guide to golf links, New York and vicinity" — [NYPL item](https://digitalcollections.nypl.org/items/5b505400-b9ea-0131-fe37-58d385a7b928) **[rights not checked]**. I did not find a dedicated LoC golf-map collection; LoC's map division holds more than 4.5 million items, of which only a fraction are online — [Smithsonian](https://www.smithsonianmag.com/smart-news/library-congress-putting-its-map-collection-on-the-map-180961380/).
- **Flay use:** the site map is literally a course map. It is an 18-hole "Course of Flay" that turns more Bosch the further you play: the front nine is a normal parkland course, and the back nine drifts into hell-panel terrain (lakes of ink, giant birds, burning trees). Each hole is a section, and the hazards are Easter eggs. This also works as a yardage-book-style mobile nav: swipe hole by hole, each hole a card with a diagram and a "yardage" of products.

**7. Medieval bestiaries, illuminated manuscripts and marginalia (drolleries)**
- Drolleries are small marginal images in illuminated manuscripts, most popular from about 1250 into the 15th century, and include recurring scenes like knights fighting snails — [Stephens College glossary](https://blog.stephens.edu/arh101glossary/?p=2261); [Daily Art Magazine](https://www.dailyartmagazine.com/killer-rabbits-in-medieval-manuscripts/).
- Assets: Wikimedia Commons has a "Drolleries" category — [Commons](https://commons.wikipedia.com/wiki/Category:Drolleries). Cleveland Museum of Art open-access psalter leaf marked public domain ("copy, modify, and distribute") — [CMA](https://www.clevelandart.org/art/2011.55).
- **Flay use:** this is the *margin layer*, not the main structure. Long-form pages (Build Log, About) get a wide margin where small drollery creatures (a heron caddie, a snail in a flat cap) react to the paragraph next to them. They could be footnote markers: click the creature to open a side-note. Illuminated drop-cap initials in Pirata One with teal-filled counters could open each section. Blackletter and illumination are natural partners.

**8. Bosch's triptych format (closed outer panels, then open)**
- Web example: NTR, with Q42 and Fabrique, built the interactive documentary *Hieronymus Bosch: The Garden of Earthly Delights*. It breaks the painting into roughly 40–50 narrated scenes and uses Q42's ultra-high-res zoom tool Micrio (mouse-wheel zoom inside the painting); it is part of a transmedia triptych with a film and a VR piece — [Q42](https://www.q42.nl/en/work/garden-of-earthly-delights); [NTR site](https://tuinderlusten-jheronimusbosch.ntr.nl/EN); [Colossal](https://www.thisiscolossal.com/2016/02/interactive-bosch/).
- Vocabulary: the closed wings show a grisaille (grey monochrome) globe; opened, the panels run Eden, Garden, Hell left to right **[unverified, background knowledge]**. Grisaille closed → color open maps neatly onto the brand's ash-grey base → earth tones.
- Assets: Bosch is long out of copyright (brand doc). High-res scans are on Wikimedia Commons **[specific file licences not checked; museum photographs of 2D PD works are generally treated as PD in the US: unverified]**.
- **Flay use:** the hero or drop-reveal mechanic. The landing page shows two grisaille shutter panels (the "closed" club, grey and respectable), and scrolling or clicking swings them open in 3D CSS (`rotateY` on the hinges) to reveal a three-panel scene: left = "the Club" (heritage, About), center = "the Garden" (shop), right = "the Rough"/hell (limited dark drops). The panels become the three top-level sections. A Micrio-style deep-zoom hero, where hidden products sit inside a commissioned Bosch-pastiche painting, is the high-effort version (inference).

**9. Tarot / memento mori / vanitas**
- Rider-Waite-Smith (1909) is public domain in the US (copyright not renewed) and, per the Pamela Colman Smith life+70 reading, in the UK/EU since 1 Jan 2022. US Games claims the RIDER-WAITE trademark, and its 1971 recolorings are separate — [Wikipedia](https://en.wikipedia.org/wiki/Rider%E2%80%93Waite_Tarot); [Sacred-texts FAQ](https://sacred-texts.com/tarot/faq.htm). Sources disagree on the UK/EU date (older ones say 2012).
- Vanitas still-life vocabulary: skulls, guttering candles, hourglasses, wilting flowers, overturned glasses **[unverified]**. The Rijksmuseum (strong in Dutch vanitas) releases images under CC0 with a free account; reports differ on whether the museum discourages commercial use — [ianvisits](https://www.ianvisits.co.uk/blog/2021/01/21/over-700000-paintings-from-the-rijksmuseum-online-copyright-free/); [Lines and Colors](https://linesandcolors.com/?p=6622).
- **Flay use:** a "draw a card" product finder or loading state. A Flay deck of 22 trumps (The Duffer, The Caddie, The Water Hazard, The Mulligan...) as a randomizer that serves a product or tip. Or the drop page as a three-card spread (past / present / next drop). Vanitas still-lifes work as product photography direction (a ball beside a skull and an hourglass) rather than as navigation.

**10. Carnival sideshow / freak-show banners**
- Vocabulary: painted canvas banners in rows, exaggerated claims, "ALIVE!", ticket stubs, barker copy **[unverified]**. Sources: Library of Congress / Circus World / Ringling collections **[not checked]**.
- **Flay use:** limited-drop "midway". Each drop is a banner in a horizontal-scroll row, with a ticket-stub add-to-cart. It is risky as a primary system, because it pushes toward "loud" and kitsch, and sideshow imagery carries exploitation baggage around disabled people. Use carefully if at all (inference).

**11. Victorian seed / mail-order catalogues (Sears, Montgomery Ward)**
- Vocabulary: dense multi-column engraved product cuts, item numbers, price columns, order forms, "money back" guarantees **[unverified]**. Old Sears catalogues are on Internet Archive **[not verified this session]**.
- **Flay use:** a "Catalogue No. 1" view that is a dense, print-like, engraved listing of every product with item numbers and an order form, as a toggle against the airy plate view. This suits the DIY "we make our own gear" story, because mail order was the anti-boutique way to buy (inference).

**12. Field notebooks / lab notebooks**
- Vocabulary: graph or ruled paper, dated entries, taped-in samples, sketches with measurements, crossed-out failures, specimen numbers **[unverified]**.
- **Flay use:** the **Build Log** (blog). Each post is a dated notebook spread, with taped-in photos (CSS rotated, a masking-tape pseudo-element), margin sketches, and "Trial 14: failed" stamps. Entries cross-link to the plate of the product they led to, so the notebook becomes the provenance trail behind every product. It must not show or describe the tee mechanism: log entries about the tee stay at the level of "Trial 9, cracked" with the drawing obscured or redacted (an ink-blot), which itself fits the mood.

**13. Patent drawings**
- US patent text and drawings are "typically not subject to copyright", with exceptions where applicants include specific copyright authorization language (37 CFR 1.71(d)-(e), 1.84(s)) — [USPTO archived guide](https://webarchive.library.unt.edu/web/20120928002250/http://www.uspto.gov/main/ccpubguide.htm); [Penn State FAQ](https://psu.libanswers.com/faq/325294). A Commons discussion argues the blanket PD label is overstated — [Commons talk](https://c.enwp.org/wiki/Commons_talk:Licensing/U.S._patents).
- Vocabulary: Fig. 1/Fig. 2, numbered reference leaders, exploded views, ruled shading, the inventor's signature block **[unverified]**.
- **Flay use:** **do not use this for the tee.** A patent sheet is by definition an explanation of how something works. Use it for non-secret items (a cap's panel construction, a towel clip, a divot tool) or as *parody* (an 1890s patent sheet for "Improved Apparatus for the Flaying of Golf Balls" showing the logo board). Old golf patents (ball winding machines, early tees) as background texture are possible after checking each one's notice. Overlaps heavily with metaphor 1, whose callouts are safer.

**14. Tattoo flash sheets**
- Vocabulary: a sheet of small bold-outlined designs in a grid, limited color, hand lettering, numbered or priced flash **[unverified]**. The brand doc already points the logo commission at fine-line/blackwork tattoo artists.
- Assets: most famous flash (e.g. Sailor Jerry) is commercially licensed. **No reliable PD flash source was found**: treat it as a commission-only style.
- **Flay use:** the stickers/patches/merch index page as a flash sheet. Each design is a tile you click to buy ("pick off the wall"). It is good for the "loud" merch line, and commissioned flash doubles as merch art.

**Additional candidates (added)**
- **15. Book of Hours / calendar pages** (the labors of the months): a 12-month drop calendar. Overlaps with the almanac idea **[unverified]**.
- **16. Danse macabre (Holbein woodcuts)**: Death tapping each "type" (the banker, the member, the caddie) is a direct anti-country-club satire device. It could be the About page or a scrolling frieze footer **[Holbein's 1538 *Dance of Death* is PD by age; specific scans unverified]**.
- **17. Herbals / "the rough" botanicals**: BHL botanical plates as section dividers for "the rough" — [Hyperallergic on BHL botanicals](https://hyperallergic.com/over-150000-botanical-illustrations-enter-the-public-domain).

### Inferences
- The brand doc's own line, the logo "like a specimen plate hung on a dark wall", already chooses metaphor 1 as the visual anchor. The others should be judged by how well they *index*: scorecard (18 slots), plate legend (A–Z), drawers (grid), triptych (3), notebook (chronological).
- A coherent stack: **Clubhouse scorecard = global nav; Anatomical plate + specimen card = product page; Field notebook = build log; Rule book = FAQ; Member ledger = waitlist; Triptych = hero/drop reveal; Drolleries = margin ornament.** Tarot, sideshow, apothecary and flash are best kept to limited-drop micro-sites, which matches the brand's "limited drops go darker / loud is mostly on merch".
- Contrast is the joke. A *respectable* 1920s club artifact (scorecard, ledger) gets *infested* with Bosch/drollery creatures. That expresses "country club, haunted" structurally, which pure-horror metaphors (sideshow, flayed anatomy everywhere) do not.

### Gaps
- No live brand/e-commerce websites using scorecard-as-nav, triptych-opening heroes, or specimen-plate product pages were verified in this session. The sibling `reference_sites.md` may cover this.
- No public-domain 1920s scorecard or locker-tag scans with clear rights were found. Library of Congress and Internet Archive need direct searching.
- Sears catalogue, sideshow banner and Holbein sources were not verified this session.

## Q2. Where to get public-domain assets, and under which licences?

### Takeaway
Wellcome, BHL, Rijksmuseum, Cleveland Museum of Art and Commons are the core sources, but none of them is "all PD". Check the per-item licence: Wellcome mixes CC0/PDM, CC BY and CC BY-NC; Old Book Illustrations gives no legal guarantee; patent drawings have exceptions.

### Cited Findings
- **Wellcome Collection:** licences are mainly CC BY 4.0 and public domain (CC0/PDM). Some digitized works are CC BY-NC 4.0 and some rights-managed images are low-res CC BY-NC-ND. Attribution "Wellcome Collection" is expected for CC BY — [Wellcome guidance](https://docs.wellcomecollection.org/collections-information/lQ0fMyP8xr8hWWJiQkfl/enquiries/images-and-copyright/copyright-images); 2014 release of about 100k images under CC BY — [Public Domain Review](https://publicdomainreview.org/collection/selection-from-wellcome-library-s-release-of-100k-openly-licensed-images/). Commons volunteers found metadata mismatches (some files tagged cc-by-nc) — [Commons batch page](https://commons.wikimedia.org/wiki/Commons:Batch_uploading/Wellcome_Images).
- **Biodiversity Heritage Library (Flickr):** "most" images are PD and free to reuse. Attribution is not legally required but is encouraged. Some items are under CC by permission, so check the rights field. Uploads reportedly paused around 2022 — [Illinois guide](https://guides.library.illinois.edu/BHL/flickr); [BHL blog](https://blog.biodiversitylibrary.org/2021/02/250000-free-nature-images-flickr).
- **Rijksmuseum:** CC0 1.0, high-res, free account, 700k+ works (2021). Sources conflict on whether the museum asks people to avoid commercial use, so check the current terms — [ianvisits](https://www.ianvisits.co.uk/blog/2021/01/21/over-700000-paintings-from-the-rijksmuseum-online-copyright-free/); [Lines and Colors](https://linesandcolors.com/?p=6622); [OpenGLAM 2013](https://openglam.org/2013/02/27/case-study-rijksmuseum-releases-111-000-high-quality-images-to-the-public-domain/).
- **Old Book Illustrations:** the site does not limit use of illustrations but "accept[s] no responsibility" and gives no non-infringement guarantee. It treats US works published before 1928 and EU works before 1890 as PD. Its own texts are CC BY-NC-SA 4.0 — [OBI Terms](https://www.oldbookillustrations.com/terms-of-use/).
- **Cleveland Museum of Art:** open-access items marked public domain, "copy, modify, and distribute" without permission — [CMA psalter leaf](https://www.clevelandart.org/art/2011.55).
- **US patents:** generally not under copyright, with notice-based exceptions — [USPTO archived](https://webarchive.library.unt.edu/web/20120928002250/http://www.uspto.gov/main/ccpubguide.htm).
- **Rider-Waite-Smith tarot (1909):** PD in the US. UK/EU status most likely from 2022. Trademark claimed by US Games — [Wikipedia](https://en.wikipedia.org/wiki/Rider%E2%80%93Waite_Tarot).
- **NYPL Digital Collections, Library of Congress, Internet Archive:** relevant golf items exist (NYPL golf motor guide; LoC-digitized golf books on IA), but rights were not checked item by item — [NYPL](https://digitalcollections.nypl.org/items/5b505400-b9ea-0131-fe37-58d385a7b928).

### Inferences
- For a commercial brand, prefer CC0/PDM items, keep a simple asset register (URL, licence, date retrieved), and credit CC BY items in the footer "Colophon" (which itself fits the plate/catalogue metaphor).
- Using PD scans *as texture* (paper grain, borders, drolleries) is low risk. Hero imagery should be commissioned so Flay owns it, which also matches the brand doc's stance that the logo must be human-drawn.

### Gaps
- Current Rijksmuseum commercial-use terms; current Wellcome per-item licences for the specific Albinus/Vesalius plates; Library of Congress golf ephemera rights; public-domain tattoo flash (none found).

## Q3. Which web techniques make these metaphors feel tactile?

### Takeaway
SVG `feTurbulence` noise (optionally lit with `feDiffuseLighting`) is the standard, lightweight way to get paper grain and rough edges. Engraving and halftone looks can be done with GLSL shaders, and Bayer dithering is a well-documented, cheaper cousin.

### Cited Findings
- `feTurbulence` generates Perlin noise (attributes baseFrequency, numOctaves, seed, stitchTiles, type). It can distort text and images (ink bleed, wobbly edges) and, with SVG lighting, make a rough paper texture — [Codrops, Sara Soueidan](https://tympanus.net/codrops/2019/02/19/svg-filter-effects-creating-texture-with-feturbulence/).
- A practical paper-grain pipeline: fractalNoise, desaturate with `feColorMatrix`, `feBlend` overlay onto the shape. Common Paper used SVG filters to replace raster textures and cut image weight by about 99% — [Common Paper](https://commonpaper.com/blog/speeding-up-common-paper-reducing-image-size-by-99).
- Engraving-style halftone: Øyvind Kolås's "Spachrotyzer" produces AM-halftone *engraving lines* with angle/frequency driven by hue and saturation, and includes a WebGL-compatible GLSL shader — [Spachrotyzer](https://pippin.gimp.org/spachrotyzer). Stefan Gustavson's WebGL halftone dot tutorial — [LiU](https://itn-web.it.liu.se/~stegu76/webglshadertutorial/shadertutorial.html). Mike Bostock's halftone notebook — [Observable](https://observablehq.com/@mbostock/halftones).
- Codrops has a WebGL Bayer-dithering background guide — [Codrops author page](https://tympanus.net/codrops/author/zavalit).
- Deep-zoom: the Bosch documentary used Q42's Micrio for ultra-high-res zoom storytelling — [Q42](https://www.q42.nl/en/work/garden-of-earthly-delights).

### Inferences (techniques mapped to metaphors)
- **Paper grain** (feTurbulence + overlay) on every plate/card/ledger surface; a different `seed` per card so no two look identical (fits "disorganized beauty").
- **Ink bleed** on Pirata One headings: a tiny `feDisplacementMap` driven by turbulence, scale about 1–2px. Animate the seed on hover for a "wet ink" twitch.
- **Torn/deckled edges:** a `clip-path` or mask generated from noise, for notebook pages and ledger leaves.
- **Letterpress impression:** inset text-shadow (dark offset 1px down and light 1px up) on bone paper for scorecard and ledger text.
- **Engraving shader:** run product photos through a Spachrotyzer-style line-halftone so photos sit in the same visual world as the commissioned engravings. This is the key bridge between photography and the anatomical-plate system.
- **Wax seal:** a teal wax-seal SVG as the single CTA (Add to cart / Sign the ledger), which puts the brand's "teal for the one call to action" rule into practice. Press animation: scale 0.96 plus a darker inner shadow.
- **Rubber stamps:** "DEACCESSIONED" (sold out), "NO DRESS CODE", "TRIAL FAILED": rotated, multiply blend, noise-masked for patchy ink.
- **Grisaille-to-color:** CSS `filter: grayscale()` to color transitions for the triptych opening.
- Performance: SVG filters on large areas can be costly; render the grain once to a background (or a small tiling noise PNG) rather than filtering live text across the page **[unverified general practice]**.

### Gaps
- No Codrops tutorial specifically on engraving shaders was found; no verified brand-site examples of wax-seal CTAs or torn-edge masks in this session.

## Q4. Brand-safety constraints across metaphors (Bolton, the tee)

### Takeaway
The anatomical/flaying vocabulary is the brand's strongest asset and its largest Bolton risk. Keeping it clinical (specimen plate, lettered callouts, golf ball subject, bone/teal palette) rather than heraldic (banners, shields, a flayed human, pink/red fields) keeps the two apart. Patent drawings and lab notebooks are the metaphors most likely to leak the tee's mechanism.

### Cited Findings
- Brand doc caution: the X-board-with-flayed-body is House Bolton's sigil core. Keep it a golf ball, no human figure, no pink or red field, and get a trademark view — `docs/brand/brand.md` (internal).
- Patent drawings exist to disclose how an invention works (inherent in patents; see 37 CFR drawing rules cited above) — [USPTO archived](https://webarchive.library.unt.edu/web/20120928002250/http://www.uspto.gov/main/ccpubguide.htm).

### Inferences
- Avoid **heraldic** framing anywhere the X board appears: no banners, shields, or "house" language, and no red/pink grounds (use rust/ochre sparingly, never as a field behind the board).
- Keep flayed or anatomical imagery to *objects* (balls, gloves, caps as "specimens"). Human anatomy plates (Albinus, Vesalius) can appear as texture or margin pieces, but never combined with the X board.
- Tee content in the Build Log or patent styling should be redacted, cropped or shown only as an exterior silhouette. A visible redaction bar fits the "secret workshop" tone and protects the mechanism.

### Gaps
- No trademark or legal review was done. Whether specific metaphors (e.g. the flaying-board motif on navigation icons) increase Bolton confusion risk needs counsel.
