# Prototype · Juno

> Module 2 · Prototype the Idea Worth Building. The working build that tests your idea, made with your build tool of choice.

## Prototype link

_The public share URL from your build tool. No share URL? A screenshot of the working flow is fine, the prompt is what is assessed._

https://p3-center.tail7806f2.ts.net:8443
Password = ProductSchool
Username = ProductSchool
_____

## What it demonstrates

_The one flow this prototype proves._

The one flow this prototype proves is the daily Signal Digest: Juno reads every new support ticket, Jira update and Slack message since the last digest, groups them into themes with counts, affected customers and sources, ranks them on evidence, and posts a digest a PM can read in under three minutes, while handing every decision to a named human instead of making it.
_____

## Debrief

- **What worked:** _____

- **The digest itself.** On a synthetic RocketShip dataset (56 tickets, 15 Jira issues, 23 Slack messages), Juno posted a digest that passed every format check: all six sections, under 300 words, correct headline numbers, and no invented ticket ids.
- **Finding the problem nobody was tracking.** The dataset hides a cluster of EU customers reporting stuck runs with no Jira issue. Juno surfaced it as a top theme in every digest run.
- **The line between what Juno does and what a human decides.** Reads run on Juno's own authority. The digest and on-call alerts post immediately. Everything else waits in an approval queue, and Juno has no tool at all for changing a ticket. Asked to promise a customer a ship date, every model tested refused.
- **Enforcing rules in the tools, not only in the prompt.** The posting tool rejects a digest that is too long or missing a section and tells Juno what to fix. This turned a rule the models often broke into one they always met.
- **The skill file as the single source of behaviour.** Juno's role, constraints and digest format live in one readable file that is used verbatim as the core of the system prompt.
- **Choosing the model by measurement.** A benchmark screened 28 local models and scored the finalists on Juno's real tasks. It cut answer time from four to five minutes to about 20 to 30 seconds for a question, and it exposed the trade-off between speed and depth.
- **Running without a paid API.** The prototype runs on a self-hosted inference server, is deployed in a container behind a login, and streams its progress so the wait is visible.


- **What broke / felt like a toy:** _____

- **The data is synthetic and too clean.** Slack, Jira, Notion and the support queue are a generated file, not live connections. The tickets come from templates, so the wording repeats and grouping them is far easier than it would be with real customers.
- **Speed and depth pulled against each other.** The first model took four to five minutes per answer. The fast replacements answer in seconds but stop after one or two searches, and none connected the EU queue problem to its likely cause. Only the slow model did.
- **Correct format, shaky content.** Digests passed every format check while still filing a missed run under late alerts and failing to tie late-alert tickets to the Jira issue that already explains them.
- **Rule-following depended on the model setting.** With reasoning off, Juno skipped the on-call alert for a data-loss bug and named the wrong person as the decision owner. With it on, the digest took five minutes instead of 40 seconds.
- **My measure of "helping" is mechanical.** The benchmark checks sections, word counts and invented ids, on one run per model. Nothing scores whether the themes are right, and no PM has used it.
- **"Daily" is a button.** There is no schedule and no memory of yesterday's digest, so "new, growing, steady or shrinking" is inferred from ticket dates and not from a real comparison.
- **It is a single-user demo.** State lives in memory and resets on restart, everyone shares one login, and the audit log cannot say who approved what.
- **Operational slips along the way.** A background benchmark slowed the app while it was in use, and a changed password locked the login. Both were small, but they show how thin the setup is.


- **What I'd change next pass:** _____

- **Score the content against the answer key.** Each synthetic ticket already carries a hidden label for its true cause. I would measure whether Juno's themes match those labels, with several runs per model, before trusting any digest.
- **Connect one real source, read-only.** Start with the Jira or support queue of a real team, to learn how the grouping holds up on messy data.
- **Put it in front of a PM for a week.** Track time to read the digest, how many "needs a human" flags were correct, and how often the PM edited or ignored a theme. That answers "is it helping" better than any benchmark.
- **Move more rules from the prompt into code.** Enforcing the digest format in the tool worked. I would do the same for the data-loss alert and for owner lookup, so they no longer depend on the model's mood.
- **Use two speeds on purpose.** A fast model for questions, and a slower, deeper one for the scheduled digest, where nobody is waiting.
- **Make it genuinely daily.** Schedule the run and store each digest, so trends compare against yesterday.
- **Add per-person sign-in and saved state.** Approvals then carry a name, and a restart no longer wipes the queue and the audit log.
- **Settle the open rule in the skill file.** Decide whether a known, in-progress data-loss bug should alert on-call every day, and write the answer down.

  
