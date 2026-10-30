# **Code Futures: Reading Automated Analysis of Your Codebase**

This guidebook is about what automated analysis of a codebase can and cannot see. It uses the Code Futures report as its worked example, because that is the report you are most likely to be holding — but the thing worth learning is the judgement, not the format. Tools change. The question of what a tool's output is evidence *of* does not.

**Who This Is For:**

* Tech Leads who have received a Code Futures report and want to act on it
* Developers trying to understand what a green check actually proves
* Mentors and advisors reading reports alongside a team
* Faculty running a program like this one at another institution
* Anyone who has looked at a dashboard and wondered how much to believe it

**What You'll Learn:**

* What automated analysis establishes, and what it only appears to establish
* How to read change coupling and hotspots, and what they say about where your boundaries are
* How to read each finding in a Code Futures report, including what it does not mean
* Why `present` and `enforced` are different, and why that gap is the most useful finding you will get
* How to choose one thing to change, and why one
* What making a check blocking costs, and when it is worth paying
* Where this ends and human judgement has to start

**Schema version.** This guidebook describes reports carrying the schema `code-futures/v1`. Your report states its schema in the header. If those two do not match, trust the report and [tell us](https://github.com/oss-slu/guidebook_code_futures/issues) — a mismatch is our problem to fix, not a puzzle for you to solve.

---

## **1. Introduction**

### **What the report is**

A Code Futures report asks one question about one repository: **which engineering practices are in place?** Does something lint this code. Does anything test it. Is review required before a change merges. Is there a licence somebody outside could rely on.

It asks the same way every time, which is why it is worth having. One person cannot read twenty-nine codebases. A tool puts the same questions to all of them in a few minutes, and that consistency is what makes the results comparable.

### **What the report is not**

This matters more than what it is, because every failure mode of a document like this comes from somebody reading it as something it is not.

* **Grades.** The report carries no score, no total, and no ranking, and feeds no evaluation. Adding up outcomes misreads it.
* **A verdict on the code.** It records which practices exist. A well-designed system with no linter and a mess with a linter will read as you would expect, and the report will be wrong about which you would rather maintain.
* **A verdict on the team.** Repositories are inherited, and much of what a report finds predates whoever reads it.
* **The technical assessment rubric.** That is a separate instrument and the one that assesses a codebase; this report produces evidence to bring to it. See §8.
* **A complete picture.** Section 2 lists what it cannot see.

### **A note on the name**

"Code Futures" comes from Kent Beck's distinction, in *Tidy First?*, between the two kinds of value in software. **Features** are what the software does now. **Futures** are what remains possible later. Behaviour buys features; structure buys futures.

That is a precise claim about what this report measures, and it is worth sitting with. **Not one probe in this report is a feature.** A test suite does nothing for a user. Neither does a linter, a branch protection rule, or a licence file. Every one of them is a purchase of optionality — a thing that costs a little now and determines what you can still change next semester without breaking something you cannot see.

This is also why these items are so easy to postpone forever. Nothing bad happens the week you skip them. You pay later. It shows up as a general feeling that the codebase has become difficult, by which point no one can trace it to a decision.

Naming them *futures* is an attempt to make that visible. Work on structure is not overhead. It is buying back options you would otherwise lose.

---

## **2. What Automated Analysis Can and Cannot See**

This is the heart of the guidebook, and it comes before the section explaining the findings. Read the other way round, the findings look more authoritative than they are.

### **Why tools are persuasive**

Automated analysis is convincing in a specific and slightly dangerous way: it produces **confident output in consistent units**. A letter grade. A percentage. A green check. A count of issues.

Confident output in consistent units reads as objective whether or not it is. But:

* A maintainability grade is a vendor's model rendered as a letter. Another vendor's model gives a different letter for the same code.
* A code smell is a rule somebody wrote down. Reasonable engineers disagree about many of them.
* A coverage percentage counts lines executed. It cannot tell whether a single assertion ran.
* A green check means only this: **the checks that ran, passed.** It says nothing whatever about the checks that do not exist.

None of this makes the tools useless. It makes them *instruments*, and an instrument is only as good as your understanding of what it is measuring.

**A measurement is not a score.** This distinction does most of the work in the rest of the guidebook, so it is worth stating plainly. "62% of lines were executed by the test suite" is data: it has units, a method, and a population. "Your testing is a C" is a judgement, arrived at by collapsing data against a threshold somebody chose. The first is something you can reason about and argue with. The second has already done your thinking for you, usually badly, and the reasoning is not shown.

Your report carries plenty of numbers. None of them is collapsed into a verdict, and the reason you are given the measurement rather than the grade is so that you can do the collapsing yourself, knowingly, in the context only you have.

Two numbers are withheld, and the reasons are different from each other. We do not carry the OpenSSF Scorecard's 0–10 aggregate because it is **calibrated for a different population** — widely-depended-upon libraries, scored partly on fuzzing and signed releases. Running it on a one-semester project does not give you a weak signal; it gives you a measurement of something else wearing a familiar label. And we do not carry a maintainability letter grade, because a letter is precisely the collapse this section is about.

### **What it genuinely does well**

**Consistency at a scale no person can match.** Twenty-nine repositories, the same questions every time, no fatigue and no favourites. A human reviewer on repository nineteen is not the same reviewer who looked at repository two.

**Finding what nobody has time to look for.** Nobody is going to check whether every repository has a pull request template. A tool checks in milliseconds and never finds it boring.

**Precision about specific, checkable facts.** "A linter is configured here and no CI step invokes it" is exact, actionable, and almost impossible to spot by reading code.

**Change over time is real.** Because the instrument does not drift, the difference between this month's report and last month's is a fact about the repository rather than a fact about who was reading it.

### **What it cannot see**

This list is the one to remember.

* **Whether a test asserts anything.** A test that runs the code and checks nothing passes, counts toward coverage, and proves nothing. No tool in this report can distinguish it from a real test.
* **Whether the domain model is right.** The most expensive mistakes in software are conceptual — the wrong abstraction, a boundary in the wrong place, a model that does not match the problem. Every one of them is invisible here.
* **Whether the code is readable by the people who have to read it.** Complexity metrics approximate this badly.
* **Whether a practice is real or ceremonial.** A repository can have code review required, and have every pull request approved in nine seconds without being read. The tool sees a healthy signal.
* **Context.** A prototype and a system with users should be held to different standards. The tool holds them to the same one.
* **Why.** It can show you a file that changes constantly and has no tests. It cannot tell you whether that is a design problem, a requirements problem, or the one file in the system that genuinely should change constantly.

### **The precise form of the limit**

The failure mode is not that tools lie. It is subtler, and worth stating as a rule:

> **A tool's finding is evidence that a practice is present. A tool's silence is
> not evidence that a practice is absent.**

If the report says a linter is configured, a linter is configured. If it says none is, that means none was *found where we looked*. A project with its own check script in a Makefile, invoked by a convention everyone on the team knows, has the practice and will be reported as not having it.

So a finding you believe is wrong is worth saying so about. It is more likely to be a gap in the detection than a thing you forgot you did.

### **The best illustration of the whole problem**

There is a category in static analysis called a **Security Hotspot**, and it is the clearest demonstration of this guidebook's thesis in any tool we use.

A security hotspot is not a vulnerability. It is *security-sensitive code that a human must look at*. The tool is explicitly declining to decide. It has found the place where the question lives and handed the question to you.

That is what good automated analysis looks like. Compare it with a tool that reports a number: the hotspot is more useful precisely because it is less confident. Automation got you to the question. The answer is still yours, and no version of the tool is going to take that over.

---

## **3. Reading Your Report**

A worked example report accompanies this guidebook as [`worked-example-report.md`](worked-example-report.md). It is **fabricated** — no such repository exists — and it is composite, so that every kind of outcome appears somewhere. Read it alongside this section.

### **The four outcomes**

Every finding in the report is one of four values. This is a vocabulary, not a scale: there is no arithmetic you can do on it.

| Outcome | Means | Who it is about |
| ----- | ----- | ----- |
| **enforced** | In place, *and* something in CI runs it | the repository |
| **present** | In the repository, but nothing runs it automatically | the repository |
| **absent** | We looked, and it is not there | the repository |
| **unreadable** | We could not find out | **us** |

Two of these need argument.

**`present` is not a worse `enforced`.** It is a different state, and usually a much more useful one. A repository with a complete test suite that no workflow invokes is one file away from having continuous integration. A repository with no test suite is weeks away. Collapsing those two into "doesn't have CI testing" destroys the most actionable finding this report produces — which is why the outcome is not a boolean.

**`unreadable` describes the tooling, not you.** A check may time out, a token may lack a permission, an API may return something unexpected. The report says so rather than quietly recording an absence. This is a matter of trust rather than courtesy. Report our failures as your deficiencies, and you lose any way to tell a real finding from a broken probe.

Not every probe can produce every outcome. A README cannot be **enforced**, because there is nothing for CI to run. The top rung available to a probe depends on the kind of question it is.

### **Why there are no ratings, stars, or colours**

You will notice the report uses plain words rather than ✅ / ⚠️ / 🔴 marks. That is deliberate, and it is not a stylistic preference.

Those three marks mean something specific in this program: they are the levels of the *Codebase Technical Assessment Rubric* — Established, Emerging, Needs Attention. That rubric is a **human** instrument, applied by a person exercising judgement over seventeen dimensions. Borrowing its visual language for automated output would invite you to read a probe outcome as a rubric rating. The two make different claims: one says "a file exists", the other says "a person assessed this and formed a view".

### **Section 1 — Structure**

| Probe | Asks |
| ----- | ----- |
| `structure.hotspots` | Where do complexity and change frequency concentrate? |
| `structure.change_coupling` | Which files keep changing together? |
| `structure.untested_modules` | Which frequently-changed modules have no tests? |
| `structure.duplication` | How much logic is duplicated? |
| `structure.complexity` | Which files carry the most cognitive complexity? |

**This is the only section about your code.** Every other section describes the process around it — whether a linter runs, whether review is required, whether a licence exists. Useful, and all of it could be perfect in a project that is painful to change. This section is the one that looks at the shape of the thing you are building.

#### **Change coupling, and why it beats counting imports**

Two files that keep changing together are coupled, whether or not either imports the other.

That is Kent Beck's definition made measurable. Coupling is *the probability that changing one element forces you to change another*, and your git history is a record of how often that actually happened. A dependency graph tells you what the code says it depends on; co-change tells you what it really depends on. They are frequently different, and where they differ the history is right.

A static analyser cannot see this at all. Two files in different languages, in different directories, with no import between them, that change together in four commits out of five, are as coupled as anything in your system.

Not all coupling is a problem, and the report does not pretend otherwise:

* A test file changing with the module it tests is coupling working as intended
* A model and its migration changing together is structural and unavoidable
* A route handler changing every time a database model changes is a leak — the boundary between those layers is not holding
* Two unrelated features changing together usually means a shared utility that has become a dumping ground

**The question to ask of each pair is whether you would choose that relationship.** Where the answer is no, you have found a boundary in the wrong place, and the report has told you where to put the refactor.

#### **Hotspots, and the thing to do about one**

A hotspot is complexity multiplied by change frequency. Both halves matter. Complicated code nobody touches costs you nothing until you touch it, and simple code that changes daily is fine. The expensive intersection is code that is both hard to understand and constantly being changed, because every change there is a chance to get it wrong.

Complexity is approximated by indentation depth rather than by parsing each language. That is an approximation and you should treat it as one — it is a direction to look, not a measurement of quality. It does work identically across every language, which is what makes hotspots comparable inside a polyglot repository.

**A hotspot with no tests is the combination worth acting on first.** The code most likely to break, with the least protection while you change it. If your report shows a file in both `structure.hotspots` and `structure.untested_modules`, start there.

#### **What this section cannot tell you**

* **Whether a hotspot should be complex.** Some problems are genuinely hard, and a parser or a scheduler may be the most complicated file in the project for good reason.
* **Why two files are coupled.** It gives you the pair, not the cause.
* **Whether your architecture is right.** Change coupling finds boundaries that leak. It has no opinion about whether you chose the right boundaries in the first place, and that question needs a person — see §8.
* **Anything useful on a very short history.** Hotspot ranking on a project with a few dozen commits mostly reports that recently-written files were recently written. Your report says so when that applies. Change coupling holds up better: which files change together is visible after one semester.

### **Section 2 — Open Source Baseline**

Whether the repository carries the documents an outside contributor needs.

| Probe | Asks |
| ----- | ----- |
| `practice.readme` | Is there a README? |
| `practice.contributing` | Is there a contributing guide? |
| `practice.code_of_conduct` | Is there a code of conduct? |
| `practice.license` | Is there a licence, and is it an identifiable one? |
| `practice.issue_template` | Is there an issue template? |
| `practice.pull_request_template` | Is there a pull request template? |
| `practice.security_policy` | Is there a `SECURITY.md`? |

**What it does not mean.** A present README is not a good README. This is pure presence detection: the tool opens no file and reads no prose. A repository can score perfectly here and have documentation that helps nobody.

**The licence probe has an extra state worth understanding.** A licence file that exists but cannot be matched against any OSI-approved licence reads as **present**, with the detail saying so. That is a genuinely different finding from having no licence. Somebody decided this project should be licensed; what is missing is the part that makes the decision legible to a machine, a contributor, or a lawyer. The usual cause is a licence that was retyped or truncated. It is ten minutes to fix and it is worth fixing, because a public repository nobody can safely reuse is not yet open source in the way that counts.

### **Section 3 — Linting and Formatting**

| Probe | Asks |
| ----- | ----- |
| `lint` | Is a linter configured, and does CI invoke it? |
| `format` | Is a formatter configured, and does CI check formatting? |

**Detection works in two layers, and the gap between them is the finding.** Manifests and config files establish what is *configured*: `[tool.ruff]`, `.eslintrc`, `checkstyle.xml`, `Cargo.toml`. Workflow files establish what is *enforced*, meaning a step that actually invokes the tool. **present** means we found the first layer and not the second.

**Polyglot repositories get a row per stack.** A project that is Python and React has both a `ruff` row and an `eslint` row, and they can disagree. Several projects in this program are in exactly that position, and a single combined answer would hide which half needs attention.

**What it does not mean.** An enforced linter says a linter ran. It says nothing about which rules are switched on. A configuration that disables every rule it finds inconvenient is **enforced** and is doing very little. The tool cannot tell the difference, and this is one of the easier places to be accidentally dishonest with yourself.

### **Section 4 — Testing**

| Probe | Detected by |
| ----- | ----- |
| `test.unit` | A runner configured, and invoked in CI |
| `test.integration` | A separate job, or a marked suite |
| `test.e2e` | Playwright, Cypress, or Selenium |
| `test.coverage` | A coverage tool wired into CI |
| `test.mutation` | Stryker, mutmut, PIT, or cargo-mutants |

**Coverage percentages are omitted.** Coverage counts lines executed rather than behaviour verified, so a module at 80% whose tests assert nothing is at 0% in every sense that matters. A number also invites a target, and a target invites tests written to move it. Naming which *kinds* of test run shows you which kind you lack, and that is actionable.

**What it does not mean.** This is the weakest inference in the whole report, so be careful with it. **enforced** here means a test command ran in CI and exited zero. It does not mean the tests are good, that they cover anything important, or that they would fail if the code broke. A test suite can rot into a set of assertions about things that no longer matter and keep passing indefinitely.

**Absent is frequently correct.** Four of five kinds absent is an ordinary reading for a young project, and often the right one. End-to-end tests on a service with no user-facing flows are scaffolding for something that is going to be rewritten. This section reports which kinds run, not which kinds should.

### **Section 5 — Review and Integration**

| Probe | Asks |
| ----- | ----- |
| `review.required` | Does the default branch require an approving review? |
| `ci.present` | Does any CI workflow run at all? |

**Protection can exist and gate nothing**, and this is the trap in the section. A branch can be protected with zero required approving reviews, which shows a reassuring green state on the settings page while permitting unreviewed merges. Protection that exempts administrators is similar: the rules describe an intention rather than a constraint. Both read as **present** rather than **enforced**, and the detail line says which case you are in.

**What it does not mean.** Required review proves a review was requested, not that a review happened. An approval nine seconds after a pull request opens counts identically to a careful read. No tool in this report distinguishes them, and only the team knows which is happening.

### **Section 6 — Security and Dependencies**

| Probe | Asks |
| ----- | ----- |
| `security.sast` | Does static application security testing run? |
| `security.token_permissions` | Do workflows declare minimal permissions? |
| `security.workflow_safety` | Are there dangerous workflow patterns? |
| `deps.update_tool` | Is there a dependency update tool, and is it working? |

**Two different kinds of security tool appear in this program, and the difference is worth knowing** because it is a good lesson in what automation is for:

| Tool | The lesson |
| ----- | ----- |
| CodeQL | Some vulnerabilities are provable by machine, and deeper than a person would find by reading. Dataflow analysis can trace an injection path from a request parameter through three function calls into a query |
| Security Hotspots | Most security work is judgement under uncertainty, and a good tool says so rather than guessing |

**What it does not mean.** A clean SAST run means the analyser found nothing it knows how to find. The class of vulnerability it has no rule for is invisible, and "no findings" is not "no vulnerabilities" in any tool ever built.

### **Reading the composite checks carefully**

Some findings in this section come from the OpenSSF Scorecard, and we use **selected checks only, never its composite score**. The 0–10 aggregate is calibrated for widely-depended-upon open source libraries, and includes checks — fuzzing, signed releases, packaging — that score zero for essentially every student and research project. The aggregate would therefore put every team in this program in the low single digits, which is both demoralising and simply not true as a statement about their code.

This is a general lesson rather than a quirk of one tool. **A composite score encodes a population.** When your project is not in the population the score was designed for, the number is not a weaker signal; it is a different measurement wearing a familiar label.

One check is excluded for a related reason: `Maintained` scores activity in the last ninety days. It is meaningful mid-semester and collapses for every repository over a winter break, which would make a seasonal artefact look like a finding.

Six of the Scorecard checks are not used at all, and the reasons are worth knowing because they are the same reasons you should be careful with any tool's output. Two report the *presence of a defect* rather than the presence of a practice, so reading them as "is this in place" inverts them. One is a proportion of recent commits rather than a state. Three ask questions we can answer better from the repository directly — the branch gate is one, where Scorecard's score flattens the difference between protection that requires a review and protection that requires zero.

### **Section 7 — Code Quality Analysis**

| Probe | Asks |
| ----- | ----- |
| `quality.project` | Is the project analysed by SonarQube Cloud? |
| `quality.gate` | Is a quality gate defined, and does CI apply it? |
| `quality.hotspots_reviewed` | Have flagged security hotspots been reviewed? |

**This section carries no grade.** SonarQube Cloud produces a maintainability rating, a reliability rating, and issue counts. None of them appear in your report, because a letter grade is a vendor's model rendered as a letter, and this report describes which practices are in place. What it records is whether the analysis runs and whether anyone has acted on what it flags.

**The hotspot is the part worth your attention**, and it is the clearest illustration of §2's argument in any tool here. A security hotspot is not a vulnerability. It is security-sensitive code that a person must review, and the tool is explicitly declining to decide. Reviewing one means judging whether the context makes it safe and recording that judgement. Most reviews end in "this is fine", which is a result rather than wasted effort: the next person can see it was considered rather than missed.

**What it does not mean.** A passing quality gate means the code met thresholds somebody configured, which are adjustable and frequently left at a default nobody chose. A rising issue count can mean the code got worse, or that a new rule was switched on.

**This analysis is public.** SonarQube Cloud publishes analysis of public repositories on sonarcloud.io, visible to anyone without an account. Your code is already public, so this discloses nothing new, and you are being told in advance rather than finding out later.

---

## **4. Common Patterns and What They Mean**

> **This section is deliberately incomplete, and will be written from real
> reports rather than from guesswork.**
>
> The patterns below are the ones we expect to be common. They are a
> *hypothesis*. Once reports exist across the full set of active repositories,
> this section will be rewritten from what actually turned up — described in
> aggregate ("this appears in roughly a third of projects") and illustrated with
> composite examples, never naming a repository.
>
> We are telling you this rather than quietly shipping a plausible list because
> the difference matters: a guessed pattern list is a description of what the
> authors imagine, and reading it as a description of reality is exactly the
> mistake §2 is about.

### **"Configured But Never Run"**

**Pattern:**

* A linter, formatter, or test runner is configured
* Nothing in any workflow invokes it
* It reports **present**, never **enforced**

**What it means:**

* Somebody set it up, probably early
* The CI step was never added, or was removed when it got noisy
* The check now runs only for whoever remembers to run it

**Impact:**

* Quality depends on individual habit rather than on the project
* New contributors have no way to know the standard exists
* The gap widens silently, because nothing reports it

**Solutions:**

* Add the CI step — usually under ten lines
* Expect a wave of findings on first run; see §5 on landing this without a hundred-file diff
* Keep it advisory at first. Blocking is a separate decision, covered in §7

### **"Green But Untested"**

**Pattern:**

* CI is present and passing
* `test.unit` reports **absent**, or the suite is very small
* The badge is green

**What it means:**

* The pipeline builds, lints, or deploys, and verifies no behaviour
* A green check is being read as a safety net that does not exist

**Impact:**

* False confidence, which is worse than known absence
* Refactoring becomes risky in a way nobody has noticed yet
* The first real regression is found by a user

**Solutions:**

* Write one test for the thing that would hurt most if it broke
* Prefer a test that would have caught your last bug
* Resist starting with a coverage target; start with a risk

### **"The Inherited Codebase"**

**Pattern:**

* Many **absent** findings
* The repository predates everyone currently working in it
* No single person made any of the decisions being reported

**What it means:**

* The report is describing accumulated history, not this team's choices
* Baselines were set before the current standards existed

**Impact:**

* Reads as an indictment when it is a description
* Teams can disengage from a report that looks like a list of their failures

**Solutions:**

* Read the report as a starting position, not a performance
* Compare against this repository's own past, not against other repositories
* Pick the one item that most reduces the risk of your *next* change

---

## **5. From Findings to Change**

### **Pick one thing**

A report with eight absent findings presents eight pieces of work, and the instinct is to plan all eight. Do not.

The argument is not about discipline; it is about completion. A team that picks six items at the start of an iteration finishes none of them, because each one is competing with feature work that has a client attached to it. A team that picks one finishes it, and then picks another. Over a semester the second team does more.

**But:** one does not mean one forever. It means one *in flight*. When it lands, choose the next from a report that has already changed.

### **How to choose**

In rough order of priority:

1. **Anything with consequences outside the team.** A missing or unidentifiable licence stops other people from using your work. That is categorically different from an internal gap.
2. **Anything that makes the next finding cheaper to fix.** Enforcing a linter before a refactor means the refactor starts clean. Order matters more than it looks.
3. **The cheapest real improvement.** A **present**-to-**enforced** change is usually a few lines. Prefer finishing something.
4. **The highest risk.** An untested module that changes every week is where a regression is actually going to come from.

Explicitly *not* on this list: whichever item would most improve how the report looks. There is no score, so there is nothing to improve, and optimising for a document rather than a codebase is the failure mode this whole guidebook exists to prevent.

### **Landing a linter without a hundred-file diff**

The commonest practical obstacle to fixing **"Configured But Never Run"** is that switching the check on produces hundreds of findings across files nobody intended to touch. A pull request that reformats the entire codebase is unreviewable, conflicts with everything in flight, and buries the actual change.

The standard answer is to **separate the formatting commit from the behaviour commit**. This is Kent Beck's argument in *Tidy First?*, applied directly. A structural change and a behavioural change in one commit cost more to review than either costs alone.

Three widely used approaches, and they compose:

* **Bulk-fix once, in its own commit, reviewed as a formatting change.** Record it in `.git-blame-ignore-revs` so `git blame` still finds real authorship.
* **Baseline the existing findings** and enforce only on new code. Most tools support this directly; it trades a clean slate for not blocking anyone.
* **Enforce on changed files only** — run the linter against the pull request's diff. The codebase improves as it is touched.

**But:** the third option means some files are never cleaned, which is fine if you have decided it is fine and a slow surprise if you have not.

### **Say what you decided not to do**

When a team reviews a report and consciously declines an item, that decision is worth writing down — in the repository, near the thing it is about.

An undocumented decision is indistinguishable from an oversight six months later, and the next person will spend an afternoon rediscovering what you already worked out. One sentence in a `CONTRIBUTING.md` or an architecture note is enough: *we do not run end-to-end tests because there is no user-facing flow yet; revisit when the web client lands.*

That sentence also means the next report's **absent** finding reads as a decision rather than a gap.

---

## **6. Continuous Improvement**

### **A report is a snapshot; the useful thing is the sequence**

A single report tells you where you are. Two reports tell you which way you are going, and the second is the one worth having. The probes do not drift, so a difference between two reports is a real difference in the repository rather than a difference in who was looking.

Compare a repository against **its own past**, not against other repositories. Projects differ in age, stack, client, and what they are for; a cross-project comparison mostly measures those differences. A repository against itself measures what the team did.

### **Getting the feedback continuously**

Reports are produced periodically. Feedback is more useful attached to the pull request that caused it. That is what the reusable workflow is for. A team adopts it by adding one file:

```yaml
# .github/workflows/code-futures.yml
name: Code Futures

on:
  pull_request:
  push:
    branches: [main]

permissions:
  contents: read
  security-events: write

jobs:
  code-futures:
    uses: oss-slu/code-futures/.github/workflows/code-futures.yml@v1.0.0
    secrets: inherit
```

**Copy the `permissions:` block exactly.** Omitting it is the most likely reason a first attempt fails. A reusable workflow cannot grant itself more permission than the workflow calling it. So `security-events: write` must be declared here, in your repository. Without it the security scanning job fails, and the error does not obviously point back to this block.

**Nothing in it will fail your build.** Every job is advisory by default. This is deliberate: a team whose build breaks the day they adopt a quality tool learns that quality tools are an obstacle, and they are not wrong to conclude that from the evidence available to them.

**Pin the exact version.** `@v1.0.0` is immutable and will never change under you. We avoid the common convention of pinning a moving major tag like `@v1`, because a moving tag silently changes the CI of every repository using it, leaving nobody able to say what ran last Tuesday. The cost is that fixes require you to bump the tag; you will be told when there is one.

### **A cadence that works**

**Each iteration, briefly.** Look at the diff between reports, not the whole report. It takes two minutes and it is the only part most iterations need.

**At the retrospective, properly.** A report is a good retrospective input precisely because it is about the repository rather than about anybody's performance, which makes it unusually safe to discuss. Two questions are enough: what changed since last time, and what is the one thing for the next iteration?

**When onboarding.** A new contributor reading the report learns the project's actual conventions faster than from documentation, because the report describes what is true rather than what was intended.

### **When to stop**

**There is no state in which every probe reads `enforced`, and chasing one is a mistake.** Several of these practices are not appropriate for every project. Mutation testing on a six-week prototype is effort taken from something that matters more. A code of conduct on an internal tool with three contributors is a file nobody reads.

Reasonable places to stop:

* The practices that protect your ability to change the code safely are in place
* The remaining absences are decisions you can state out loud, and have
* Nothing on the list is blocking anything anyone is trying to do

**But:** "we decided not to" is only a real answer if somebody actually decided. There is a difference between a considered judgement and a thing nobody got round to, and only the team knows which one is in front of them. The report cannot tell, and will never ask.

---

## **7. Making Checks Blocking**

### **The trade-off is real, and it is not resolved here**

Every Code Futures job is advisory. The guidance below is for a team that has decided to make one binding — and that decision belongs to the team, including the decision not to.

This is a place where the field genuinely disagrees, so it would be wrong to present an answer as settled. "Run a linter in continuous integration" is settled practice. "Block merges on linter failures" is a real trade-off, and which side you land on depends on things this guidebook does not know about your project.

| Blocking buys you | Blocking costs you |
| ----- | ----- |
| The standard is actually maintained, not just documented | A failing check stops work that may be unrelated to it |
| No "we'll fix it later" backlog, because later is now | Pressure to disable the check under deadline, which is worse than never having it |
| Contributors learn the standard from the tool, not from review comments | Flaky checks destroy trust in all checks, quickly |
| Review conversations are about design rather than style | Small teams can be blocked by one unavailable reviewer |

A team shipping its first feature for a client has different constraints from a team maintaining a system with external users, and the right answer differs.

### **A reasonable order, if you are going to do it**

1. Run it advisory for a few weeks and see how noisy it actually is.
2. Fix or baseline the existing findings, so day one of blocking is not a wall.
3. Make **formatting** blocking first. It is deterministic, auto-fixable, and almost never wrong — the gentlest possible introduction.
4. Then **linting**, once the rule set has stopped surprising people.
5. Then **tests**, which is the highest-value and the most disruptive, because this is where flakiness lives.
6. Leave security scanning advisory longer than feels comfortable. Its false positive rate is the highest of anything here.

### **How, mechanically**

Two steps, and the second is the one people forget.

**Remove the `continue-on-error` line** from the job you want to be binding:

```yaml
jobs:
  lint:
    continue-on-error: true   # ← delete this line to make lint blocking
```

**Then add the job to your branch protection's required status checks.** Skip this second step and the job reports a failure while the pull request merges anyway. That is the worst of both worlds: the friction of a red check, none of the protection.

### **Protection that does not protect**

While you are in that settings page, two configurations are worth checking, because both look protected and gate nothing:

* **Zero required approving reviews.** The branch is protected and any pull request can merge unreviewed.
* **Administrators exempt.** Whoever has admin on the repository bypasses every rule above. On a small team that is often most of the team, and the rules then describe an intention rather than a constraint.

**But:** exempting administrators is sometimes the right call — someone has to be able to fix a broken `main` at 2am. The point is to choose it rather than inherit it, and to know which one you have.

---

## **8. Where to Go Deeper**

### **The human instrument**

This report is one half of a pair, and it is the shallow half.

The **Codebase Technical Assessment Guidebook and Rubric** ([`oss-slu/guidebook_tech_debt`](https://github.com/oss-slu/guidebook_tech_debt)) is a person reading a codebase and exercising judgement across seventeen dimensions. It covers investigation technique, note-taking, prioritisation, and how to communicate findings to a team without putting them on the defensive.

**The rubric remains the assessment instrument.** Nothing in a Code Futures report replaces it or competes with it. What this report does is give a person starting an assessment a set of established facts, so the hours of human attention go to the questions only a person can answer.

The two cover each other's blind spots, and the division is clean:

| Automated analysis | Human assessment |
| ----- | ----- |
| Finds every repository without branch protection in under a minute | Finds that the domain model is wrong |
| Same yardstick across every project, no fatigue | Adapts the question to the project |
| Cannot tell whether a test asserts anything | Reads the test |
| Says where risk concentrates | Says why, and what to do about it |
| Cheap enough to run weekly | Worth a half-day, a few times a year |

A sensible sequence: run the automated report first, read it to find where to look, then use the rubric where the report says something interesting or says nothing at all.

### **On the specific tools**

* [OpenSSF Scorecard](https://github.com/ossf/scorecard) — the check definitions are worth reading, particularly the documentation of what each check actually examines, which is narrower than the check names suggest
* [CodeQL](https://codeql.github.com/docs/) — semantic analysis and dataflow, and the best available explanation of why some vulnerabilities are provable and others are not
* [OWASP Top 10](https://owasp.org/www-project-top-ten/) — vocabulary for the categories the security tools report in

### **On the ideas**

* Kent Beck, *Tidy First?* — features versus futures, and the argument for separating structural changes from behavioural ones. This guidebook's title and its central frame both come from here
* Michael Feathers, *Working Effectively with Legacy Code* — the source of "legacy code is simply code without tests", and the practical answer to an inherited codebase with no safety net
* Nicole Forsgren, Jez Humble and Gene Kim, *Accelerate* — the empirical case that these practices correlate with delivery performance, which is the evidence behind treating any of this as more than preference
* Martin Fowler, *Refactoring* — the catalogue of named code smells that most static analysis rules are ultimately derived from

---

## **Acknowledgements**

This guidebook was developed for the Opening Skills and Solutions program at Saint Louis University, and written with the assistance of Claude (Anthropic AI).

It is a companion to the *Codebase Technical Assessment Guidebook and Rubric*, and borrows its structure and conventions from it.

**Feedback is wanted**, particularly on anything that assumes knowledge you do not have. That is the failure mode hardest for the authors to see, and the one most worth hearing about. Open an issue on [the repository](https://github.com/oss-slu/guidebook_code_futures/issues).
