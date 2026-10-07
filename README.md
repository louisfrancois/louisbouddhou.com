# louisbouddhou.com

Personal site for Louis Bouddhou, senior frontend engineer.

Hand-written HTML, CSS, and JavaScript. No framework, no dependencies, no build step.
One request, roughly 20 KB over the wire.

The page instruments itself with `PerformanceObserver` and reports its own LCP, CLS, INP,
and TTFB live in the browser. The controls under the metrics cause genuine regressions
rather than animating fake numbers:

- **Inject layout shift** inserts real DOM above the viewport, which the `layout-shift`
  observer picks up and adds to CLS.
- **Block the main thread** runs a synchronous busy loop inside a `pointerdown` handler,
  which is what actually moves INP.
- **Append 5,000 nodes** adds real elements with `data-testid` attributes.
- **Reload with a slow render** re-navigates with `?degrade=1`, which runs a blocking loop
  at the top of `<body>` before anything can paint, so LCP is genuinely re-measured.

LCP is not adjustable live on purpose: the browser stops reporting it at the first
interaction, so a reload is the only honest way to change it.

## Deploy

GitHub Pages, served from the default branch root. `CNAME` points the custom domain at it.
