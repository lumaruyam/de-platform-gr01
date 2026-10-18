# Data Engineering Course — All Project Pages (01–10 + Defence)
This documented is the extracts of project pages from the website: https://education.tangoai.fr/courses/data-engineering
---

## Project step 1: start the platform

Across this course your group builds one data platform for Albert's Marketplace, one layer per lesson. At the end you defend it: ten minutes of presentation, five minutes of questions, and each member defends one technical choice without preparing it.

This first step sets up the repository and writes down the decision everything else depends on: what you are building towards.

Groups of three or four. Your teacher creates your repository from the course template and adds you to it.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | The team, in the README | README.md |
| 2 | Albert's Marketplace's data flows | docs/data-flows.md |
| 3 | Your first architecture decision record | docs/adr/0001-target-architecture.md |
| 4 | The target architecture and its diagram | docs/architecture.md |
| 5 | The first star schema sketch | docs/model/star-daily-demand.md |

### 1. The repository

Clone the repository your teacher created for your group. It comes from de-platform-template, so it already has the folders and templates you need.

Fill in the team table in README.md: names and GitHub handles.

Read the working rules in that README. The two that matter from today: every change goes through a pull request reviewed by another member, and the commit history is part of your mark.

### 2. The data flows

Write docs/data-flows.md: one row per flow, with source, data, consumer, frequency and format. This is your group's version of exercise 1, so start from what your pairs produced in class and reconcile the differences.

Add the answers to the three questions under the matrix: which flows carry personal data, which consumers read from a system that serves customers, and where the same figure could be computed twice.

### 3. The architecture decision record

Copy docs/adr/0000-template.md to docs/adr/0001-target-architecture.md and fill it in.

The decision: warehouse, lake, two-tier or lakehouse, for Albert's Marketplace.
The options considered: all four, with what speaks for and against each.
The criteria: at least four from the decision frame (workload, data types, freshness, team and skills, cost, governance), and how the options scored.
The consequences: what becomes easier, what becomes harder, what you will have to watch.

**Write it so a stranger can follow it**

An ADR is read months later by someone who was not in the room. State the facts you had, not the conclusion alone. "We chose a lakehouse because it is modern" is the failure mode.

### 4. The architecture and its diagram

In docs/architecture.md:

- Context: what Albert's Marketplace needs, in three to five sentences.
- The diagram: your target architecture. Mermaid renders on GitHub, and any drawing tool works if you commit the image.
- The layers table: for bronze, silver and gold, what lands there and what it guarantees.

This diagram is a graded deliverable, and you update it at the end of every lesson. By the defence it shows the platform you actually built.

### 5. The star schema sketch

In docs/model/star-daily-demand.md, sketch the table the ML team asked for: units sold per product, per country, per day.

- The business process you are measuring.
- The grain, in one sentence: "one row per …".
- The dimensions.
- The facts.

You build this for real, with tests, in lesson 5. Sketching it now tells you which columns the platform has to carry from the very first ingestion.

### Done when

- [ ] Every member has pushed at least one commit
- [ ] docs/data-flows.md covers every flow in the brief
- [ ] docs/adr/0001-target-architecture.md names options, criteria and consequences
- [ ] docs/architecture.md has the diagram and the layer guarantees
- [ ] docs/model/star-daily-demand.md states the grain in one sentence
- [ ] The work arrived through at least one reviewed pull request

**Deadline: before session 2.** Nothing else in the course starts from scratch: lesson 2 adds the bronze layer to this repository.

Last updated on Oct 13, 2026

---

## Project step 2: the bronze layer

Lesson 1 left your repository holding a decision: what you are building towards, argued in an architecture decision record (ADR) — a short, dated note saying what you decided, what else you considered and what it commits you to — with a diagram. This week you build the bottom layer of it for real, and everything after this reads from what you create now.

In exercise 1 each of you built a bucket under your own prefix. This is the group's one, agreed between you, in the shared part of the project.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | Bronze, as partitioned Parquet | your group's bucket |
| 2 | The layout decision, as an ADR | docs/adr/0002-bronze-layout.md |
| 3 | Data dictionary v1 | docs/data-dictionary.md |
| 4 | Lifecycle and access, as files | infra/lifecycle.json, infra/iam.md |
| 5 | What it costs, per month | docs/costs.md |
| 6 | The architecture diagram, updated | docs/architecture.md |

### 1. Bronze in the bucket

One bucket for the group, in europe-west1, with the export written as Parquet, partitioned. You choose the partition key and you justify it in deliverable 2.

**Promise to keep:** bronze holds what arrived, unchanged. No cleaning, no deduplication, no dropped columns. Lesson 5 is where data gets fixed; if you fix it here you have destroyed the evidence.

### 2. The ADR: why this layout

An ADR is read months later by someone who was not in the room — often by you. Copy docs/adr/0000-template.md to docs/adr/0002-bronze-layout.md.

The decision: partition key, file format, compression codec.
The options considered: at least one alternative partition key, and CSV.
The evidence: your own numbers from exercise 1 — sizes, object counts, query times. An ADR that argues from the lecture rather than from your own measurements is a weaker one.
The consequences: what this layout makes cheap, what it makes awkward, and what would force you to rewrite it.

### 3. The data dictionary

docs/data-dictionary.md: every table you have landed, and for each column its name, type, meaning, and whether it carries personal data.

That last column is the one that matters. It is how you will know, in lesson 3, what must never be ingested, and in lesson 10, what has to be audited. The export contains customer names, addresses, phone numbers — and, in the legacy tables, passwords and card numbers.

Mark them now, while you are looking at them. Nobody ever goes back.

### 4. Lifecycle and access, as files

infra/lifecycle.json — the policy you applied, committed, not just clicked.
infra/iam.md — a table of every binding on the bucket: principal, role, scope, and one line saying why.

Both of these become Terraform in lesson 8. Writing them as files now means that lesson is a translation rather than an archaeology exercise.

### 5. What it costs

docs/costs.md: the monthly cost of what you have built, computed from published prices, with the date you read them. Storage, operations, and a sentence on what would change if the data were a hundred times larger.

You will extend this file every lesson. By lesson 9 it is the input to the FinOps work.

### 6. The diagram

Update docs/architecture.md so the diagram shows what now exists rather than what you planned in lesson 1: the bucket, its region, its layout, and who can read it.

This diagram is graded, and you update it at the end of every lesson. At the defence it should show the platform you actually built.

### Done when

- [ ] The bucket holds partitioned Parquet, and bronze is unmodified
- [ ] docs/adr/0002-bronze-layout.md argues from your own measurements
- [ ] docs/data-dictionary.md marks every personal-data column
- [ ] infra/lifecycle.json and infra/iam.md match what is really applied
- [ ] docs/costs.md names its prices and their date
- [ ] The diagram shows the bucket
- [ ] Every member has pushed at least one commit
- [ ] The work arrived through at least one reviewed pull request

**Check it, do not assume it**

gcloud storage buckets describe and gcloud storage buckets get-iam-policy tell you what is actually configured. A committed file that no longer matches the bucket is worse than no file, because the next person trusts it.

**Deadline: before session 3.** Lesson 3 ingests into this bucket — from batch files, from a stream, and from the database's change log — so the layout you choose this week is the layout three pipelines will write into.

Last updated on Oct 13, 2026

---

## Project step 3: three roads into bronze

Lesson 2 left your repository holding a bucket: partitioned Parquet, a lifecycle rule, a least-privilege binding, a data dictionary — and a bronze layer that a human filled by hand. This week that stops.

By the end of it, three pipelines write into your bronze layer without anyone touching a keyboard, and you can say, for each of them, exactly what it would lose and exactly what it would duplicate.

In exercise 1 each of you did this alone, under your own prefix. This is the group's version: agreed between you, in the shared part of the project, and committed as code.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | A connector into bronze, with the secret columns excluded | your bucket, plus the connection exported or screenshotted in ingestion/ |
| 2 | A hand-written extractor for one table | ingestion/extract_<table>.py |
| 3 | The Kafka consumer, writing partitioned Parquet | ingestion/consume_clickstream.py |
| 4 | The clickstream's schema, written down | ingestion/clickstream.schema.json |
| 5 | A dead letter queue that rejects rather than drops | bronze/_dlq/ in the bucket |
| 6 | The failure-mode coverage list | docs/ingestion-failure-modes.md |
| 7 | An architecture decision record (ADR): which mode for which source | docs/adr/0003-ingestion-modes.md |
| 8 | The data dictionary, extended to the clickstream | docs/data-dictionary.md |
| 9 | The architecture diagram, updated | docs/architecture.md |

### 1. The connector

One connection from the company's PostgreSQL into your bronze layer, in incremental mode with a cursor you chose, or in CDC mode if you can run it.

The streams it must carry. Choose your own cursor and mode, but the connection has to bring these seven tables, because lessons 4 and 5 are built on them:

```
orders   order_items   review   product_reviews   product
customer_shipping      shipping_details
```

The first four are what lesson 4 queries. The last three are the dimensions lesson 5's star schema needs. Syncing them now costs you one extra click per stream; discovering they are missing in week five costs you an evening.

The columns that must never arrive: pwd on customer, and card_no and cvv on payment_details. Deselect them in the connection, not downstream. A column filtered in dbt has already been written to bronze, and bronze is backed up.

Commit the evidence: an exported connection configuration if your route allows it, otherwise a screenshot of the stream settings showing those columns unselected.

### 2. The extractor you write yourself

ingestion/extract_<table>.py, for one table of your choosing. Plain SQL through a database driver — no ORM, and no framework. The course does not prescribe a library; your ADR records what you used.

It must:

- read incrementally, keeping its cursor somewhere it survives a restart;
- read its credentials from the environment, never from the source file;
- retry a connection failure with a growing delay, and give up eventually.

That last point is the difference between the connector and your script: the connector does it for you, and this is the week you find out what "for you" was covering.

### 3. The consumer

ingestion/consume_clickstream.py, reading the clickstream topic and writing partitioned Parquet into bronze.

Write down which timestamp you partition by — event_time or emitted_at — and why. There is no free choice there, and lesson 7 will come back to it.

Say in the file's docstring what delivery guarantee it provides, in one sentence, and make sure the code matches the sentence. That claim is the thing the failure-mode list is built on.

### 4. The schema, written down

ingestion/clickstream.schema.json: a JSON Schema for the event, validated in the consumer before the row is accepted. The events already carry schema_version; use it to decide which schema applies.

We do not run a schema registry in this course. The discipline is the part that transfers: something states what an event must look like, and something enforces it at the boundary.

### 5. The dead letter queue

An event that fails validation goes to bronze/_dlq/, with the row, the reason and the time — not to the logs, and not to /dev/null.

One malformed row must not stop the flow. Test it: send a broken event into the topic on purpose and show that the pipeline kept running and the row is findable.

### 6. The failure-mode coverage list

docs/ingestion-failure-modes.md, one table, and it is the syllabus's deliverable for this module:

| Failure | Handled? | By what | What happens if it occurs |
|---------|----------|---------|---------------------------|

Cover at least: new rows, updates, deletes, a source that is down, a malformed row, a schema change, a duplicate after a restart, and a credential that expires.

"Not handled" is a valid answer and a good one when it is honest. Say what would happen instead. A list where everything is handled is a list nobody tested.

The same file says where your credentials live — which environment, which secret store, who can read them — and confirms they are not in the repository.

### 7. The architecture decision record

An architecture decision record (ADR) is a short, dated note saying what you decided, what else you considered, and what it commits you to — read months later by someone who was not in the room, often you. You wrote your second one in lesson 2; this is the third.

Copy docs/adr/0000-template.md to docs/adr/0003-ingestion-modes.md.

The decision: which mode for each source you ingest.
The options considered: for at least one source, the mode you rejected.
The evidence: your own numbers from exercise 1 — duplicate counts, what the cursor missed, what the slot retained.
The consequences: the delivery guarantee each choice commits you to, and what would force you to change it.

### 8 and 9. Dictionary and diagram

Extend docs/data-dictionary.md with the clickstream event: every field, its type, its meaning, and whether it carries personal data — buyer_id and session_id need an answer, not a blank.

And record what the connector adds. Airbyte writes four extra columns into every stream — _airbyte_raw_id, _airbyte_extracted_at, _airbyte_meta and _airbyte_generation_id. Your hand-written extractor writes none of them. That means bronze does not have one shape, and whoever models silver in lesson 5 needs the dictionary to say which tables came from which path.

Update docs/architecture.md so the diagram shows three arrows into bronze instead of a person with a laptop.

### Done when

- [ ] Three paths write into bronze without a human in the loop
- [ ] pwd, card_no and cvv are excluded at the connector, with evidence in the repo
- [ ] ingestion/extract_<table>.py is incremental, retries, and reads its credentials from the environment
- [ ] ingestion/consume_clickstream.py states its delivery guarantee, and the code matches
- [ ] ingestion/clickstream.schema.json exists and the consumer enforces it
- [ ] A deliberately broken event is in bronze/_dlq/ with its reason, and the pipeline did not stop
- [ ] docs/ingestion-failure-modes.md has at least one honest "not handled"
- [ ] docs/adr/0003-ingestion-modes.md argues from your own measurements
- [ ] docs/data-dictionary.md marks the personal-data fields of the clickstream
- [ ] No credential is in the repository, and the file says where they are instead
- [ ] Every member has pushed at least one commit
- [ ] The work arrived through at least one reviewed pull request

### Two things that keep costing after you stop

**Replication slots.** If you used CDC, a slot nobody reads makes the database keep log files for ever. Drop the ones you are not using before you close the week.

**Long-lived keys.** If your connector needed an HMAC key or a service account key, it still exists. Record it in docs/ingestion-failure-modes.md with the command that revokes it. A credential nobody wrote down is the one still valid in two years.

**Deadline: before session 4.** Lesson 4 models what you have landed into a warehouse, and lesson 5 is where the duplicates you are about to create get removed — so the more honest your failure-mode list is this week, the less of a surprise silver will be.

Last updated on Oct 13, 2026

---

## Project step 4: the warehouse, tuned and capped

In your group, before session 5.

After lesson 3 your bronze layer fills itself from three directions and nobody queries it directly, because every question re-reads the bucket from the beginning. This step makes it queryable, makes it cheap, and puts a ceiling on what it can spend.

It also asks for a runbook, which is the deliverable most groups underestimate. Tuning a warehouse is a one-afternoon job. Knowing what to check when a query that took two seconds starts taking two minutes is the thing your future colleagues will actually need from you.

### What you deliver

| # | Deliverable | Where it lives |
|---|-------------|----------------|
| 1 | Bronze loaded into native BigQuery tables | your group's dataset |
| 2 | Partitioning and clustering applied, with before/after numbers | docs/warehouse-tuning.md |
| 3 | A materialised view for daily demand, and the collapse ratio that justifies it | warehouse/, documented in the same file |
| 4 | Three analytical queries with their measured cost | docs/warehouse-tuning.md |
| 5 | An architecture decision record: on demand or capacity | docs/adr/0004-warehouse-pricing.md |
| 6 | The slow-query runbook | docs/runbooks/slow-query.md |
| 7 | maximum_bytes_billed set, and said where | docs/warehouse-tuning.md |

### 1 · Load bronze into native tables

Bronze stays in the bucket. What you load is the working set: the tables people filter repeatedly. Say in warehouse-tuning.md which tables you loaded and which you deliberately left external, with the reason.

A group that loads everything has not made a decision. A group that loads nothing has not done the step.

### 2 · Tune, and record what each change was worth

For each table you tuned, one row:

| Table | Change | Query | Bytes before | Bytes after | Ratio |
|-------|--------|-------|--------------|-------------|-------|

**The two ways this table ends up meaningless**

- **Bytes billed, not bytes processed.** BigQuery bills a minimum of 10 MiB, and your tables are smaller than that, so every billed figure will be identical. Record bytes processed.
- **The query cache.** A repeated query reports zero bytes. Untick Use cached results before you measure, and say on the page that you did.

A tuning table full of zeros and identical numbers is the single most common way this deliverable fails.

Report the changes that bought nothing too. You will find some — clustering on a table nobody filters by the clustering key, or a partition grain that turned out too fine. A row saying 1.0x with a sentence explaining why is worth more than the row you left out.

### 3 · The materialised view, and whether it earns its place

Build the daily-demand view. Then justify it with the number from the lecture:

```
order lines / distinct (product, day) = lines per row
```

A ratio near 1 means the view saves nothing, because the stored result is nearly the size of the table it summarises. If yours comes out near 1, you have two honest options and both score: build it anyway and say what it costs and why you kept it, or build it at a coarser grain and show that the ratio moves.

What does not score is building it because the brief said to and never checking.

### 4 · Three analytical queries, costed

Pick three questions your project actually needs answered. For each: the SQL, the bytes processed, and the cost at $7.50/TiB for europe-west1.

Most of your costs will round to $0.000. Write them down anyway and give the ratio between tuned and untuned alongside. The ratio is what survives a change of scale; the euro is not.

### 5 · The pricing ADR

An architecture decision record is a short, dated document capturing one decision, its context, the alternatives, and what you are giving up. You wrote your first in lesson 1; this is the fourth.

docs/adr/0004-warehouse-pricing.md decides on demand or capacity for this project, and it must contain:

- The workload in numbers, even if estimated — queries per day, bytes each, concentration in time.
- The crossover arithmetic. At $7.50/TiB with the first TiB free, how many TiB a month before a commitment is cheaper?
- The decision, and what it costs you. On demand means an unbounded bill from one careless query, which is why deliverable 7 exists.
- What would change your mind, expressed as a number you could watch.

### 6 · The slow-query runbook

docs/runbooks/slow-query.md. A query that took two seconds now takes two minutes. What do you check, in what order, and what does each answer rule out?

Write it for a teammate at 23:00 who did not build this. That means:

- Ordered steps, cheapest and most likely first.
- What each step rules in or out, not just what to look at.
- Where to click. "Check the execution details" is not a step; "open the job, click Execution details, find the stage whose time is far above its neighbours" is.
- A stopping condition. When do you escalate instead of continuing?

Your runbook should be able to reach at least these findings, and you should be able to say which check reveals each:

| What is actually wrong | What gives it away |
|------------------------|-------------------|
| The query lost its partition filter | Bytes processed jumped; the filter is gone or now wraps the column in something the engine cannot invert |
| The table grew past what the grain suits | Partition count or average partition size moved |
| One stage dominates | Its time is far above its neighbours in the execution details |
| Skew | Within that stage, the slowest worker is far above the average |
| The data changed shape | Rows in versus rows out moved for the same query |
| Nothing is wrong and you are comparing against a cached run | The fast "before" reported zero bytes |

That last row belongs in every runbook you ever write. Most reports of "it got slower" are really reports of "it was never that fast".

**What a runbook is not**

It is not a description of your architecture, and it is not a list of everything that could theoretically be slow. It is what a tired person does next. If a step does not change what they do, cut it.

### 7 · The cap

Set maximum_bytes_billed, and say in warehouse-tuning.md where you set it — on individual queries, on the default configuration, or both — and what value you chose and why.

Include the error message a capped query produces. A ceiling nobody has seen trip is a ceiling nobody trusts.

### Project state after this step

Bronze is queryable as native tables. The expensive questions are partitioned, clustered where that helps and materialised where it pays. Every query has a known cost and a cap, and there is a document that tells someone what to do when one of them gets slow.

And the numbers are still wrong. There is no layer between the bucket and your aggregates where anybody wrote down what an order is, so "revenue" still has more than one answer depending on which orders you count.

That is the next step, and it is the one that makes this project defensible rather than merely fast.

### How you are assessed on this step

| Criterion | What is looked for |
|-----------|-------------------|
| Before/after numbers that are real | Bytes processed, cache off, ratios given |
| Decisions with reasons | Including the ones that bought nothing |
| The collapse ratio | Checked, not assumed |
| The ADR | Arithmetic, not preference |
| The runbook | Usable by someone who did not build this, at 23:00 |
| The cap | Set, located, and seen to trip |

Last updated on Oct 13, 2026

---

## Project step 5: silver, gold, and the tests that hold them up

In your group, before session 6.

After step 4 your warehouse is fast, cheap, capped — and still cannot say what an order is. This step builds the layer that can, and defends it with tests that fail the day it stops being true.

The deliverable that matters is number 6: your team's acceptance criteria, written as tests rather than as a paragraph. Everything else is the machinery that makes that possible.

### What you deliver

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | A dbt project that builds from your bronze layer | transform/ |
| 2 | Sources declared, with freshness thresholds and why those numbers | transform/models/sources.yml |
| 3 | Staging models, one per source, each fixing a named defect | transform/models/staging/ |
| 4 | The gold star schema, grain declared in each model's description | transform/models/marts/ |
| 5 | A test per declared grain, plus relationships across the star | the same YAML |
| 6 | Your acceptance criteria, as tests | docs/acceptance-criteria.md, linking to them |
| 7 | The lineage graph | docs/lineage.md, or the image |
| 8 | The materialisation decision, as an ADR | docs/adr/0005-materialisations.md |

### 1 · A project that builds

dbt build from a clean checkout, with no manual steps in between. If a teammate has to be told something before it works, that something belongs in the repository.

Commit packages.yml and package-lock.yml. Do not commit target/ or dbt_packages/.

### 2 · Sources, with thresholds you can defend

Declare every bronze table you read. Put freshness on the ones that are actually loaded on a schedule.

Then write one sentence per threshold saying what decided it. Not "12 hours seemed reasonable" — what breaks, and for whom, if this data is twelve hours old? Two sources should probably have different numbers; if all of yours are identical, you have not made the decision yet.

### 3 · Staging, one per source

Each staging model fixes something, and the header comment names it:

```sql
-- Drops Unnamed: 0 (present on all 222,644 rows).
-- Trims blank review text into NULL: 14 empty strings, 110 single spaces.
```

Your docs/data-dictionary.md from lesson 2 is the list to work from. Every defect it records should be handled in exactly one staging model, or explicitly left alone with a reason.

### 4 and 5 · Gold, with grains you declare and test

At minimum: fct_order_items, fct_daily_demand, dim_product, dim_date.

Every fact model's description states its grain in English, and carries a uniqueness test on exactly the columns that grain names.

```yaml
  - name: fct_daily_demand
    description: "One row per product per country per day. GRAIN: (p_id, country, order_date)."
    tests:
      - dbt_utils.unique_combination_of_columns:
          arguments:
            combination_of_columns: [p_id, country, order_date]
```

A grain in a comment with no test is a claim nobody is checking — which is precisely what exercise 2's generated model did.

**The fan-out will happen to you**

Your gold layer needs at least two joins that can match more than one row: orders to addresses, and anything involving reviews. One has a rule in the data that picks one row; the other does not and must be aggregated first.

When a uniqueness test fails, do not remove the test. Removing it is the single worst thing you can do at this step, and it is the most tempting.

### 6 · Acceptance criteria, as tests

This is the step's real deliverable.

Your project brief has acceptance criteria — things that must be true for the warehouse to be usable. They are probably prose. Turn as many as you can into tests that run.

| A criterion in prose | The same thing as a test |
|----------------------|--------------------------|
| "Revenue is not double-counted" | unique_combination_of_columns on the fact grain |
| "Every order line has a product" | relationships to dim_product |
| "Ratings are between 1 and 5" | accepted_range, or accepted_values |
| "Data is no more than a day old" | freshness on the source |
| "Every product has a name" | not_null, after trimming in staging |

docs/acceptance-criteria.md lists each criterion, links to the test that checks it, and — for any you could not turn into a test — says so and says why. That list is worth as much as the tests; a criterion nobody can check is a criterion nobody is meeting.

### 7 · Lineage

dbt docs generate, then capture the graph. Commit the image or the target/ artefacts, and add one paragraph: if your most upstream source changed shape tomorrow, what would you have to look at?

### 8 · The materialisation decision, as an ADR

You have just decided something with a real cost attached, and the decision is almost invisible in the code: staging and intermediate models are views, marts are tables. Write it down, because in six months the only evidence will be a config block.

docs/adr/0005-materialisations.md, in the usual shape. The alternatives are genuine, and the ADR should say why you rejected them:

| Option | Buys you | Costs you |
|--------|----------|-----------|
| Everything as views | always fresh, nothing stored | every consumer pays the full scan, every time |
| Everything as tables | fast and cheap to read | stale between runs, and every layer stored twice |
| Views, then tables (ours) | fresh staging, fast marts | a rebuild step, and marts are only as fresh as the last run |
| Incremental marts | cheap rebuilds at scale | you must define what "new" means, and handle late-arriving rows |

Name the thing that would change your mind. If the marts took an hour to rebuild, or a consumer needed minute-fresh data, which option would you move to and what would it cost you?

### Project state after this step

Bronze is declared. Silver holds one agreed version of each fact, with every known defect fixed in exactly one place. Gold is a star schema at declared, tested grains. Your acceptance criteria run on every build.

What it still cannot do: run without a person typing dbt build, run anywhere but a laptop, or keep personal data out of gold. Lessons 7, 6 and 10.

### How you are assessed

| Criterion | What is looked for |
|-----------|-------------------|
| It builds from a clean checkout | no undocumented steps |
| Staging fixes named defects | one place per defect, traced to the data dictionary |
| Every fact declares and tests its grain | in English, and in YAML |
| The fan-out is handled, not deleted | filtered or aggregated, and you can say which and why |
| Acceptance criteria are executable | with the un-testable ones named |
| Thresholds are defended | freshness numbers with reasons, not defaults |

Last updated on Oct 13, 2026

---

## Project step 6: images that reproduce what they produced

In your group, before session 7.

Your pipeline currently runs because you are sitting in front of it. This step packages it so that a machine you have never logged into can run the same thing — and then proves it, which is deliverable 4 and the only one that cannot be faked.

### What you deliver

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | A Dockerfile per job, pinned base and pinned dependencies | ingestion/, transform/ |
| 2 | A .dockerignore that excludes .env and the virtualenv | repository root |
| 3 | Both images pushed, tagged with a date or a commit — never latest | Docker Hub or Artifact Registry |
| 4 | A clean-environment reproduction, with evidence | docs/reproduce.md |
| 5 | Installation notes a new teammate can follow | README.md |
| 6 | An ADR on the tagging scheme | docs/adr/0006-image-tags.md |
| 7 | A retention policy, and evidence you applied it | the same ADR |

### 1 and 2 · Two Dockerfiles and a .dockerignore

One for the ingestion job, one for the dbt runner.

Pinned means pinned at both ends. FROM python:3.12 is not pinned; FROM python:3.12.12-slim-bookworm is. pandas is not pinned; pandas==3.0.5 is. A Dockerfile with one end loose reproduces nothing, and section 1 of the lecture is what that looks like eighteen months later.

The .dockerignore goes in before your first build. It protects every build from that directory, including the careless one somebody writes in a hurry — which is the point.

### 3 · Tags that mean something

Either registry is fine and the choice is yours to defend in deliverable 6. Docker Hub is free and public; Artifact Registry is private and costs $0.100 per GiB-month above a free 0.5 GiB.

No image is tagged latest. If a tag in your repository can point at two different builds on two different days, deliverable 4 is impossible.

### 4 · Reproduce it from nothing — the deliverable that matters

docs/reproduce.md shows, as a transcript rather than a description:

1. A job run, and what it produced — a row count, a file, a table.
2. The image deleted from the machine: docker rmi, then docker images showing it gone.
3. The image pulled back by tag.
4. The job run again, and the same output.
5. The digest from the push and the digest from the pull, side by side.

The digests are the evidence. Everything else can be retyped from memory; two matching sha256: values cannot.

**The three ways this gets faked**

- Running it again without deleting the image first. Deleting is the whole test.
- A different output "close enough" to the first. If the row count moved because the load generator was running, say so and pin the input instead.
- Pasting a description of what happened. A transcript has prompts and error messages in it. Prose does not.

### 5 · Installation notes

README.md gets a section a new teammate can follow on a machine with nothing on it. Assume Docker, assume nothing else.

The test is simple and you should actually run it: hand it to somebody in another group and watch where they stop.

### 6 · The ADR on tagging

docs/adr/0006-image-tags.md. The usual shape — context, decision, alternatives, consequences — answering:

- What goes in a tag? A date, a git short SHA, a semantic version?
- Which registry, and why? Cost, privacy, who needs to pull it.
- What happens when you need to roll back? Name the exact commands.
- What would change your mind?

### 7 · The retention policy, with evidence

Added because Docker makes this genuinely necessary: a tag per push, per job, per group, never deleted, is how a registry bill grows without anybody deciding anything.

The ADR states the policy — which tags survive, and for how long — and docs/reproduce.md or the ADR shows the command that enforced it, and its output:

```bash
# Docker Hub: via the web UI, or the API
# Artifact Registry:
gcloud artifacts docker images delete <image>:<tag> --delete-tags --quiet
```

Evidence, not intention. "We will clean up monthly" is not a policy; a deletion you actually ran is.

### Project state after this step

Every job in the pipeline is an image. Every image is pinned at both ends, carries no secrets, runs as a non-root user, and is tagged so that a specific build can be named and pulled back. One of them has been reproduced from nothing, on the record.

What it still cannot do: run without a person. Lesson 7 schedules exactly what you built here, and it can only do that because you pinned it.

### How you are assessed

| Criterion | What is looked for |
|-----------|-------------------|
| Pinned at both ends | base image and dependencies |
| No secret in any image | and a .dockerignore that makes it structural |
| Non-root | in both images |
| Tags name a build | and latest appears nowhere |
| The reproduction is a transcript | with two matching digests |
| The retention policy was executed | not just written |

Last updated on Oct 13, 2026

---

## Project step 7: your pipeline, on a schedule

Lesson 6 left you with images that any machine can run, and a person who has to type. This step hands them to a scheduler. By the end of it your platform runs without you — which is the first time in this course that it can also go wrong without you, so half of this step is about knowing when it has.

Groups of three or four, in the repository you have been building since lesson 1.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | A DAG that runs your pipeline end to end | orchestration/dags/ |
| 2 | Every task idempotent, and a note saying how each one achieves it | the DAG, plus docs/orchestration.md |
| 3 | Retries with a sensible delay, and a failure alert that reaches a person | the DAG |
| 4 | A quality gate, and proof that it stops the pipeline | orchestration/dags/, written up in docs/orchestration.md |
| 5 | A back-fill you actually ran, with before and after row counts | docs/orchestration.md |
| 6 | The integration-failure runbook | docs/runbooks/failed-run.md |
| 7 | How reruns are made safe, as an ADR | docs/adr/0007-idempotency.md |

### 1. The DAG

One DAG, running the containers you built in lesson 6, in order. It belongs in orchestration/dags/ in your repository, and the folder is what your Airflow dags/ points at.

Keep the DAG file to structure. No pandas import, no database query at the top level: that file is re-parsed on a timer, and work at import time slows the scheduler for everybody.

Pass the period into each task from the run, not from the clock. If a task needs to know which day it is covering, it reads it from an environment variable your DAG fills with {{ data_interval_start | ds }}.

### 2. Idempotency, with the mechanism named

This is the deliverable that matters. For every task that writes anything, docs/orchestration.md carries one line:

| Task | What it writes | Why running it twice is safe |
|------|----------------|------------------------------|
| load_orders | bronze.orders | MERGE on order_id |
| … | | |

"It is idempotent" is a claim. The note has to name the mechanism — the merge key, the partition you replace, the constraint, the CREATE OR REPLACE. If you cannot name one for a task, that task is not idempotent yet, and saying so honestly scores better than a sentence that sounds confident.

Remember what you measured in class: BigQuery does not enforce uniqueness, so a primary key is not a mechanism you can lean on there.

### 3. Retries, and an alert that reaches a person

Retries on every task, with a delay that is longer than the thing you are retrying — a five-second retry against an API that rate-limits you is three failures instead of one.

The alert goes on on_failure_callback, which fires when a task has failed for the last time. Not on_retry_callback, which fires on every attempt and will teach your group to ignore the channel within a week.

Where it lands is your choice: a Slack webhook, an email, a line in a log somebody reads. Say in docs/orchestration.md who is supposed to see it and what they are expected to do about it.

### 4. A quality gate, and proof that it stops the pipeline

Your gate is dbt test as a task — the tests you have been writing since lesson 5, run by the scheduler instead of by you.

A gate nobody has ever seen fire is a decoration, so fire it:

1. Insert a poison row into bronze, designed against one of your own tests — a duplicate key, a null in a not_null column, an orphan foreign key.
2. Run the DAG for that period.
3. Record what happened: which test failed, whether the run went red, whether the alert arrived.
4. Then look at your marts. If the bad number reached them, say so.
5. Delete the poison row.

If step 4 found the bad row in a mart, your gate is an alarm and not a lock. Fix it — dbt build tests each model as it is built and skips everything downstream of a failure — and record both states. The before and after is the deliverable, not a clean run.

### 5. A back-fill you actually ran

Pick a range you can check, predict the row counts before you run it, and write the prediction down. Then run it:

```bash
airflow backfill create --dag-id <your_dag> \
  --from-date <start> --to-date <end> --dry-run
```

--dry-run lists the runs it would create. Look at that list before you run it for real.

In docs/orchestration.md: the range, your prediction, the actual counts, and — if they differ — why. A wrong prediction that you explain is worth more than a right one you did not think about.

### 6. The runbook

docs/runbooks/failed-run.md, written for the person on call at 3am who did not build this. It answers, in order:

1. How do I know something is wrong? Where the alert arrives, what it says.
2. What do I look at first? Which task, which log, where in the UI.
3. What are the three things that usually broke? From your own week — credentials, a source that moved, an image tag that no longer exists.
4. Is it safe to just re-run it? This is the question the runbook exists for, and your answer comes from deliverable 2. If it is not safe, say what to do instead.
5. Who do I tell, and when do I stop?

Your fire drill in deliverable 4 is what proves the runbook is accurate. If the drill taught you something the runbook does not say, the runbook is wrong.

### 7. How reruns are made safe, as an ADR

Deliverable 2 asks you to name an idempotency mechanism per writing task. This is the decision behind those notes, gathered in one place — because it is the thing you will be asked to defend: what happens when the same interval runs twice?

docs/adr/0007-idempotency.md. The alternatives are real:

| Option | Works when | Breaks when |
|--------|------------|-------------|
| Delete, then insert, by partition | the interval maps to one partition | an interval spans partitions, or the delete succeeds and the insert fails |
| MERGE on a business key | you have a key you trust | the key is not unique — which lesson 5 taught you to test for |
| Write a new table, then swap | the table is small enough | storage doubles during the swap |
| Accept duplicates | never, here | always — lesson 4 showed you what they do to revenue |

Say which tasks use which, and name the one you are least sure about. A rerun nobody has thought about is what pages somebody at 3am.

### Done when

- [ ] The DAG runs the whole pipeline end to end, from a schedule, with no manual step
- [ ] Every writing task has a named idempotency mechanism in docs/orchestration.md
- [ ] A deliberate failure produced an alert that a named person received
- [ ] The gate was fired with a poison row, and what reached the marts is recorded
- [ ] A back-fill was run, with the prediction written down beforehand
- [ ] docs/runbooks/failed-run.md exists and answers "is it safe to re-run"
- [ ] docs/adr/0007-idempotency.md names a mechanism per writing task, and the alternative it was chosen over
- [ ] Every member has pushed at least one commit, and the work arrived through a reviewed pull request

**Deadline: before session 8.** Lesson 8 stops creating your infrastructure by clicking on it and declares it in Terraform instead — including, if you get that far, the machine this scheduler runs on.

Last updated on Oct 13, 2026

---

## Project step 8: your platform, declared

Seven lessons built a platform by hand. This step writes it down, so that the question lesson 8 opened with — rebuild it in a new project, today — has an answer that is a directory rather than a shrug.

Groups of three or four, in the repository you have been building since lesson 1.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | Bucket, datasets and service account declared | infra/terraform/ |
| 2 | Remote state in a gcs backend, with versioning on | infra/terraform/versions.tf |
| 3 | Provider pinned, and .terraform.lock.hcl committed | the same |
| 4 | terraform fmt -check and validate on every pull request | .github/workflows/ |
| 5 | A drift you caused and caught, with the plan output | docs/infra.md |
| 6 | One line per resource: why it exists | docs/infra.md |
| 7 | Where state lives, as an ADR | docs/adr/0008-remote-state.md |

### 1. The platform, declared

Everything lessons 2 to 6 created by hand, in infra/terraform/: the bucket with its lifecycle rule, your datasets, and the service account from lesson 2.

Adopt, do not recreate. Use import blocks and get to No changes. Your infrastructure matches the configuration. — that sentence is the deliverable, not a file that would work if you started from nothing.

If some resource cannot be adopted cleanly, say so in docs/infra.md and move on. A description that is honest about a gap beats one that hides it.

**What is in scope for access, and what is not**

You can declare resource-level access — google_storage_bucket_iam_member and google_bigquery_dataset_iam_member — and you should, for anything your pipeline needs.

You cannot declare project-level bindings, because your project does not grant the role that allows it. That is deliberate, and lesson 10 is about why. Do not spend an evening fighting it.

### 2 and 3. State, and pins

State in a gcs bucket with object versioning enabled. The bucket is made by hand — that is expected, and deliverable 6 is where you record that you did it.

required_version and a provider constraint in versions.tf, and .terraform.lock.hcl committed. Then check that terraform.tfstate* and .terraform/ are in .gitignore, and that git log -- infra/terraform/terraform.tfstate finds nothing.

### 4. Continuous integration that needs no credentials

A workflow that runs on every pull request touching infra/:

```yaml
- run: terraform fmt -check -recursive
- run: terraform init -backend=false
- run: terraform validate
```

-backend=false is the important flag. It lets init set up providers without reaching for your state bucket, so the job needs no cloud access at all — which is why every group can have this and nobody needs a key.

plan stays on your own machines, where your own login already works.

### 5. A drift you caused and caught

The same shape as lesson 7's fired gate, and the same reason: a mechanism nobody has ever seen work is a claim.

1. Change something about a managed resource outside Terraform.
2. Run plan, and save the output.
3. Decide: adopt or overwrite. Do it.
4. Write down which you chose and why — and what you would do if it had been an emergency fix during an incident instead of your own test.

Paste the plan output into docs/infra.md. The output is evidence; the paragraph is the deliverable.

### 6. Why each resource exists

One line per resource, in a table:

| Resource | Why it exists | Made by hand? |
|----------|---------------|--------------|
| google_storage_bucket.bronze | the landing zone from lesson 2 | no — adopted |
| the state bucket | holds Terraform's state; cannot manage itself | yes, once |
| … | | |

This is the answer to lesson 8's opening question, and the reason the step is worth doing at all. The 23 enabled APIs in the teaching project were each switched on by somebody for a reason nobody wrote down; this table is you not doing that.

### 7. Where state lives, as an ADR

You moved state into a bucket this week. That is a decision with consequences for everybody who will ever run terraform apply on this platform, and it is exactly the kind of choice the oral asks you to defend.

docs/adr/0008-remote-state.md:

| Option | Buys you | Costs you |
|--------|----------|-----------|
| Local state | nothing to set up | one laptop holds the only copy, and nobody else can apply |
| A gcs bucket (ours) | shared, versioned, locked | a bucket Terraform cannot manage itself |
| Terraform Cloud | locking, history and policy, managed | an account each, and your state on somebody else's platform |

Answer two questions in it, because they are the ones you will be asked: who can read that bucket, and what happens if somebody deletes the state file — you saw the answer to the second one in the lecture.

### Done when

- [ ] terraform plan prints no changes against your real project
- [ ] State is in a versioned bucket, and no state file is in git history
- [ ] The provider is pinned and .terraform.lock.hcl is committed
- [ ] A pull request shows fmt and validate running, and passing
- [ ] A drift was caused, caught, decided on and written up
- [ ] Every resource has a line saying why it exists
- [ ] docs/adr/0008-remote-state.md says where state lives, who can read it, and what losing it would cost
- [ ] Every member has pushed at least one commit, and the work arrived through a reviewed pull request

**Deadline: before session 9.** Lesson 9 asks what all of this costs — and it is much easier to answer about resources that are in a file than about resources that are in a console.

Last updated on Oct 13, 2026

---

## Project step 9: attributable, and one saving measured

Lesson 8 wrote your platform down. This step makes its cost readable — so that when somebody asks what the pipeline costs, the answer is a query rather than a shrug.

Groups of three or four, in the repository you have been building since lesson 1.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | Every Terraform resource carries your label convention | infra/terraform/ |
| 2 | The convention itself, written down and justified | docs/costs.md |
| 3 | A budget alert, and what it does not do, in one line | docs/costs.md |
| 4 | Your largest cost, its SKU, and its shape | docs/costs.md |
| 5 | One optimisation, with a before and an after | docs/costs.md |
| 6 | The ledger: one row per resource, with price and date | docs/costs.md |
| 7 | How spend is attributed, as an ADR | docs/adr/0009-cost-attribution.md |

### 1 and 2. Labels, and the convention behind them

Every resource. Agree the keys first, and write down why each one exists — a convention nobody can explain is one that stops being applied in March.

Four keys is usually right. More than six and people start leaving them out.

Apply it through Terraform, not the console, and note in docs/costs.md that labels only apply from the moment they are set. If some of your spend is already unattributable, say so — that sentence is worth more than a tidy table.

### 3. An alert somebody receives

A threshold on your project's budget, and the name of the person it reaches.

Then one line on what it will not do. If your line does not contain the words "does not stop", read section 5 again.

### 4. Your largest cost, and its shape

Query the billing view for your project. The total gives you the SKU; grouping by day gives you the shape.

Name which of the five it is. If your bill is entirely zeros — likely, inside the free tier — say so and use the units instead: bytes scanned per day, gigabyte-months stored. The shape is visible in units long before it is visible in money.

If the export still has no rows by the deadline, fall back to INFORMATION_SCHEMA.JOBS as in exercise 1 and say that is what you used. Remember that both sources omit days on which nothing ran.

### 5. One optimisation, with a before and an after

The deliverable that matters, and the one with a deadline problem: billing lags and you are inside the free tier, so you will probably not see money move.

So measure units.

| What we measured | bytes scanned by the hourly query |
|------------------|-----------------------------------|
| Before, on 2026-11-02 | 4.8 GiB per run |
| The change | partitioned orders on order_date |
| After, on 2026-11-04 | 0.4 GiB per run |
| At $7.50/TiB | €0.035 → €0.003 per run, about €23/month at hourly |

Then the sentence that makes it engineering rather than decoration: what would have told you that you were wrong.

### 6. The ledger, finally

One row per resource you own: what it is, what drives its cost, the published unit price, the date you read that price, and your estimate.

This is the file lesson 2 asked for. It is also the answer to the question this lesson opened with — and the reason it is deliverable 6 rather than deliverable 1 is that until this week you had no labels, so you could not have been sure the list was complete.

### 7. How spend is attributed, as an ADR

Deliverable 2 writes down what your label convention is. This one records why attribution works that way at all, which is the question somebody will ask when the platform has four teams on it instead of one.

docs/adr/0009-cost-attribution.md. The alternatives are not obvious, and that is the point:

| Option | Buys you | Costs you |
|--------|----------|-----------|
| Labels on every resource (ours) | one project, one bill, attribution by query | labels apply forward only, and nothing enforces them |
| A project per team | attribution for free, and a hard boundary | a project to provision per team, and cross-team data gets awkward |
| Tags instead of labels | inheritance, and they can be required by policy | organisation-level setup you do not have |
| Split the bill by hand | nothing to set up | somebody's afternoon, every month, and it is always wrong |

Say what you would do differently with ten teams, because the answer is usually not "more labels".

### Done when

- [ ] Every resource in infra/terraform/ carries the convention
- [ ] docs/costs.md explains the convention and admits what is already unattributable
- [ ] A budget alert exists, reaches a named person, and its limits are stated
- [ ] The largest cost is identified by SKU and by shape
- [ ] One optimisation has a before, an after, units, and a failure condition
- [ ] The ledger has one row per resource, each with a price and a date
- [ ] docs/adr/0009-cost-attribution.md says how attribution works and what you would change at ten teams
- [ ] Every member has pushed at least one commit, through a reviewed pull request

**Deadline: before session 10.** Lesson 10 asks the other half of the question: you now know whose cost it is — but who can read it?

Last updated on Oct 13, 2026

---

## Project step 10: classified, restricted, and erasable

The last step that changes the platform. Lesson 9 made its cost readable; this one makes its contents readable only by the people who should read them, and provable afterwards.

Groups of three or four, in the repository you have been building since lesson 1.

### What you produce this week

| # | Deliverable | Where |
|---|-------------|-------|
| 1 | A data-classification policy — every column, its class, its owner | docs/governance.md |
| 2 | An access matrix — role × class × what they may do | docs/governance.md |
| 3 | The platform hardened: narrowed roles, classified columns, enforcement on | infra/terraform/ |
| 4 | An erasure runbook, tested once | docs/runbooks/erasure.md |
| 5 | How access is decided, as an ADR | docs/adr/0010-data-classification.md |

### 1. The classification policy

Every column in your warehouse gets a class. Most will be public or internal, and writing those down is not wasted work — it is the evidence that you looked.

| Column | Class | Owner | Why |
|--------|-------|-------|-----|
| shipping_details.phone | pii.contact | the group | directly identifies a person |
| orders.order_date | internal | the group | no personal content |
| review.r_desc | unclassified | the group | free text; may contain anything |

That last row is required. Name your free-text columns and mark them honestly. A policy that quietly implies they are clean is worse than one that admits it cannot tell — and a reader who knows which columns are uncertain can do something about it.

Who the owner is matters more than what the class is. An owner is a role that can answer questions about that column. "The group" is acceptable now; "nobody" is the thing to avoid, and it is what you have today.

### 2. The access matrix

| Role | public | internal | pii.contact | unclassified |
|------|--------|----------|-------------|--------------|
| Analyst | read | read | no | no |
| Engineer | read | read | read | read |
| ML service account | read | read | no | no |

Three or four roles is plenty. For each no, say what someone in that role should do when they genuinely need the data — a request process, even an informal one, because a rule with no exception path is a rule people route around.

### 3. Harden it

- At least two columns classified and enforced, through a taxonomy and policy tags.
- At least one narrowed grant — a role or a scope reduced, with the reason in the commit message.
- The access matrix reflected in reality, as far as your lab project allows. Where it cannot be, say so in docs/governance.md: "the matrix says the ML service account cannot read pii.contact; we could not enforce this because …". A documented gap is a finding. An undocumented one is a lie.
- Labels from lesson 9 are still on every resource. Do not lose them.

**Find out what breaks before you enforce**

Tagging a column takes effect immediately, for everything. Before you tag anything, run the audit log for your dataset and find out what reads it.

If you break your own pipeline, fix it and write down what happened — that is a better deliverable than a platform you hardened by never touching anything.

### 4. The erasure runbook

docs/runbooks/erasure.md, written so that somebody who was not in the room can follow it. It must cover:

1. How you find the person — every table and column holding the identifier.
2. Silver and gold — the dbt command, and what it rebuilds.
3. Bronze — the procedure, honestly. If you have not automated it, write the manual steps and say how long they took.
4. The copies that survive — object versions and time travel, with your retention settings and what you would have to do about them.
5. How you would prove it was done.

Test it once, on one real identifier, and record what happened — including the parts that did not work.

**The deliverable is the honest version**

A runbook saying "steps 1 to 3 take about two hours; step 4 we have not solved, because our bucket keeps versions for 30 days and we have not decided whether to shorten that" is a better deliverable than one that claims a clean erasure.

The first can be handed to a lawyer. The second falls apart the first time somebody checks.

### 5. How access is decided, as an ADR

The classification policy says which columns are sensitive. This says why access is granted by classification at all, rather than by one of the simpler things you could have done.

docs/adr/0010-data-classification.md:

| Option | Buys you | Costs you |
|--------|----------|-----------|
| Classify columns, grant on the class (ours) | one rule survives new tables; auditable | two grants to reason about, and a tag that silently protects nothing if misconfigured |
| A view per consumer | exactly the right shape, no tagging | a view to maintain per consumer, forever |
| A dataset per sensitivity level | simple, coarse, hard to get wrong | the same data copied twice, and two things to keep in step |
| Trust people not to query it | nothing to build | it is not a control, and the audit log will eventually prove it |

This is the last ADR of the course and the best one to defend, because every option above is defensible and the argument between them is the job.

### Done when

- [ ] Every column in the warehouse has a class and an owner in docs/governance.md
- [ ] Free-text columns are explicitly marked as unclassified
- [ ] The access matrix covers every class, with a route for each no
- [ ] At least two columns are classified and enforcement is verified by being refused
- [ ] At least one grant has been narrowed, with the reason in the commit
- [ ] Gaps between the matrix and what you could enforce are written down
- [ ] docs/runbooks/erasure.md exists and has been run once
- [ ] docs/adr/0010-data-classification.md argues for classification over the alternatives, rather than only describing what you built
- [ ] Every member has pushed at least one commit, through a reviewed pull request

**Deadline: before session 11,** which reviews the whole course — these two documents are the governance half of what we will look at.

### Why this one is worth doing properly

Of everything you have built, this is the step most likely to be asked for by name. "Show me your data classification and your access matrix" is a normal question in an audit, a security review, and a due-diligence process — and being able to answer it with a file rather than a meeting is the difference the whole lesson is about.

Last updated on Oct 13, 2026

---

## Project defence

Fifteen minutes each. Ten to show what you built and why, five to answer for it. Everything about how the day runs is on this page, in advance, because a defence that surprises you measures composure rather than engineering.

In this session:

- This page: how the day runs, the fifteen minutes, and how they are scored.
- How to present this: what works and what does not, for this project specifically.
- The checklist: every deliverable from all ten project steps, in one table. Check it before you present.

### 1. How the day runs

| Time | What happens |
|------|--------------|
| 0:00 | How the session works, and the peer-review form opens |
| 0:10 | Defences — four groups |
| 1:10 | Break, fifteen minutes |
| 1:25 | Defences — the remaining groups |
| 2:25 | Peer review submitted, and the floor |
| 2:45 | Close |

Your teacher sets the running order and will tell you before the day. You will not be called at random on the morning.

Every group stays for every defence. Not politeness — peer review is 10% of the project grade, and you cannot write one for a presentation you did not watch.

### 2. The fifteen minutes

| Time | What happens |
|------|--------------|
| 10 minutes | your pitch, structured however you like |
| 5 minutes | questions, from your teacher and from the other groups |

The clock is hard. At ten minutes the pitch stops, mid-sentence if necessary, and you keep your five minutes of questions. A group that has not rehearsed against a clock loses the end of its pitch — which is always the part with the interesting content in it.

**The unprepared question**

In the five minutes, each member is asked one question about one architecture decision — and not one they chose. It may be about a decision somebody else in the group wrote.

This is what the 10% for individual deliberation is. It is not a trick: every question comes from one of the ten decision records in your own repository, and you have had a week with them.

**The honest answer is a good answer**

"I don't know — Marie wrote that one. What I can tell you is why we needed it and what it cost us" is a strong response. It is specific, it is true, and it shows you understand the shape of the decision even though you did not make it.

Guessing is the weak move. A confident wrong answer about your own platform is the one thing that is hard to recover from in five minutes.

### 3. How it is scored

The project and defence together are 50% of the course. Within that:

| Weight | Judged from |
|--------|-------------|
| The repository | 40% | your repository, read before the day, against the checklist |
| The group oral | 40% | the pitch and the group's answers |
| Individual deliberation | 10% | your unprepared question |
| Peer review | 10% | the forms other groups write about you |

Most of your mark is already decided by the time you walk in: 40% is the repository you have been building for ten weeks. The defence is where you account for it, not where you earn it from nothing.

### 4. Peer review

Every group reviews every other group, on a form that opens at the start of the session and is submitted before you leave.

You are not marking them out of ten. The questions are about things you can observe:

- Did they justify a decision with a number rather than an adjective?
- Did they name a trade-off — something their choice cost them?
- Could every member answer a question about the platform?
- What is one thing you would take back to your own project?

**What happens to what you write**

Your written comments go back to the group you reviewed, with your name removed. Your teacher knows who wrote what; the group does not.

Your scores stay private — they go to your teacher and nowhere else.

So write the comment you would want to receive, and score honestly.

Reviewing well is marked too. A form that says "good job" for all seven groups tells your teacher nothing, including about you.

### 5. If you cannot be there

A well-justified absence costs you no marks. Tell your teacher as early as you can.

Your group presents without you, and your unprepared question comes by email — you answer it in writing, about one of your repository's decision records, the same question you would have been asked in the room.

### 6. What to do before the day

- Open the checklist and walk your repository against it. The 40% is marked from exactly that list, and the most common loss is a file nobody has touched since week two.
- Read how to present this. It is short, and it is specific to this project rather than general advice.
- Rehearse once, against a clock, with the handovers between speakers.
- Check your video plays from the link in your README.md, from a different machine than the one that recorded it.

### 7. After today

The written exam is separately scheduled, and comes after this session. Today is not the end of the course.

It has three parts: a critique of a generated proposal — which you rehearsed in session 11 — an applied knowledge check, and a case study. The ten patterns on the review page are the most efficient revision you have.

Last updated on Oct 13, 2026