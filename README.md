# LaunchPad Consulting — website

A single static page. No framework, no build step, no dependencies, no accounts to keep alive. Open `index.html` in a browser and it works.

```
index.html      the whole site
styles.css      the whole design system
assets/
  fonts/        Archivo Black, Archivo Narrow, Courier Prime (self-hosted .woff2)
  team/         square face-centered crops, generated from photos/
  mark.png      rocket mark cropped from the logo, for the nav and favicon
  logo.png      full logo lockup
photos/         original source images — untouched, keep these
PRODUCT.md      what the firm is and does; the facts the site is built on
DESIGN.md       the visual system and its rules
```

Nothing is fetched from the internet at runtime. Fonts are local. The only external links are the four client sites and LinkedIn.

## Canonical domain and redirects

The official domain is `https://launchpadconsulting.xyz/`. The `.net` domain should
301-redirect every path to the matching `.xyz` path. `_redirects` contains the rule
for Netlify. GitHub Pages does not execute redirect files, so configure the same
redirect at the DNS/hosting provider if `.net` is also attached there. Keep the
`.xyz` domain attached to the site and keep `CNAME` set to `launchpadconsulting.xyz`.

After deployment, verify both `https://launchpadconsulting.xyz/` and
`https://launchpadconsulting.xyz/team/` return `200`, then verify a non-canonical
URL such as `https://launchpadconsulting.net/team/` returns `301` to the `.xyz`
version. Add the canonical `.xyz` URL to Google Search Console as a Domain property,
submit `/sitemap.xml`, and request indexing for `/` and `/team/` in URL Inspection.

## Deploying

Pick one. All free.

**Netlify (easiest)** — go to [app.netlify.com/drop](https://app.netlify.com/drop) and drag this whole folder onto the page. You get a URL immediately. To use your own domain, Site settings → Domain management.

**Vercel** — `npx vercel` in this folder, or drag-and-drop at [vercel.com/new](https://vercel.com/new).

**GitHub Pages** — push this folder to a repo, then Settings → Pages → Deploy from branch → `main` / root.

To update anything later: edit the file, save, re-upload the folder. There is no build step to run.

## Before you go live

Four things need your input. Nothing is broken without them — they're accuracy, not bugs.

1. **Confirm the class year.** The site says "four rising seniors" and "founded in 2025 by Advaith Ramanan while a high school junior." I took the rising-senior detail from you; the founding year came from the old site's date stamp. Check both. They appear in the Authorization block in [index.html](index.html) and in the `Founded` row of the spec table.

2. **Real testimonials.** Part 5 ships with three empty remark boxes stamped "Awaiting client signature." That is deliberate — I did not write quotes and attribute them to NextGen Golf, Manavatha, or anyone else, because a client who spots an invented quote about themselves is a client you lose. Collect real ones and replace the boxes. Suggested email:

   > Subject: Quick favour — two sentences?
   >
   > Hi [name], we're putting up a new site for LaunchPad and we'd love to include a short note from you. Two sentences on what we did and whether it helped is plenty. Totally fine to say no.
   >
   > Thanks, [your name]

   To fill one in, replace the `remark__field` div and the stamp with the quote and the person's name and title. Leave the remaining boxes as-is; a mix of filled and pending looks honest.

3. **Better headshots.** Yours and Aneesh's are phone selfies with household backgrounds; Advaith's and Ubaid's are neutral-wall portraits. The blue duotone treatment hides most of the difference, but four photos taken against the same wall in the same light would be a real upgrade. Drop new files into `photos/` and re-run the crop:

   ```
   python3 -c "
   from PIL import Image
   for name, top in [('advaith_ramanan',20),('safiullah_baig',70),('ubaid_maredia',5),('aneesh_dwarsala',200)]:
       im = Image.open(f'photos/{name}.jpeg').convert('RGB')
       w,h = im.size; s = min(w,h); t = max(0, min(top, h-s))
       im.crop((0,t,s,t+s)).resize((640,640), Image.LANCZOS).save(f'assets/team/{name}.jpg', quality=88)
   "
   ```
   Adjust the second number per photo to move the crop window up or down.

4. **Mopac Mart's link.** File MOP currently shows "In progress" with no link. When the site ships, wrap the `<h3>` text in an anchor the same way the other four are, and change the stamp to `Completed`.

## Things I changed from the old site, deliberately

- **Dropped the Twitter, Facebook, and Instagram icons.** They pointed nowhere. LinkedIn is the only social link.
- **"Local small businesses" → "small businesses and organizations."** Your roster includes a global IT consultancy and an international nonprofit. The old phrasing undersold it and wasn't accurate.
- **Rewrote the single-founder story** around the four of you, keeping Advaith as founder and CEO.
- **No client is credited with a specific service.** You confirmed the engagements but not which services each got, so the site states sector and status only. Add specifics when you're sure of them.

## If you want to change the look

[DESIGN.md](DESIGN.md) documents the system: four inks, four paper stocks, three typefaces, and the rules for boxes, stamps, perforations, and the carbon offset. The colors are CSS variables at the top of [styles.css](styles.css) — changing `--ink-blue` there recolors the whole site.

Two rules worth not breaking, because the whole thing rests on them: no rounded corners and no soft shadows. The moment either appears, it stops reading as a printed form and starts reading as a website pretending to be one.
