# Demo video assets (Phase B)

When the product demo is recorded, add files here and update the HTML embed on `index.html` and `product.html` (`#demo`).

## Files

| File | Purpose |
|------|---------|
| `demo.mp4` | Primary walkthrough (~60–90s), H.264, GitHub Pages–friendly size |
| `demo-poster.jpg` | Poster frame for `<video poster="...">` and slow networks |

Optional: host on YouTube/Vimeo and replace the `<video>` block with an iframe.

## Embed steps

1. Place `demo.mp4` and `demo-poster.jpg` in this directory.
2. On **index.html**, inside `.demo-video-slot`, replace the placeholder with:

```html
<video class="demo-video" controls playsinline muted loop poster="assets/demo-poster.jpg">
  <source src="assets/demo.mp4" type="video/mp4">
</video>
```

3. On **product.html** (`#demo`), same replacement inside `.demo-video-slot`.
4. Remove or hide the “Recording in progress” label and storyboard-only note if you want video-only above the fold (storyboard can remain below).
5. Optional: `muted loop` on home only for autoplay-friendly hero (respect `prefers-reduced-motion` in a follow-up if needed).

## Recording checklist

See `docs/site_messaging.md` — section “Recording script”.
