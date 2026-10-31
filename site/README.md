# Flay Golf website

The flaygolf.com site, built with [Astro](https://astro.build). Right now it's a
pre-launch home page: the logo, the pitch, and a waitlist signup. The build log,
video, About page and shop come next.

## Run it locally

Needs Node 22.22 or newer.

```bash
cd site
npm install
npm run dev      # http://localhost:4321
npm run build    # outputs the finished site to site/dist
```

## Connect the waitlist (Kit)

1. Create a free account at [kit.com](https://kit.com). The free plan covers up
   to 10,000 subscribers.
2. Create a form, open its embed code, and copy the number in the form's URL
   (`https://app.kit.com/forms/<FORM_ID>/subscriptions`).
3. Set `PUBLIC_KIT_FORM_ID=<FORM_ID>` as an environment variable in the hosting
   settings (and in `site/.env` for local testing).

Until it's set, the form shows "Signups open soon" instead of submitting.

## Deploy (Cloudflare Pages, free)

1. Create a free Cloudflare account and go to **Workers & Pages → Create →
   Pages → Connect to Git**, then pick this repository.
2. Build settings:
   - Root directory: `site`
   - Build command: `npm run build`
   - Output directory: `dist`
   - Environment variables: `NODE_VERSION` = `22.22.0`, and `PUBLIC_KIT_FORM_ID`
     once you have it
3. Every push to `main` then redeploys the site automatically.
4. To use flaygolf.com, add it under the project's **Custom domains** once
   you've registered it.

## Notes

- The logo in `src/assets/logo.png` is cropped from
  `docs/brand/logo-reference.png`, the AI-generated reference. Swap in the
  illustrator's final art when it's ready.
- Selling will use Stripe Payment Links, added with the shop.
