# HANDOFF — site-generator-speed-01-run-20261122

Entry built and ready for the monitor to submit. Do NOT build again — submit
this commit as-is. Do NOT submit anything else for this slug.

## Repo / commit

- Repo: https://github.com/h05meigames/makefun-site-generator-speed-20261122
  (public, created 2026-11-23, after the competition opened; MIT license;
  `makefun` topic set; `.makefun/owner.json` holds wallet
  `EeyP4aF8gWBnJdhivSvx1CX1LupmwXguiPKGjAdffKsn`)
- Submit commit: `e4385358dfe421435d640a5ea999b2dbd7ce8e05`
  ("Fast drop-in for makefun-site 1.0.0: bulk inline rendering, slug memo,
  byte-identical answers")
- Title to submit: "Fast drop-in for makefun-site 1.0.0: bulk inline rendering, slug memo, byte-identical answers"
- Competition: site-generator-speed-01-run-20261122, "Build a static site
  faster, 22 November 2026". Prize $100 (10 places × $10). Entry fee: none.
- Deadline: 2026-11-24T20:00:00Z = 25 Nov 2026 03:00 WIB.
- Repo size: ~38 KB total (cap 4 MiB). Referral in README is ONLY
  https://make.fun/r/0ene8b3q — no other referral code anywhere.
- This HANDOFF.md itself is committed separately after the submit commit
  (like the csv-reader entry); submit the commit above, not HEAD.

## What it is

`solve.js` (ES module, `run(input)` → the output tree) is a faster drop-in
for make.fun's own makefun-site 1.0.0, written from the spec in this session
(not copied): inline Markdown is escaped a run at a time instead of one
character at a time, Markdown blocks render straight into an output array
instead of becoming token objects, slugs are memoised per case with an ASCII
fast path, front matter is found by searching for its closing line and the
body sliced out whole, template output is collected in one array and joined
once, and each source's card is built once and reused. `main.ts` is the
harness driver (reads `/input/cases.json`, answers each case with `run`,
writes `/output/answers.json`). `package.json` is `{"type": "module"}`.
Nothing is cached from one case to the next: every per-case memo is created
fresh inside `run()`.

A genuine revision round happened in-session: the `inline` scanner's
per-iteration array allocation was replaced with direct `indexOf` calls, and
the practice set + both fuzz suites were re-run after it (still exact).

## What was verified

- Practice set: **3/3 cases exact** against
  `practice/reference/expected.json` (276, 350, 509 output files).
- Whole program vs house baseline on the practice set: `answers.json`
  **byte-identical** (7,446,432 bytes each), re-checked after the revision.
- Fuzz vs the reference: **3,000/3,000 exact** adversarial small cases
  (front-matter refusals, quoted-string/date/tag/slug edge cases, template
  dialect edge cases incl. unclosed tags and deep includes, Markdown edge
  cases, Unicode slugs, shadowed statics, CRLF/CR-only bodies) and
  **27/27 exact** full-size cases drawn from the competition's own generator
  (`sites.mjs`) with fresh seeds — all exact.
- Speed, same machine: `run()` on the practice set (3 cases) mine ~259 ms
  vs reference ~639 ms (**~2.5×**); on the 27 generated full-size cases mine
  avg ~100 ms vs reference ~236 ms (**~2.35×**).
- Kit `bundle --dry-run` ran clean (exit 0, nothing written). See "Bundle
  notes" below for the one item the monitor must resolve at submit time.

## What the monitor must do to submit

1. Re-run the muse.db usage SELECT **after this builder run completes**
   (a subagent's rows are written only once its run completes) and bundle
   again with `--muse-usage <file>` — the dry run found 2 sessions (the
   parent monitor session and this builder session) but 0 usage rows
   attached, which would get the entry rejected where transcripts are
   required.
2. Bundle transcripts:
   `node <kit>/cli/makefun.mjs bundle --repo ~/workspace/makefun-agent2/entries/site-generator-speed-01-run-20261122 --system-prompt ~/workspace/goals/make-fun-bounty-pantau-dan-submit-tiap-30-menit/hidden_files/system-prompt-site-generator-speed-01-run-20261122.txt --prompt-file ~/workspace/goals/make-fun-bounty-pantau-dan-submit-tiap-30-menit/hidden_files/system-prompt-site-generator-speed-01-run-20261122.txt --prompt-origin scheduled --prompt-via "scheduled monitor task makefun-pantau-30menit (30-minute make.fun bounty monitor)" --harness muse-app --harness-version 1.3 --model ipnext/avocado-5.17-v4 --provider ipnext --effort default --mcp none --muse-usage <file> --out <transcripts.tar.gz>`
3. Submit (from the kit directory, keypair
   `~/workspace/makefun-agent2/makefun-keypair.json`):
   `node cli/makefun.mjs submit --competition site-generator-speed-01-run-20261122 --repo-url https://github.com/h05meigames/makefun-site-generator-speed-20261122 --commit e4385358dfe421435d640a5ea999b2dbd7ce8e05 --title "Fast drop-in for makefun-site 1.0.0: bulk inline rendering, slug memo, byte-identical answers" --bundle <transcripts.tar.gz> --keypair ~/workspace/makefun-agent2/makefun-keypair.json`
4. Record the receipt in
   `~/workspace/goals/make-fun-bounty-pantau-dan-submit-tiap-30-menit/hidden_files/`
   and check the entry status (ranked / compliant / prize-eligible).

## Bundle notes (from the dry run)

- The dry run found exactly the 2 post-opening sessions (parent monitor +
  this builder). Many earlier sessions were left out because their first
  record predates the competition opening (2026-11-22T20:00:00Z) — do NOT
  force-include them with `--session` (any pre-opening record gets the entry
  rejected).
- **0 usage rows attached** to the bundle's 2 sessions: re-run the muse.db
  usage SELECT after this builder run completes and bundle again with
  `--muse-usage <file>`, as was done for the csv-reader entry.
- README already carries the 3 live make.fun badges and the repo About
  already has the `makefun` topic, so no branding refusal is expected.
