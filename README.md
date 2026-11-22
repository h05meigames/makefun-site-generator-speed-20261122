# makefun-site, faster

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/h05meigames/makefun-site-generator-speed-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/h05meigames/makefun-site-generator-speed-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Entry for [site-generator-speed-01-run-20261122](https://make.fun/bounty/site-generator-speed-01-run-20261122)
— "Build a static site faster, 22 November 2026".

A faster drop-in for make.fun's own `makefun-site` 1.0.0, written for the
competition [site-generator-speed-01-run-20261122](https://make.fun/competitions/site-generator-speed-01-run-20261122)
("Build a static site faster, 22 November 2026").

`solve.js` (an ES module, `run(input)`) returns, for every input, byte for
byte the answer `makefun-site` 1.0.0's `build` returns: every page's
`index.html`, the index and tag pages, `build-report.txt` and each static
file. `main.ts` is the harness driver: it reads every case from
`/input/cases.json`, answers each with `run`, and writes
`/output/answers.json`.

## How it is faster

The reference spends its time on small repeated work, so this implementation
does the same work in bulk:

- Inline Markdown is escaped a run at a time instead of one character at a
  time (plain runs go through one regex replace; only the markup characters
  are handled singly).
- Markdown blocks render straight into an output array instead of first
  becoming token objects.
- Slugs are memoised per case (tag names and headings repeat heavily), with
  an ASCII fast path that skips Unicode normalisation.
- Front matter is found by searching for its closing line and the body is
  sliced out whole, instead of splitting the whole source and re-joining it.
- Template output is collected in one array and joined once, instead of
  concatenating a string at every node.
- Each source's card is built once and reused for its page, instead of twice
  for every post.

Nothing is cached from one case to the next: every per-case memo is created
fresh inside `run()`.

## Verification

- Practice set: 3/3 cases exact against the kit's
  `practice/reference/expected.json`, and the whole program's `answers.json`
  byte-identical to the house baseline's.
- Fuzzed against the reference on thousands of generated cases (front-matter
  refusals, template dialect edge cases, Markdown edge cases, Unicode slugs,
  tag/index pages, broken links, static shadowing) — all exact.
- Faster than the reference on the practice set on this machine (see
  HANDOFF.md for the numbers).

## Referral

https://make.fun/r/0ene8b3q

## License

MIT — see [LICENSE](LICENSE).
