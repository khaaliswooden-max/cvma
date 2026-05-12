# Contributing to CVMA

Thanks for your interest. CVMA is a pre-alpha architecture project with an
active provisional patent (`ip/provisional-patent-application.md`), so a few
things differ from a typical open-source repo. Please read this file before
opening a PR.

## License and Patent Grant

CVMA is licensed under Apache-2.0 (see `LICENSE`). By submitting a
contribution you agree that:

1. Your contribution is licensed under Apache-2.0, including the patent grant
   in Section 3 of that license.
2. You have the right to submit it — it is your own work, or you have
   permission from the rights holder to contribute it under Apache-2.0.
3. You understand that contributions may be incorporated into material
   referenced by the CVMA provisional patent application. If your
   contribution embodies an invention you wish to retain separately, say so
   explicitly in the PR description before it is merged.

We use the [Developer Certificate of Origin](https://developercertificate.org/).
Sign off every commit:

```
git commit -s -m "your message"
```

This adds a `Signed-off-by:` trailer. PRs without sign-off will not be merged.

## What to Contribute

Useful right now:

- Corrections to the architecture documents in `design/`, `compliance/`,
  `rsi/`, `maas/`, `infra/`.
- Additional prior art for `ip/prior-art-survey.md`. Cite sources.
- Stubs and tests under `src/` consistent with the architecture.

Hold off on:

- Large refactors of `src/` — the module boundaries are still moving.
- New compliance frameworks without a mapping to the canonical asset graph.

## Workflow

1. Open an issue describing the change before writing code, unless it is a
   small fix (typo, broken link, obvious bug).
2. Branch from `main`. Keep PRs focused — one logical change per PR.
3. Match the existing style. Markdown uses semantic line breaks where the
   surrounding file already does.
4. For code changes, include tests when a test harness exists for the module.
5. Reference the issue in the PR description.

## Reporting Security Issues

Do not open a public issue. See `SECURITY.md`.

## Questions

Open a GitHub Discussion or a draft issue tagged `question`.
