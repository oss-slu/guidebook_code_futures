# Code Futures: Reading Automated Analysis of Your Codebase

A guide to what automated analysis of a codebase can and cannot see. Written to accompany the [Code Futures](https://github.com/oss-slu/code-futures) report, but the subject is the judgement rather than the format: tools change, and the question of what a tool's output is evidence *of* does not.

Developed for use in [Open Source with SLU](https://oss-slu.github.io) experiential learning programs, and applicable to any team trying to work out how much to believe a dashboard.

## Contents

- **[Guidebook](guidebook.md)** — What automated analysis establishes and what it only appears to establish; how to read each finding; how to choose one thing to change; what making a check blocking costs
- **[Worked example report](worked-example-report.md)** — A **fabricated** Code Futures report, for a repository that does not exist. Composite by design, so every kind of outcome appears somewhere

## Who This Is For

- Tech Leads who have received a Code Futures report and want to act on it
- Developers trying to understand what a green check actually proves
- Mentors and advisors reading reports alongside a team
- Faculty running a program like this one at another institution
- Anyone who has looked at a dashboard and wondered how much to believe it

## Usage

Read §2, *What Automated Analysis Can and Cannot See*, before §3, *Reading Your Report*. The order is deliberate — read the other way round, the findings look more authoritative than they are.

Keep the worked example open alongside §3. It is the report §3 describes.

## Relationship to the technical assessment rubric

This guidebook is the companion to the [Codebase Technical Assessment Guidebook and Rubric](https://github.com/oss-slu/guidebook_tech_debt), not a replacement for it or a competitor to it.

The rubric is a **human** instrument: a person reading a codebase and exercising judgement across seventeen dimensions. This guidebook is about **automated** signal — broad, cheap, repeatable, and blind to everything requiring judgement. Automation finds every repository without branch protection in under a minute and will never tell you the domain model is wrong.

The rubric remains the assessment instrument. A Code Futures report produces evidence to bring to it.

## Schema version

This guidebook describes reports carrying the schema `code-futures/v1`. Every report states its schema in the header. If the two disagree, trust the report and open an issue — a mismatch is a bug on our side.

## Contributing

Corrections and suggestions are welcome by pull request.

Feedback on **anything that assumes knowledge you do not have** is particularly wanted. That is the failure mode hardest for the authors to see and the one most worth hearing about, and it does not need to arrive as a pull request — an issue saying "I did not understand this paragraph" is a useful contribution.

---

[![License: CC BY-NC-SA 4.0](https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

This work © 2026 by [Open Source with SLU](https://oss-slu.github.io), the open source program office of Saint Louis University, is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/).
