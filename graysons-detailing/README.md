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

The site only uses facts from his earlier draft and from family. Fill these in when you have them:

- [ ] **Photos.** Add 6–12 before/after shots from Facebook to `public/images/`. Then list them in `PHOTOS` near the bottom of `index.html`:
  ```js
  var PHOTOS = [
    { src: "images/f150-interior-after.jpg", caption: "F-150 interior, after" },
  ];
  ```
  The labelled "Photo coming soon" slots disappear once `PHOTOS` has entries. Resize photos to about 1600px wide before adding them.
- [ ] **Phone number,** if he wants calls or texts. Add a "Phone" row in the booking section.
- [ ] **Prices,** if he wants them shown. Each service card currently says "Priced by vehicle".
- [ ] **Mobile or shop?** Does he come to the customer, or do they drop off? Add it to the facts in the hero and to step 3.
- [ ] **Reviews.** Two or three customer reviews from Facebook would fit well between "How booking works" and "About".
- [ ] **The quote in About.** It was adapted from his draft ("…the one that wins us our third year"), since he has now won a third. Check that he's happy with the wording.

## Award record (as given)

| Year | Renfrew County |
|------|----------------|
| 2023 | 1st |
| 2024 | 1st |
| 2025 | 2nd |
| 2026 | 1st |

It appears in the hero, the rosette, the podium strip, the About section, and the JSON-LD `award` list in `<head>`. Update every place each year.

## How the booking form works

There is no server. When someone fills in the form and taps **Prepare my request**, the page writes up their request. They can then open it in their email app, already addressed to grayson@graysonsautomotivedetailing.com, or copy it and send it by Facebook message.

## Preview locally

Open `public/index.html` in a browser, or run:

```sh
npx wrangler dev
```

## Going live on graysonsautomotivedetailing.com

The domain is registered at Squarespace (it moved over from Google Domains) and currently shows a "Coming Soon" page.

1. **Deploy.** Log in to Grayson's Cloudflare account, where his draft `nameless-glade-6630` lives:
   ```sh
   cd graysons-detailing
   npx wrangler login
   npx wrangler deploy
   ```
   Alternatively, paste `public/index.html` over the draft Worker's assets in the Cloudflare dashboard.
2. **Add the domain to Cloudflare.** In the Cloudflare dashboard, go to **Add a site**, enter `graysonsautomotivedetailing.com` and pick the Free plan. Cloudflare imports the existing DNS records.
   - ⚠️ **Check email before continuing.** If grayson@graysonsautomotivedetailing.com is a working mailbox (Google Workspace or similar), make sure its **MX** and **TXT** records came across. Otherwise email stops working.
3. **Switch nameservers.** In Squarespace go to **Domains → graysonsautomotivedetailing.com → DNS → Nameservers → Use custom nameservers**. Enter the two nameservers Cloudflare gives you. The change usually takes effect within an hour.
4. **Attach the domain to the Worker.** In Cloudflare go to **Workers & Pages → graysons-detailing → Settings → Domains & Routes → Add → Custom domain**. Add both `graysonsautomotivedetailing.com` and `www.graysonsautomotivedetailing.com`. HTTPS is set up automatically.
