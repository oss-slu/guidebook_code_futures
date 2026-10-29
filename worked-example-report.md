# **Code Futures Report: `oss-slu/tidewater-atlas`**

> ### **This report is fabricated.**
>
> `tidewater-atlas` **is not a real repository.** No project by that name exists
> in `oss-slu` or anywhere else, and nothing here describes any actual team's
> codebase. It was written by hand to give the guidebook something concrete to
> point at, and it is deliberately composite: it carries a mix of findings that
> no single real project would be likely to show all at once, so that every kind
> of outcome appears somewhere.
>
> Your own report will look exactly like this one in shape — the same sections,
> in the same order, with the same vocabulary. Only the findings differ.

| | |
| ----- | ----- |
| **Repository** | `oss-slu/tidewater-atlas` |
| **Commit** | `a0408fc` |
| **Collected** | 2026-11-01 14:00 UTC |
| **Schema** | `code-futures/v1` |
| **Stacks detected** | Python, Node |
| **Classification** | Internal — shareable within the program, not published |

---

## **How to read this**

This report says **which engineering practices are in place** in this
repository. It does not say whether the code is good, and it is not an input to
anybody's grade.

Every finding is one of four outcomes:

* **enforced** — the practice is in place *and* something in CI runs it
* **present** — it is in the repository, but nothing runs it automatically
* **absent** — we looked, and it is not there
* **unreadable** — **we** could not find out. This is our gap, not yours, and
  it is never evidence of anything about this repository

The sections below are always the same, in the same order, whether or not they
found anything. A section that says "nothing found here" is telling you
something; a section that was quietly left out would not be. That is so two
people can put two reports side by side and talk about the difference.

The companion guidebook explains what each probe means and — more importantly —
what it would be wrong to conclude from it. Read §3 before acting on anything
below.

---

## **Outcomes at a glance**

| Outcome | Count |
| ----- | ----- |
| enforced | 6 |
| present | 7 |
| absent | 8 |
| unreadable | 1 |

**Twenty-two findings from twenty probes.** The two numbers differ because this
repository has two stacks, and `lint` and `format` are answered once per stack —
see §2 below.

**There is no score here, and no total.** The counts are a way to find your
place in the report, not a measurement. Twenty probes answered is not twenty
points available, and a repository with more `absent` findings than another is
not behind it — the two may not be trying to do the same things.

---

## **1. Open Source Baseline**

Whether the repository carries the documents an outside contributor needs in
order to arrive, understand the project, and contribute without asking a person
first.

| Probe | Outcome | What we found |
| ----- | ----- | ----- |
| `practice.readme` | **present** | `README.md` at the repository root |
| `practice.contributing` | **present** | `CONTRIBUTING.md` at the repository root |
| `practice.code_of_conduct` | **absent** | No `CODE_OF_CONDUCT.md` in the root, `docs/`, or `.github/` |
| `practice.license` | **present** | A `LICENSE` file exists, but its text does not match any OSI-approved licence we recognise. No SPDX identifier could be assigned |
| `practice.issue_template` | **present** | `.github/ISSUE_TEMPLATE/bug_report.md` |
| `practice.pull_request_template` | **absent** | No pull request template found |
| `practice.security_policy` | **absent** | No `SECURITY.md` |

**On the licence.** A licence file that cannot be identified is its own
finding, and a different one from having no licence at all. Someone did the
work of deciding this project should be licensed. What is missing is the part
that makes the decision legible to a machine, to a potential contributor, or to
a lawyer — which, for a public repository, is most of the value. The usual
cause is a licence that was edited, retyped, or truncated. Replacing it with the
verbatim text from [choosealicense.com](https://choosealicense.com) is a
ten-minute job.

---

## **2. Linting and Formatting**

Whether a tool checks style and common errors, and whether anything makes that
check happen.

| Probe | Stack | Outcome | What we found |
| ----- | ----- | ----- | ----- |
| `lint` | Python | **enforced** | `ruff` configured under `[tool.ruff]` in `pyproject.toml`, invoked at `.github/workflows/ci.yml:31` |
| `lint` | Node | **present** | `eslint` configured in `frontend/.eslintrc.json`, with no CI step that invokes it |
| `format` | Python | **enforced** | `ruff format --check` at `.github/workflows/ci.yml:35` |
| `format` | Node | **absent** | No `prettier` configuration, and no formatting step in any workflow |

**Two stacks, two different answers, and that is the finding.** This repository
is Python and Node, so it gets a row for each. The Python side is in good
shape. The Node side has a linter that someone configured and that nothing ever
runs — so it catches problems on the machine of whoever remembers to invoke it,
and nowhere else.

That gap between **present** and **enforced** is the most actionable thing in
this report, because closing it is a few lines of YAML rather than a project.
The guidebook calls this pattern **"Configured But Never Run"** and §5 works
through it.

---

## **3. Testing**

Which *kinds* of testing this project does. We report kinds, not a coverage
percentage — see the note below.

| Probe | Outcome | What we found |
| ----- | ----- | ----- |
| `test.unit` | **enforced** | `pytest` configured in `pyproject.toml`, invoked at `.github/workflows/ci.yml:38` |
| `test.integration` | **absent** | No separate integration job, and no marked integration suite |
| `test.e2e` | **absent** | No Playwright, Cypress, or Selenium configuration found |
| `test.coverage` | **present** | `pytest-cov` is a declared dependency, but no CI step invokes it |
| `test.mutation` | **absent** | No mutation testing tool configured |

**No coverage percentage appears in this report, deliberately.** Coverage
measures lines *executed*, not behaviour *verified*: a module at 80% coverage
whose tests assert nothing is at 0% in every sense that matters. Naming which
kinds of test run is the finding a team can act on, because it shows you which
kind you do not have.

**Absent is not the same as wrong.** Four of the five kinds above are absent
here, and for a project at this stage that may be entirely correct. End-to-end
tests on a service with no user-facing flows yet would be effort spent on
scaffolding that will be rewritten. The question this section answers is "which
kinds does this project run", not "which kinds should it".

---

## **4. Review and Integration**

Whether code is reviewed before it lands, and whether anything is run against it
when it does.

| Probe | Outcome | What we found |
| ----- | ----- | ----- |
| `review.required` | **present** | Branch protection is enabled on `main`, but `required_approving_review_count` is `0`. Status checks are not required, and administrators are not subject to the rules |
| `ci.present` | **enforced** | Three workflows found: `CI`, `CodeQL`, `Dependabot Updates` |

**Protection that does not gate on review is weaker than it looks**, and this
is the reason `review.required` reads **present** rather than **enforced**.
The branch is protected, so the settings page shows a reassuring green state —
but with zero required approvals, nothing actually stops a pull request from
merging unreviewed. Administrators being exempt compounds it: the rules describe
an intention rather than a constraint.

This is a configuration change rather than a piece of work, and §7 of the
guidebook covers what making it binding costs a team in practice, because the
answer is not zero.

---

## **5. Security and Dependencies**

| Probe | Outcome | What we found |
| ----- | ----- | ----- |
| `security.sast` | **unreadable** | The Scorecard `SAST` check did not complete within the timeout for this repository. **We could not establish anything here** |
| `security.token_permissions` | **absent** | Workflows do not declare a top-level `permissions:` block, so jobs run with the repository default |
| `security.workflow_safety` | **enforced** | No dangerous workflow patterns detected. No untrusted input reaches a `run:` step, and no `pull_request_target` is used with a checkout of the head ref |
| `deps.update_tool` | **enforced** | Dependabot configured at `.github/dependabot.yml`, and update pull requests are being opened |

**`security.sast` is unreadable, and that means nothing about this project.**
A probe that failed on our side is reported as a failure on our side. This
repository has a `CodeQL` workflow — visible in §4 — so there is good reason to
think SAST is running here; we simply could not confirm it through this channel
on this run. Had we rendered this as **absent**, the report would have accused
the team of missing something they had already done, which is the single worst
thing a document like this can do.

If an outcome in your own report is **unreadable** and you believe it is wrong,
that is worth telling us. It is a bug in the tooling, not a dispute about your
codebase.

---

## **What we could not read**

| Probe | Why |
| ----- | ----- |
| `security.sast` | Scorecard check timed out |

One probe out of twenty. If this list were long — or if it were everything —
the right conclusion would be that the sweep failed, not that the repository is
empty.

---

## **Where to start**

Pick one thing. The guidebook argues this at length in §5, and the short version
is that a team that picks six things finishes none of them, while a team that
picks one finishes it and picks another.

If it were our call, here, it would be **the licence**, and the reasoning is
worth seeing rather than just the answer:

* It is the smallest piece of work on the list — ten minutes, no design
  decisions, no coordination.
* It is the only finding here with consequences **outside** the team. An
  unidentifiable licence means a potential contributor cannot tell whether they
  are allowed to use this, and a public repository that nobody can safely reuse
  is not really open source yet.
* It is wholly within the team's control, which is not true of everything here.

Three further candidates, in the order we would take them:

1. **Run `eslint` in CI.** The configuration already exists; this is adding a
   step that invokes it. Expect a wave of findings on first run — see the
   guidebook's note on landing a linter on an existing codebase without a
   hundred-file diff.
2. **Require one approving review on `main`.** A settings change, but not a free
   one: on a small team it can mean waiting on a single reviewer. §7 covers the
   trade-off honestly.
3. **Wire `pytest-cov` into the CI run.** The dependency is already there. Treat
   the resulting number as a map of what is untested, not as a target.

**Nothing on this list is urgent, and nothing here is a criticism.** Every item
is a future — something that costs little now and buys the ability to change
this codebase safely later. None of them is a feature, and none of them will be
visible to a user. That is exactly why they are easy to postpone indefinitely,
and why it is worth naming them.

---

*Generated by [Code Futures](https://github.com/oss-slu/code-futures), schema
`code-futures/v1`. Companion guidebook:
[`guidebook_code_futures`](https://github.com/oss-slu/guidebook_code_futures).
This report describes a repository, not the people working in it, and it is not
an input to any evaluation.*
