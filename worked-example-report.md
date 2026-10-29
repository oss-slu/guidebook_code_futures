# **Code Futures Report: `oss-slu/tidewater-atlas`**

> ### **Fabricated example**
>
> `tidewater-atlas` is not a real repository. No project by that name exists in `oss-slu` or anywhere else, and nothing here describes any actual team's codebase. It is composite: it carries a mix of findings chosen so that every kind of outcome appears somewhere, which no single real project would be likely to show at once.
>
> Your report will have this shape. Only the findings differ.

|                    |                                                        |
| ------------------ | ------------------------------------------------------ |
| **Repository**     | `oss-slu/tidewater-atlas`                              |
| **Commit**         | `a0408fc`                                              |
| **Collected**      | 2026-11-01 14:00 UTC                                   |
| **Schema**         | `code-futures/v1`                                      |
| **Languages**      | Python, JavaScript                                     |
| **Classification** | Internal — shareable within the program, not published |

---

## **How to read this**

This report describes which engineering practices are in place in one repository. It records practice. Judging the code is a separate job, done by people, and nothing here feeds an evaluation.

Each finding carries one of four outcomes:

* **enforced** — in place, and CI runs it
* **present** — in the repository, but nothing runs it automatically
* **absent** — we looked, and it is not there
* **unreadable** — we could not find out. This describes our tooling, not your repository

The companion guidebook explains each probe and the limits of what it shows. Read §3 before acting on anything below.

---

## **Outcomes**

| Outcome    | Count |
| ---------- | ----- |
| enforced   | 6     |
| present    | 7     |
| absent     | 8     |
| unreadable | 1     |

### **No score**

Twenty-two findings from twenty probes, because `lint` and `format` are answered once per language.

Use these counts as an index to find your place. Each probe reports on one practice, and the practices a project needs depend on what it is for. Two repositories with different totals may simply be doing different work.

---

## **1. Open Source Baseline**

Whether the repository carries what an outside contributor needs. Someone who has never met you should be able to understand the project and open a pull request without asking a person first.

* **`practice.readme`** — **present**. `README.md`
* **`practice.contributing`** — **present**. `CONTRIBUTING.md`
* **`practice.code_of_conduct`** — **absent**. Searched the root, `docs/`, and `.github/`
* **`practice.license`** — **present**. `LICENSE` exists; its text matches no OSI-approved licence we recognise, so no SPDX identifier was assigned
* **`practice.issue_template`** — **present**. `.github/ISSUE_TEMPLATE/bug_report.md`
* **`practice.pull_request_template`** — **absent**. Searched the root, `docs/`, and `.github/`
* **`practice.security_policy`** — **absent**. No `SECURITY.md`

### **Unidentifiable licence**

An unrecognisable licence file is a different finding from a missing one. Someone decided this project should be licensed; what is missing is the part that makes the decision legible to a machine, a contributor, or a lawyer. A retyped or truncated licence is the usual cause. Replacing it with verbatim text from [choosealicense.com](https://choosealicense.com) takes ten minutes.

---

## **2. Linting and Formatting**

Whether a tool checks style and common errors, and whether anything makes that check run.

| Probe    | Language   | Outcome      | Evidence                                          |
| -------- | ---------- | ------------ | ------------------------------------------------- |
| `lint`   | Python     | **enforced** | `ruff` in `pyproject.toml`, run at `ci.yml:31`    |
| `lint`   | JavaScript | **present**  | `eslint` in `frontend/.eslintrc.json`, no CI step |
| `format` | Python     | **enforced** | `ruff format --check` at `ci.yml:35`              |
| `format` | JavaScript | **absent**   | No formatter configured                           |

### **Two languages, two answers**

This repository is Python and JavaScript, so each language gets its own row. They disagree. Python is covered. JavaScript has a linter that someone configured and nothing runs, so it catches problems only for whoever remembers to invoke it.

Closing that gap costs a few lines of YAML. That makes it the cheapest real improvement available here. The guidebook calls the pattern **"Configured But Never Run"**, and §5 works through it.

---

## **3. Testing**

Which kinds of testing this project runs.

| Probe              | Outcome      | Evidence                                         |
| ------------------ | ------------ | ------------------------------------------------ |
| `test.unit`        | **enforced** | `pytest` in `pyproject.toml`, run at `ci.yml:38` |
| `test.integration` | **absent**   | No separate job, no marked suite                 |
| `test.e2e`         | **absent**   | No Playwright, Cypress, or Selenium              |
| `test.coverage`    | **present**  | `pytest-cov` declared, no CI step                |
| `test.mutation`    | **absent**   | No mutation testing tool                         |

### **Kinds, not percentages**

Coverage counts lines executed. Tests that assert nothing still count toward it. Naming which kinds run shows you which kind you lack, and that is something you can act on.

### **Absence is often correct**

Four of five kinds are absent here, which may well be right for a project at this stage. End-to-end tests on a service with no user-facing flows would be scaffolding for something due to be rewritten. This section reports which kinds run, not which kinds should.

---

## **4. Review and Integration**

Whether code is reviewed before merging, and whether anything runs against it.

* **`review.required`** — **present**. `main` is protected. Required approving reviews: 0. Status checks: not required. Administrators: exempt
* **`ci.present`** — **enforced**. Three workflows: `CI`, `CodeQL`, `Dependabot Updates`

### **Protection that permits unreviewed merges**

Branch protection here requires zero approving reviews. The settings page shows a protected branch, and any pull request can still merge unread. Exempting administrators compounds this. On a small team that is most of the team, so the rules describe an intention.

Both are settings changes rather than work. §7 covers what making them binding costs, and there are real costs.

---

## **5. Security and Dependencies**

* **`security.sast`** — **unreadable**. The Scorecard `SAST` check exceeded its timeout. We established nothing here
* **`security.token_permissions`** — **absent**. Workflows declare no top-level `permissions:` block, so jobs run with the repository default
* **`security.workflow_safety`** — **enforced**. No untrusted input reaches a `run:` step; no `pull_request_target` paired with a head-ref checkout
* **`deps.update_tool`** — **enforced**. Dependabot at `.github/dependabot.yml`, opening update pull requests

### **An unreadable probe describes us**

`security.sast` failed on our side, and we report it that way. This repository runs a `CodeQL` workflow, visible in §4, so SAST is probably running. We could not confirm it through this channel on this run. Marking it **absent** would have told the team they were missing something they had already done.

If an outcome in your report reads **unreadable** and you believe it is wrong, tell us. That is a bug in our tooling rather than a dispute about your codebase.

---

## **What we could not read**

| Probe           | Reason                    |
| --------------- | ------------------------- |
| `security.sast` | Scorecard check timed out |

One probe of twenty. A long list here, or a complete one, would mean the sweep failed rather than the repository being empty.

---

## **Where to start**

Pick one item. A team that picks six finishes none, because each competes with feature work that has a client attached; a team that picks one finishes it and picks another. §5 of the guidebook makes the argument properly.

Our suggestion here is **the licence**, for three reasons:

* Ten minutes, no design decisions, no coordination
* The only finding with consequences outside the team — a contributor cannot tell whether they may use this
* Entirely within the team's control, which is not true of everything below

Then, in order:

1. **Run `eslint` in CI.** The configuration exists; this adds a step that invokes it. Expect many findings on first run — the guidebook covers landing a linter without a hundred-file diff.
2. **Require one approving review on `main`.** A settings change, though not a free one: on a small team it can mean waiting for a single reviewer. §7 states the trade-off.
3. **Wire `pytest-cov` into CI.** The dependency is already declared. Read the resulting number as a map of what is untested rather than as a target.

Every item here is a future rather than a feature. None will be visible to a user, which is why they are easy to postpone, and why naming them is worth doing.

---

*Generated by [Code Futures](https://github.com/oss-slu/code-futures), schema `code-futures/v1`. Companion guidebook: [`guidebook_code_futures`](https://github.com/oss-slu/guidebook_code_futures). This report describes a repository rather than the people working in it, and feeds no evaluation.*
