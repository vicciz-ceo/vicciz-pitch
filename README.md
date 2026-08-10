# vicciz — company pitch (pitch.vicciz.com)

The public company pitch. English at `/`, Hebrew at `/he/`. Static, indexable,
no build step: two self-contained HTML files plus the crawler metadata.

```
index.html      English pitch (canonical, x-default)
he/index.html   Hebrew pitch, dir="rtl"
robots.txt      Allows search + AI answer-engine crawlers explicitly
sitemap.xml     Both URLs with hreflang alternates
llms.txt        Structured summary for engines that read it (Perplexity)
DESIGN.md       The design language shared with demo.vicciz.com
vicciz-mark.svg Favicon / masthead mark
CNAME           pitch.vicciz.com
```

## Deployment

GitHub Pages serves `main` from the repository root — the same setup as
[vicciz-demo](https://github.com/vicciz-ceo/vicciz-demo). Push to `main` and the
site rebuilds. `.nojekyll` is present so no Jekyll processing runs.

DNS lives in Google Cloud DNS on the `vicciz.com` zone (project `vicciz`), which
is where the `demo` record already points at GitHub Pages:

```
pitch.vicciz.com.  CNAME  vicciz-ceo.github.io.
```

## Editing

Both pages inline their own CSS. The token block, type scale and component
rules are duplicated across `index.html`, `he/index.html` and the demo repo by
copy — [DESIGN.md](DESIGN.md) is the source of truth that keeps the copies
honest. Change a token in one place and change it in all three.

The Hebrew page is a genuine rewrite, not a translation of the English strings.
It swaps Lora/Lato for Frank Ruhl Libre/Heebo so the brand's serif-display,
sans-body structure survives the script change, and uses CSS logical properties
so the layout mirrors under `dir="rtl"`. The wordmark stays Latin lowercase
`vicciz` in every locale.

## SEO and GEO

The page is built to be quoted by AI answer engines as well as ranked:

- **Answer-first.** The opening block after the headline answers "what is
  vicciz" completely and independently — retrieval-based engines weight opening
  content heavily.
- **Semantic structure.** One `h1`, section-per-claim `h2`s, self-contained
  sections that survive being read out of context.
- **Sourced numbers.** Every figure carries an attribution line; nothing is
  published that cannot be attributed.
- **Structured data.** `Organization`, `WebSite`, `WebPage`,
  `SoftwareApplication` and `FAQPage` in one JSON-LD graph. Optional for Google's
  AI features, but a confirmed parsing aid elsewhere.
- **Explicit crawler permission.** `robots.txt` names GPTBot, OAI-SearchBot,
  ClaudeBot, PerplexityBot, Google-Extended, CCBot and others rather than
  relying on the wildcard.
- **Freshness.** `dateModified` in JSON-LD and a visible "last updated" line.
  Bump both when the content changes — stale pages lose citations.
- **Bilingual.** Reciprocal `hreflang` on both pages plus `x-default` to English.

After the DNS record resolves, verify the property in Google Search Console and
submit `sitemap.xml`.
