# Demo video

The product walkthrough is hosted on **Vimeo** (video ID `1195379221`) and embedded on `index.html` and `product.html` (`#demo`) inside `.demo-video-embed`.

To change the video, update the iframe `src` on both pages (or swap to a self-hosted file below).

## Self-hosted alternative (optional)

| File | Purpose |
|------|---------|
| `demo.mp4` | Primary walkthrough (~60–90s), H.264, GitHub Pages–friendly size |
| `demo-poster.jpg` | Poster frame for `<video poster="...">` |

Replace `.demo-video-embed` with:

```html
<video class="demo-video" controls playsinline poster="assets/demo-poster.jpg">
  <source src="assets/demo.mp4" type="video/mp4">
</video>
```

## Recording checklist

See `docs/site_messaging.md` — section “Recording script”.
