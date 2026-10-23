# pyb-assets — printingyourbox.com image CDN origin

Static image set for **printingyourbox.com**, served to end users through
jsDelivr's free global CDN. The main website repo (`PYB031/website`) is kept
lean by keeping these ~3,000 image files out of it.

**Base URL (set in Cloudflare Pages → Environment variables → Production):**

```
PUBLIC_IMG_BASE=https://cdn.jsdelivr.net/gh/PYB031/pyb-assets@main
```

Layout (keys match what `src/data/catalog.ts` in the website repo expects):

```
products/     product_info.image_name.image_type   e.g. 62_1542995927.jpg
sliders/      top_sliders.img_name.image_type      e.g. 1569877380.png
categories/   categories.cat_thumbnail             e.g. 10Candle_and_candle_jars.jpg
blog/         blogs.cover_image                    e.g. 2019-03-15_plastic_vs_kraft.jpg
```

Refresh workflow (run on the server where /srv/Printingyourbox lives):

```bash
# After a new ../files/ mirror pull
cd /srv/Printingyourbox/pyb-assets
rsync -av --delete /srv/Printingyourbox/files/assets/product_images/ products/
# ...etc, then
git add -A && git commit -m "Refresh images" && git push
```

jsDelivr caches by commit SHA; after a push, allow ~12 hours for the `@main`
tag to refresh globally, or deploy with `@<sha>` for immediate propagation.
