# site-generator-speed-20261122

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/ehsanef/site-generator-speed-20261122/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/ehsanef/site-generator-speed-20261122/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

A faster drop-in for make.fun's own makefun-site 1.0.0: builds static sites and answers exactly as makefun-site does on every case, much faster.

## Run it

```sh
node main.ts
```

`main.ts` reads every case from `/input/cases.json` (or `$MAKEFUN_INPUT_DIR`), answers each with `run()` from `solve.js`, and writes `/output/answers.json` (or `$MAKEFUN_OUTPUT_DIR`).

## How it is fast

Same answers as makefun-site 1.0.0, byte for byte, in about a third of the time on the practice set (solve-only, measured locally beside the house baseline: ~0.14 s vs ~0.49 s; wall clock ~0.58 s vs ~1.06 s including the same JSON input/output both pay). The wins, all semantics-preserving:

- **Markdown renders in one pass**: lexing and rendering are fused, and inline markup is found by a native scan for the next character that needs attention, so long plain runs are pushed whole instead of escaped a character at a time.
- **Slugs are cheap**: ASCII titles and tag names take a one-pass scan (NFKD is the identity on ASCII) and repeat slugs are memoised per case.
- **No wasted work**: a post's card (including its word count) is built once and reused for its page; front matter is parsed from the header region only, the body sliced out whole; template loops reuse one scope and one loop record per loop.
- **Less garbage**: rendering appends into arrays joined once, instead of concatenating strings a node at a time.

## Layout

- `solve.js` — the fast site builder: `run({site, templates, files})` returns the output tree
- `main.ts` — the harness driver

---

<sub>Entered on [make.fun](https://make.fun/?ref=qzcjfqr9&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
