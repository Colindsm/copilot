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

The domain is registered at Squarespace (it moved over from Google Domains) and currently shows a "Coming Soon" page. Grayson's earlier draft was published by uploading a folder with a single `index.html` to Cloudflare, and the new site goes up the same way.

### 1. Upload the new site (about 5 minutes)

1. Make the upload package. Create a folder (for example `graysons-cloudflare-pages`) and copy `public/index.html` **and the `public/images/` folder** into it. You can also zip the folder's contents as `graysons-cloudflare-pages.zip`.
2. Log in to Grayson's Cloudflare account at dash.cloudflare.com.
3. Go to **Workers & Pages** and open the project that holds his draft.
   - If it's a **Pages** project: open **Deployments → Create deployment**, then drag in the folder or zip.
   - If it's a **Worker** (the draft's address ends in `workers.dev`): it's simplest to make a new Pages project. Go to **Create → Pages → Upload assets**, name it `graysons-detailing`, and drag in the folder or zip.
4. Cloudflare gives you a preview link ending in `.pages.dev`. Open it on a phone and check it.

To update the site later (new photos or prices), edit `index.html` and upload it again the same way.

### 2. Point graysonsautomotivedetailing.com at it

1. In the Pages project, go to **Custom domains → Set up a custom domain** and enter `graysonsautomotivedetailing.com`. Then do the same again for `www.graysonsautomotivedetailing.com`.
2. Cloudflare asks to manage the domain's DNS. Pick the **Free** plan, and Cloudflare copies over the existing DNS records.
   - ⚠️ **Before going further, check email.** If grayson@graysonsautomotivedetailing.com is a real mailbox, make sure its **MX** and **TXT** records came across. Otherwise email stops working.
3. In **Squarespace → Domains → graysonsautomotivedetailing.com → DNS → Nameservers**, choose **Use custom nameservers** and enter the two nameservers Cloudflare shows you.
4. Wait about an hour. Then the domain shows the new site, with HTTPS set up automatically.

### Advanced: command line

`wrangler.jsonc` in this folder deploys `public/` as a Cloudflare Worker with `npx wrangler login && npx wrangler deploy`. This is an alternative to the upload method above.
