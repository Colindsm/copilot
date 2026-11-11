# Grayson's Automotive Detailing website

The new site for **graysonsautomotivedetailing.com**: a single static page with no build step.

```
graysons-detailing/
├── public/
│   ├── index.html   ← the whole site (HTML, CSS and JS in one file)
│   └── images/      ← put work photos here
└── wrangler.jsonc   ← Cloudflare Workers config
```

## Still needed from Grayson

- [x] **Photos.** Five are in `public/images/`: the Porsche 911 hero shot, the Lightning, the hood reflection, the foam wash and the Grand National with his banner.
- [x] **Phone:** (613) 602-6096, for calls and texts.
- [ ] **Prices,** if he wants them shown. Each service card currently says "Priced by vehicle".
- [ ] **Mobile or shop?** Does he come to the customer, or do they drop off? Add it to the facts in the hero and to step 3.
- [ ] **Reviews.** Two or three customer reviews from Facebook would fit well between "How booking works" and "About".
- [ ] **The quote in About.** It was adapted from his draft ("…the one that wins us our third year"). Check that he's happy with the wording.

### Adding more photos

1. Resize the photo to about 1800px on the long side, and save it as a JPEG in `public/images/`.
2. Add a line to `PHOTOS` near the bottom of `index.html`:
   ```js
   { src: "images/new-photo.jpg", w: 1800, h: 1350, caption: "What it shows" },
   ```
   `w` and `h` are the photo's width and height in pixels.

## Award record (as given)

| Year | Renfrew County |
|------|----------------|
| 2023 | 1st |
| 2024 | 1st |
| 2025 | 2nd |
| 2026 | 1st |

It appears in the hero, the rosette, the podium strip, the About section, and the JSON-LD `award` list in `<head>`. Update every place each year.

## How the booking form works

There is no server. When someone fills in the form and taps **Prepare my request**, the page writes up their request. They can then:
- **Open in email.** Their email app opens with the request addressed to grayson@graysonsautomotivedetailing.com.
- **Send on Messenger.** The request is copied and Messenger opens to Grayson's page (m.me/graysonsdetailing), so they just paste it in.
- **Copy request.** They paste it wherever they like.

## Preview locally

Double-click `public/index.html` to open it in any browser.

## Going live on graysonsautomotivedetailing.com

**The domain and email stay at Squarespace.** Squarespace keeps running the domain's DNS and its email forwarding. Nothing about mail is changed. The site itself is hosted on **Cloudflare Pages** in Grayson's Cloudflare account, as the project `graysons-detailing`.

### Updating the site

The live site is the Cloudflare Pages project **graysons-detailing** (https://graysons-detailing.pages.dev) in Grayson's account.

It's deployed as a single bundled file, because the session that set it up couldn't use Cloudflare's normal asset upload:

```sh
cd graysons-detailing
python3 build-worker.py dist-worker          # bundles public/ (page + images) into dist-worker/_worker.js
curl -X POST -H "Authorization: Bearer $CLOUDFLARE_API_TOKEN" \
  "https://api.cloudflare.com/client/v4/accounts/38e6ba3cb2fba1b47bb4a55a6e7bf97d/pages/projects/graysons-detailing/deployments" \
  -F 'manifest={}' -F 'branch=main' \
  -F "_worker.js=@dist-worker/_worker.js;filename=_worker.js;type=application/javascript+module"
```

The bundle must stay under Cloudflare's 3 MB compressed limit. With 5 photos it's about 2.1 MB, so resize any new photos to about 1800px and keep only the best ones.

Alternatively, in the Cloudflare dashboard, open the **graysons-detailing** project and create a new deployment by uploading the `public` folder.

### How the domain is connected (live since Nov 11, 2026)

Squarespace still runs the domain's DNS and email. **Email records were not changed.**

The website is reached through a **Squarespace domain forwarding rule**:

| From | To | Type |
|---|---|---|
| graysonsautomotivedetailing.com, www.graysonsautomotivedetailing.com | https://graysons-detailing.pages.dev | Permanent (301), SSL on |

Visitors who type his domain land on the site, and the address bar then shows `graysons-detailing.pages.dev`.

**Optional upgrade, to keep his domain in the address bar:**
1. Remove `www` from the forwarding rule, and point the `@` rule at `https://www.graysonsautomotivedetailing.com`.
2. In Squarespace DNS, set a CNAME record: `www` → `graysons-detailing.pages.dev`.

`www.graysonsautomotivedetailing.com` is already registered as a custom domain on the Pages project, so Cloudflare issues its certificate once the CNAME exists.
