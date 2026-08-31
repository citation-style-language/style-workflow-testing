# style-workflow-testing

A scratch repository for exercising the `Pull request feedback` workflow
(`.github/workflows/sheldon.yaml`) and [Sheldon][sheldon] without opening noise on
[citation-style-language/styles][styles].

It exists because `pull_request_target` always runs the workflow file from the **base**
branch, so a change to `sheldon.yaml` cannot be tested by the pull request that makes it.
The only way to see a change run is to land it on a default branch first — which, on the
real styles repo, means merging something untested that comments on every incoming PR.

## What's here

Everything the styles test suite needs, and nothing else:

| | |
|---|---|
| `institute-of-physics-numeric.csl`, `american-association-of-petroleum-geologists.csl` | independent styles, neither with a `template` link |
| `dependent/2d-materials.csl`, `dependent/aapg-bulletin.csl` | one dependent per independent, so the parent-link checks have something to resolve |
| `spec/` | the styles suite, verbatim, except for a trimmed `filters.yaml` |
| `Gemfile`, `Gemfile.lock`, `Rakefile` | verbatim from styles |
| `renamed-styles.json` | empty; `spec/repository_spec.rb` requires the file to exist |

Full test suite, four styles instead of ~10,000 — a run takes seconds rather than minutes.

## Keeping it in sync

`spec/`, `Gemfile*` and `Rakefile` are copies. When the real repo's suite changes, re-copy
them. `spec/filters.yaml` is deliberately *not* a copy: its exception lists exist to
grandfather in real styles, and none of them apply here. Its `EXTRA_FILES` list does have
to be updated whenever a non-`.csl` file is added here, or the "may not contain extra
files" example fails.

## Differences from the styles workflow

`.github/workflows/sheldon.yaml` is a copy with two deliberate changes, both marked
`TESTING:` in the file:

- `SHELDON_REPO: styles` — Sheldon picks its asset extension and comment templates from
  the repository name, and would otherwise refuse to run here.
- the `commit reindented styles` repository guard names this repo.

Keep every other difference at zero; a divergence here is a test that proves nothing about
production.

## Using it

Open a PR that touches a `.csl` file — from a fork to exercise the untrusted path, from a
branch for the trusted one. To test the `safe to test` path, apply that label; note it is a
one-shot authorisation, revoked automatically on the next push.

[sheldon]: https://github.com/citation-style-language/Sheldon
[styles]: https://github.com/citation-style-language/styles

<!-- label-gun review smoke test -->
