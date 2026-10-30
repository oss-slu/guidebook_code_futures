# Contributing

Corrections and suggestions are welcome. This is a prose repository: there is nothing to build and nothing to run.

## The most useful thing you can send

**Tell us where it assumes knowledge you do not have.** That is the failure the authors cannot see, and it does not need to arrive as a pull request. An issue saying "I did not understand this paragraph" is a real contribution, and a better one than a typo fix.

Also valuable:

- A finding described in a way that does not match what the tool actually reports
- Advice that would be wrong for the kind of project you work on
- A worked example that does not match the report you received

## Making a change

Fork, branch, open a pull request. `main` is protected: one approving review, conversations resolved.

Branch names follow the program convention: `feature-*`, `bugfix-*`, `docs-*`.

Small corrections — a typo, a broken link, a sentence that reads badly — need no discussion. Open the pull request.

Changes to what the guidebook *claims* are worth an issue first, so we can agree on the point before anyone writes paragraphs.

## House style

Matched to [`guidebook_tech_debt`](https://github.com/oss-slu/guidebook_tech_debt), which this is the sibling of:

| Element | Convention |
|---|---|
| Headings | `## **Bold Title**` |
| Voice | Second person, addressed to the reader |
| Trade-offs | Stated in the body, never footnoted |
| Sources | Named and attributed, book titles italicised |
| Patterns | Named in quotes as their own headings |

Two deliberate departures from the sibling, both worth preserving:

- **Real fenced code blocks.** That repository is a Google Docs export and has none, so its code examples render as prose. The workflow snippet in §6 has to be correct on first attempt, and a broken code block would make it wrong for everybody at once.
- **No backslash escapes**, and no trailing double-space line breaks. Same cause, same reason to avoid.

## The worked example is generated

`worked-example-report.md` is **output from the Code Futures renderer**, not prose written by hand. Editing it here will be overwritten and the mismatch is caught by a test in the tool repository.

To change it, change the renderer or its fixture in [`oss-slu/code-futures`](https://github.com/oss-slu/code-futures) and regenerate. The report §3 walks a reader through has to be a report somebody could actually receive.

## Two standing constraints

**It names no repository.** Examples are aggregate ("this turns up in about a third of projects") or composite and clearly labelled as fabricated. A guidebook citing a real project's weakness is a public statement about the team holding it, and this document outlives any one cohort.

**It does not advocate one way.** Three audiences read this: other faculty who might run a program like this, students inside one, and other institutions. Where a genuine disagreement exists in the field, say so and give the reader the terms to decide. "Lint in CI" is settled practice; "block the build on lint failures" is a real trade-off.

## Conduct

By participating you agree to the [Code of Conduct](CODE_OF_CONDUCT.md).
