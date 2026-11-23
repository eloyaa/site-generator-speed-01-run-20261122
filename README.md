# site-generator-speed — a faster drop-in for makefun-site 1.0.0

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/eloyaa/site-generator-speed-01-run-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/eloyaa/site-generator-speed-01-run-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A dependency-free, hand-optimised reimplementation of make.fun's own
`makefun-site` 1.0.0 static site generator (the reference program of the
[site-generator-speed-01-run-20261122](https://make.fun/competition/site-generator-speed-01-run-20261122)
bounty). `solve.js` exports `run(input)`, which returns exactly the output
tree makefun-site 1.0.0's `build` returns for the same input — every page,
index and tag page, `build-report.txt` and every static file, byte for byte —
but considerably faster.

`main.ts` is the kit's driver: it reads every case from `/input/cases.json`,
answers each with `run`, and writes `/output/answers.json`.

## How it is faster

The reference is a clean, straightforward implementation; this entry keeps
its semantics exactly and removes the work that costs the most CPU:

- HTML escaping and Markdown inline rendering are single-pass, regex-free
  scans that move whole runs of plain text at once instead of escaping one
  character at a time.
- The Markdown block lexer dispatches on character codes instead of running
  a dozen regular expressions on every line.
- Front matter is parsed by scanning lines directly, and a source's body is
  taken as a slice of its text instead of being split and re-joined.
- `slugify` results are cached per build (titles, tags and headings repeat),
  and each source's reading time, excerpt, card and table of contents are
  computed once and reused.
- Files without a carriage return skip the line-ending rewrite entirely.

Nothing is cached from one case to the next: every `run(input)` builds its
own caches, and the output depends on its input alone.

## Layout

- `main.ts` — the driver (from the kit).
- `solve.js` — the fast implementation (front matter, Markdown, templates,
  and the build pipeline).
- `package.json` — `{ "type": "module" }`.

## Licence

MIT — see `LICENSE`.

---

<sub>Entered on [make.fun](https://make.fun/bounty/site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
