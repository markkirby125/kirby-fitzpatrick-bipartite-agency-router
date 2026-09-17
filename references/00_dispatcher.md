# Bipartite Agency Router — Technical Operational Dispatcher

**Framework Author**: William Fitzpatrick (*Writer Science*)  
**Source Lecture**: [You're NOT a Bad Researcher! How to Easily Boost Citations](https://www.youtube.com/watch?v=kTl79dinMBM)  
**Parent Collection**: [Master Collection](../../kirby-fitzpatrick-writers-collection/SKILL.md) | [Global Help](../../../kirby-help/SKILL.md)  

---

## 1. Cognitive Foundation: Voice Is an Ownership Contract, Not a Style Choice

Every technical artifact is not a document; it is **a register of claims**. Each sentence in it does one of exactly two jobs, and the grammatical subject is the control surface that selects which job the sentence is performing:

* **Assignment.** *Someone chose this, and that someone can be held to it, credited for it, challenged on it, or cited as its source.* The reader's question is **"who?"** — who decided, who owns the cost, who to push back on, who to cite, who will defend this in six months.
* **Behaviour.** *This is how the system/process behaves, and the description is repeatable by anyone who reads it.* The reader's question is **"what happens, and can I run it?"** — what the job does, what the failure looks like, how to reproduce the result.

Fitzpatrick's bipartite-agency move, translated into engineering terms: **these two jobs require opposite grammatical voices, and they coexist inside one repository, one artifact, and frequently one paragraph.** Proposals, decisions, and the acceptance of a tradeoff must be **owned** — first-person active (`I`, `we`), one accountable agent per claim. Procedures, timelines, post-mortems, release notes, and code comments must be **agentless** — the system, the process, the job, the artifact, or the change sits in the subject position, and no human is ever the subject of a defect.

The failure is never the sentence. It is **routing the voice to the wrong reader-question**, and the cost is not aesthetic — it is informational corruption in two directions:

* **Ownership Hole.** A decision stated agentlessly — *"the config format was changed"*, *"it was decided that the cap is 3"*, *"the tradeoff was accepted"* — produces a claim that no one can defend, re-evaluate, or revert rationally. It survives review because it sounds objective. It dies at the first reorg, and it gets reverted by surprise two quarters later when a new owner discovers a cost nobody is recorded as having accepted.
* **Blame Leak.** A system defect stated with a person in the subject position — *"Dave deployed the config without running the check"*, *"we should have known better"*, *"this looks rushed"* — converts a missing guard into a person, and the report gets sanitised in the next revision. The guard is still missing. The defect recurs, now undocumented.

The same grammar governs citation mechanics, which is the lecture's actual subject. A claim whose agent is discoverable is **assignable** — it can be attributed, linked, and reused; a claim with a deleted agent is an orphan node that nobody can point at, and a procedure with a named human agent is **not portable**, so no one can adopt it as their method. Boosting citations, in a repository, means raising the number of independent, attributable, reusable claim nodes: PRs, ADRs, post-mortems, runbooks. The lecture's reassurance is structural, and it transfers directly: **the research is usually fine; the voice carrying it has been routed to the wrong reader.**

Five load-bearing definitions:

1. **Agency Assignment (A1)** — a sentence whose subject is an accountable human or a role (author, reviewer, on-call rotation, team + queue). The sentence is a claim about a *choice*, so it can be defended, overturned, credited, and cited.
2. **Agentless System Subject (A0)** — a sentence whose subject is a system noun: the job, the service, the queue, the artifact, the change, the procedure. The sentence is a claim about *behaviour*, so it is repeatable and auditable. A0 requires a *real* subject; a sentence with no subject at all is not A0, it is an accountability hole wearing the same clothes.
3. **Ownership Hole** — a decision sentence in a durable artifact with nobody in the subject position. Symptom: nobody remembers choosing it, yet everyone is bound by it.
4. **Blame Leak** — a defect sentence in a durable artifact with a person in the subject position. Symptom: the report is revised for tone instead of for the missing guard.
5. **Referent Decay** — the half-life of a pronoun. `I`, `we`, `you`, `someone`, `they` resolve only against an address table that lives outside the artifact, in one head, at one moment. Every pronoun in a durable artifact is a bet that the reader shares the writer's address space — a bet that fails the first time an on-call engineer reads the runbook at 03:00.

```text
[BEFORE — one voice, chosen by habit, applied to every artifact]

   artifact                    habit voice            reader asks                 failure

   RFC / ADR                   agentless passive      "who decided this?"         OWNERSHIP HOLE
   "a decision was made"                              "who do I push back on?"    reverted by surprise
   ------------------------------------------------------------ -------------------------------
   PR description              first-person diary     "what actually changed?"    credit without evidence
   "I fixed some stuff"                               "what breaks if I revert?"  unanswerable
   ------------------------------------------------------------ -------------------------------
   runbook                     "I" / "you"            "who is 'I' at 03:00?"      REFERENT DECAY
   "I usually flush first"                            "can I repeat this?"        not portable
   ------------------------------------------------------------ -------------------------------
   post-mortem                 vague "we" + names     "who got blamed?"           BLAME LEAK
   "we should have known"                             "what guard was missing?"   sanitised, recurs
   ------------------------------------------------------------ -------------------------------
   release notes               passive pile           "what changed for ME?"      unmappable to the
   "improvements were made"                           "which version / PR?"       reader's system
```

```text
[AFTER — the router: pick the voice per SENTENCE, from the reader's question]

                     ┌──────────────────────────────────────────────────┐
                     │ what must the reader DO with this line?          │
                     └───────────────────────┬──────────────────────────┘
                                             │
   "hold someone to it / cite it / credit it / overturn it"      "repeat it / obey it / diagnose it"
                                             │                                        │
                                             ▼                                        ▼
                    ┌──────────────────────────────────┐        ┌──────────────────────────────────┐
                    │ A1 · ACTIVE AGENCY               │        │ A0 · AGENTLESS SYSTEM            │
                    │ subject = person OR role         │        │ subject = system, job, service,  │
                    │ "I propose min(interval, 512)    │        │ config, artifact or change       │
                    │  and I accept +40% write cost"   │        │ "the reconciler drains the queue │
                    │                                  │        │  every 2s; a slow drain buffers" │
                    │ artifacts: ADR, RFC, PR body,    │        │ artifacts: runbook, post-mortem, │
                    │ release note attribution line,   │        │ timeline, diff/commit body,      │
                    │ review verdict, rollback promise │        │ comment, method, evidence        │
                    │ budget: exactly 1 owner per claim│        │ budget: exactly 0 human pronouns │
                    └──────────────────────────────────┘        └──────────────────────────────────┘
                              └────────── both modes in one artifact is CORRECT ──────────┘
                    the leak is never the mixture — it is using the wrong mode on a given line
```

```text
[TWO SYMMETRIC LEAKS — one move repairs both]

   OWNERSHIP HOLE                          BLAME LEAK
   "the config format was changed"         "Dave changed the config without the check"
   "it was decided the cap is 3"           "we should have known the queue'd blow up"
             │                                        │
             ▼                                        ▼
   nobody owns the tradeoff                 the missing guard is never recorded
   → reverted by surprise, silently         → blame doc, sanitised, defect recurs
             │                                        │
             └────────────► SAME REPAIR ◄─────────────┘
   For a DECISION  →  name the accountable entity: person, or role + queue.
                      (never an unbounded "we"; never nobody.)
   For a PROCEDURE →  name the system component, config, or version that acted.
                      (never a person; never nobody.)
   "Dave" is not the diagnosis. "config-service accepted a rollout without
    gating on migration state" IS the diagnosis — and it names its own fix.
```

Why this binds harder in software than in prose: engineering artifacts are read out of order, under time pressure, by strangers holding production credentials, and their readers *act*. A research paper with an ownership hole gets cited vaguely; an ADR with an ownership hole gets reverted during an incident by someone who cannot find out why the constraint existed. Some artifacts also earn a hard requirement rather than a preference — **runbooks, rollback procedures, and post-mortems have a zero-human-pronoun budget**, because they are executed by a rotation that will never include the author, on the exact day the author is unavailable.

---

## 2. Core Transformation Protocols

1. **Route per sentence, never per document.** Voice is a local decision. Classify each sentence as *decision/claim* (A1) or *procedure/behaviour/evidence* (A0) before choosing a subject. A single correct PR body contains both modes and switches at section boundaries.
2. **Derive the mode from the reader's question, not from habit.** `who decided / who owns the cost / who to cite / who to page` → A1. `what does the system do / what does failure look like / how do I repeat this` → A0.
3. **A1 requires exactly one accountable agent per claim.** *We* is not an agent — it is a subject with an unbounded referent. Replace it with a person, a team, or a role plus a queue: `I propose…`, `the platform team accepted…`, `platform-primary owns…`.
4. **First person claims a choice, never a task.** `I propose X and I accept cost Y` is a claim with an owner. `I ran the tests` and `I changed the file` are procedure narrated in the wrong mode — the system is the subject: `the suite passes on 4f9c1ab`. Own the **decision**; let the **work** be agentless.
5. **A0 requires a real subject.** `the ledger was reconciled` is not agentless-neutral, it is anonymous. `the reconciler job reconciled the ledger at 03:14 UTC` names the behaviour and is reproducible. Prefer the concrete system noun over any passive at all; `the deploy bot applies the config` beats `the config is applied by the deploy bot`.
6. **Ban agent-hiding passive.** Any passive whose function is to elide responsibility is a defect, not a neutral: *mistakes were made*, *concerns were raised*, *this has been flagged*, *it was agreed that*, *the decision was reached*. Repair by naming the system (behaviour) or the role (decision).
7. **Ban human pronouns in procedures.** No `I`, `me`, `my`, `you`, `your` inside a runbook step, a timeline entry, a diff comment, or a release note body. Replace with the process subject (`the worker drains…`), an imperative (`await compaction before flushing`), or a role noun (`the on-call engineer verifies…`).
8. **Bound the *we*, and never let it carry blame or ownership.** In post-mortems, RFCs, and ADRs, replace attributive *we* with a role, a team, or a **decision artifact ID**: `we decided to cap at 3` → `the cap of 3 is recorded in ADR-021 (owner role: storage-primary)`. `We` may remain only for a genuinely collective, dated, non-attributive fact (`the team has run this since v2.0`).
9. **Blameless means no *person* as subject — not no *system* as subject.** A post-mortem that says *something went wrong* is not blameless, it is useless. Name the component, the version, the config, the absent guard, and the runbook step that did not gate. Blamelessness is achieved by moving the subject from the person to the mechanism; it is never achieved by deleting the subject.
10. **Keep evidence agentless.** Benchmarks, query counts, traces, fixtures: the agent is noise. `the benchmark ran on main @ 9f21c0d; p99 fell from 3,140 to 610 ms over 5 runs` — reproducible, falsifiable, free of biography. Attach the attribution edge instead: PR number, commit SHA, ADR ID.
11. **Every agentless claim keeps a citation edge.** A0 removes the human from the sentence, not from the record. Release notes and post-mortem entries carry `(#4128, @author)` or `ADR-021` so the claim stays assignable while the sentence stays portable. This is the citation mechanic in engineering form: portable sentence + resolvable pointer.
12. **Replace pronouns with roles, and let the test be substitution.** Substitute the actual referent for every pronoun and read the result aloud: *`I usually flush first`* → *`<author's name> usually flushes first`* is a personal diary entry (wrong voice for a runbook); *`Dave deployed the config`* is a blame sentence (wrong voice for a post-mortem); *`the reconciler flushes on min(interval, 512)`* survives substitution unchanged (correct A0).
13. **Make mode switches visible at structural boundaries.** If an artifact mixes A0 and A1, the switch must align with a section: Context / Evidence / Timeline / Behaviour → A0; **Decision / Proposal / Who owns this / What I accept** → A1. Mixed voices inside one paragraph read as evasion, because that is what mixture usually is.
14. **Escalate a voice problem into a structure problem.** If a runbook step needs first person to make sense, the step is undocumented tacit knowledge: convert it into a checkable command with expected output, or remove it from the path. If a decision needs an apology or a hedge to be acceptably owned, the tradeoff is not yet understood — measure it, then own it.

### 2.1 Voice map: artifact → required mode

| Artifact | Primary mode | What must be A1 (owned) | What must be A0 (agentless) |
|---|---|---|---|
| PR description | **A1 lead, A0 body** | What I propose, what cost I accept, what I will do if it breaks | Diff behaviour, evidence table, verification command, blast radius |
| PR / commit subject | A0 | — | Imperative system change (`flush on min(interval, 512 records)`) |
| Code comment / docstring | A0 | — | Invariant, external constraint, version, cost of removal |
| Review comment | **A1 verdict, A0 evidence** | My verdict, my question, the reason I am blocking | `path:line` mechanism, test output, failing case |
| ADR / RFC | **A1 decision, A0 context** | The decision, its acceptance, its owner role after merge | Context, preconditions, evidence, alternatives, bounds |
| Runbook | A0 | — | Every step, expected output, failure handling; escalation = rotation + queue + target |
| Post-mortem | A0 | Action-item owners (role + date), if a person made a deliberate call, name the *role* | Timeline, impact, root cause, missing guard, contributing factors |
| Release notes | A0 | Attribution edge only (`#4128`, `@author`) | Effect on the reader's system, version range, migration requirement |

### 2.2 Transformation table: anti-patterns and clean replacements

| Anti-pattern | Voice defect | Clean replacement |
|---|---|---|
| `It was decided to cap retries at 3.` | Ownership hole in a decision | `I proposed the cap of 3 to bound the CALLER's 2s deadline; I accept that a slow attempt now consumes the budget instead of stacking. Owner after merge: `gateway-primary` (ADR-021).` |
| `We should have known the queue would blow up.` | Blame leak with an unbounded *we* | `The queue had no bound on in-flight records: the trigger was time-only, so a burst converted a latency guarantee into an unbounded buffer (`batcher.go:88`, v2.3.1).` |
| `Dave deployed the config without the migration check.` | Blame leak — person in the subject of a defect | `The config rollout began 03:02 under runbook RB-14, which does not gate on migration state. `migrate-check` is not a required status check on `config-service`. Actor: `deploy-bot`. The absent guard is the cause.` |
| `I usually flush the buffer before compacting.` | Referent decay in a procedure | `Flush before compaction so a flushed row is never rewritten in the same tick. Verify with `batch_flush_watermark=512` in the startup log.` |
| `You should check the logs if it looks stuck.` | `you` + unaddressed ambiguity | `kubectl -n payments logs -l app=reconciler --since=10m` → expect one `reconcile: scanned=<n>` line per minute; zero lines means the worker is not consuming. Ambig budget: 0.` |
| `Improvements were made to performance.` | Agentless pile; no reader mapping, no citation edge | ``reconciler` write lag p99: 3,140 → 610 ms (`#4128`, `@<author>`). Trigger changed to `min(interval, 512 records)`. No migration; affects self-hosted `>= 2.4.0`.`` |
| `// I had to add this retry because the gateway kept flaking.` | First person narrating a mechanism | `// Retries bound the CALLER's 2s deadline, not downstream health. 3 x 400ms backoff + 2 x 200ms jitter = 1.6s worst case; the deadline check runs before each sleep. Do not raise the cap without re-running budget_test.go:88.` |
| `We agreed in standup that compaction runs first.` | Conversational provenance + unattributable *we* | `The ordering invariant is recorded in ADR-021 §Decision: compaction is awaited before the flush, so a flushed row is never rewritten in the same tick.` |
| `This looks rushed — please slow down.` | Person-facing verdict with no mechanism | ``batcher.go:88` flushes before `compactor.Run()` is awaited, so a batch can be flushed mid-compaction. I'd block on that one question. Everything else LGTM.` |
| `Mistakes were made during the incident.` | Agent-hiding passive; zero system content | `During 03:02–03:41 the deploy path applied a config schema that `reconciler` could not parse; the worker crashlooped and reconciled 0 records for 39 minutes.` |
| `Action: add a check.` | Action item with no owner, no date, no verification | `Action A2: add `migrate-check` as a required status check on `config-service`. Owner role: `platform-primary`. Due 2026-10-01. Verified by pasting `gh api repos/.../branch-protection` output into appendix C.` |
| `We chose Postgres because it's more robust.` | Unbounded *we* + unfalsifiable claim | `I chose Postgres for the audit path (`audit-writer`); the alternative (append-only files on the same volume) fails the durability requirement under host loss, measured in chaos run DB-11. Reversal trigger: p99 write latency > 40 ms at 2k writes/s for a week — watched by `storage-primary`.` |
| `TODO: clean up the legacy path.` | Ownerless intent | `TODO(schema-v3, owner role platform-primary): delete the legacy path once all tenants are migrated; tracked in ISSUE-1043.` |
| `The team feels this is the right tradeoff.` | Consensus substitute for attribution | ``platform-primary` accepted the +40% write-transaction cost on 2026-09-17 (comment on `#4128`) in exchange for a bounded buffer.` |
| Runbook: `I escalate to the platform team.` | Personal escalation route with a decaying referent | `If step 3 has not cleared within 15 min, page `platform-primary` (rotation `platform-primary`, 15 min target response). Do not DM individual engineers.` |

### 2.3 Probes

| Probe | Question put to the artifact | Pass condition |
|---|---|---|
| Substitution | Replace every `I`/`we`/`you` with its actual referent and read it aloud | No sentence becomes a personal diary entry (wrong for procedure) or a blame sentence (wrong for incident text) |
| Decision owner | For each decision statement, who is in the subject position? | A person or a role with a queue; never an unbounded *we*, never nobody |
| Mechanism subject | For each defect or behaviour statement, is a system noun the subject? | Component, config, version, job, or guard named; no person; no null subject |
| Reproducibility | Can a reader who is not the author run or falsify this line? | Command, expected output, and the version it was verified against are present |
| Citation edge | Does each agentless claim point at something assignable? | PR number, commit SHA, issue, or ADR ID is attached |
| Mode boundary | Where does the artifact switch between A0 and A1? | The switch coincides with a section heading, not a mid-paragraph drift |
| Blamelessness vs usefulness | Does the report name the mechanism, or has it gone vague to be safe? | Failure path is fully specified *and* contains zero human subjects |

### 2.4 Failure diagnostics

| Symptom | Voice diagnosis | Fix |
|---|---|---|
| A constraint is reverted by a new owner who "didn't know why it was there" | Ownership hole: the tradeoff was never assigned | Re-state as `I proposed X; I accept cost Y; owner role Z` and cite the ADR |
| Two quarters later the same question is re-argued from scratch | Agentless pile with no citation edge | Attach the PR/ADR pointer; the sentence stays portable, the claim becomes assignable |
| Post-mortem revised three times, and each revision is vaguer | Blame leak: writing is optimised for safety instead of for the mechanism | Move every human out of the subject position and every system component in |
| Runbook step skipped or improvised under pressure | Referent decay: the step was addressed to a person, not written as a procedure | Imperative step, expected output, and the stop/retry/rollback decision |
| Reviewer and author trade tone-comments instead of mechanisms | Missing A1 verdict + A0 evidence split in the review comment | Reviewer owns the verdict in first person, states the mechanism agentlessly |
| Release notes read fine and help nobody | No reader mapping, no version range, no attribution edge | Rewrite as effect + version range + `#PR`/`@author` |
| "Who owns this?" asked of a merged decision | *We* carried the ownership; the *we* has since been reorganised | Attribute to a role with a queue, re-checked on an interval |
| Owned proposals read as defensive or apologetic | A1 applied to work narration and to hesitations, not to the choice | Keep first person for the choice and its accepted cost only; delete the process diary |

**Related dispatchers.** Kill the referent for good with the [Silent Author Test](../../kirby-fitzpatrick-silent-author-test/SKILL.md); strip apologetic self-narration and patronising diff tone with the [Anti-Author-Splain Commenter](../../kirby-fitzpatrick-anti-author-splain-commenter/SKILL.md); stop hedging an owned decision with the [Calibrated Technical Tone](../../kirby-fitzpatrick-calibrated-technical-tone/SKILL.md); frame the owned decision as Status Quo → Instability → Resolution with the [3-Part Proposal Engine](../../kirby-fitzpatrick-3part-proposal-engine/SKILL.md); keep first person from becoming revenge prose in threads with the [Letter of Response Reviewer](../../kirby-fitzpatrick-letter-of-response-reviewer/SKILL.md); audit the agentless half from a stranger's seat with the [Cold Reader PR Auditor](../../kirby-fitzpatrick-cold-reader-pr-auditor/SKILL.md); separate attributed evidence from opinion with the [Fact vs Judgment Classifier](../../kirby-fitzpatrick-fact-vs-judgment-classifier/SKILL.md); and address the A0 reader who arrived with no context using [Beneficiary-First Docs](../../kirby-fitzpatrick-beneficiary-first-docs/SKILL.md).

---

## 3. Engineering Application Scenarios

### 3.1 Code Reviews — the split between the verdict and the mechanism

A review comment is itself a bipartite artifact, and conflating its two halves is what turns reviews into tone arguments. The **verdict is A1** — a named human accepts responsibility for blocking, approving, or asking: `I'd block on this one question`. The **mechanism is A0** — the code, the race, the missing await, the failing test, in the subject position, with `path:line` and output. Reviewers who write both halves in first person produce a diary; reviewers who write both halves agentlessly produce an opinion with no owner and a mechanism with no verdict.

```markdown
<!-- BEFORE — blame leak + conversational provenance + unowned verdict -->
Why did you do it this way? We agreed in standup that compaction should run
before the flush. This looks rushed — please slow down.
```

```markdown
<!-- AFTER — A0 mechanism, A1 verdict, no standup dependency, no person in subject -->
`batcher.go:88` calls `flush()` before `compactor.Run()` is awaited, so a batch can
be flushed mid-compaction and rewritten in the same tick (reproduced with
`make test-batcher RACE=1`, TSAN report in the build log).

The ordering invariant — compaction is awaited before the flush — is recorded in
ADR-021 §Decision; the flushed-row guarantee in §Preconditions depends on it.

I read the deferral as deliberate; is there a constraint I'm missing? I'd block on
this one question. Two nits below are non-blocking.
```

The same split governs the code being reviewed. Comments and docstrings are **A0 without exception**: their reader is repeating or debugging behaviour, never looking for who typed the line. First person in a comment is almost always the author explaining themselves to a reviewer who is present, which is precisely the information that will be missing when that reviewer is gone.

```go
// BEFORE — a diary where the artifact should be a contract
// I added this retry because the gateway kept flaking on us.
// We should probably revisit this later.
// It was decided that the cap is 3.
// The ledger is reconciled by the reconciler job every 2s.
```

```go
// AFTER — system subject, real invariant, owned decision recorded elsewhere,
// dated verification, and a citation edge back to the decision record
//
// RetryPolicy bounds the CALLER's 2s deadline, not downstream health. 3 attempts x
// 400ms backoff + 2 x 200ms jitter = 1.6s worst case; the deadline check runs
// before each sleep, so a slow attempt consumes the budget instead of stacking
// retries on top of it. The cap of 3 is a decision, not a measurement: it is the
// largest value that still fits the caller budget (ADR-021, owner role
// gateway-primary). Do not raise it without re-running budget_test.go:88.
// verified 2025-09-17 against gateway/timeouts.go:14 (caller budget 2s).
//
// The reconciler job reconciles the ledger every 2s; a slow drain buffers records
// in memory. Bounded by flush_watermark (see batcher.go:88).
```

A PR that touches a runbook, a post-mortem, or release notes is reviewing **three artifacts with three different voice budgets**, and the diff view hides that. Runbook and post-mortem diffs are the highest-yield voice audits in a codebase, because they are the only place where a burned-out author most often writes first person (`I usually just restart the worker`), and they are read on the worst day of someone else's quarter.

```markdown
<!-- BEFORE — runbook step written to a person who is not on the rotation -->
4. If the backlog looks big, I usually restart the worker pod and watch it drain.
   You should check with the platform team if it doesn't clear up.
```

```markdown
<!-- AFTER — imperative procedure, checkable output, role-routed escalation -->
4. Restart the worker to drain the backlog:
   `kubectl -n payments rollout restart deploy/reconciler`
   Expect: `reconcile: scanned=<n>` once per minute, backlog decreasing within 2 min.
   If the backlog has not decreased after 15 min, page `platform-primary`
   (rotation `platform-primary`, 15 min target). Do not DM individual engineers.
```

### 3.2 PR Descriptions — owned proposal, agentless proof

The PR body is the one artifact read *both* by someone deciding whether to accept a tradeoff **and** by someone re-running the evidence or reverting at 03:00. Attempting one voice for both produces the two standard failures: the defensive diary (`I tried a few things and this seemed to work`) and the laundered decision (`the flush trigger was changed to bound the buffer`). The bipartite body owns the choice and dehumanises the mechanism.

| PR section | Mode | Why |
|---|---|---|
| What I propose / what I accept | **A1** | A proposal without a proposer cannot be accepted, rejected, or defended after merge |
| What changed (mechanism) | A0 | System subject; reproducible by a stranger |
| How to verify | A0 | Command + expected output + baseline; the agent is noise |
| Evidence table | A0 | Measurements, with the citation edge (`#4128`) |
| Blast radius and rollback | **A1 → A0** | `I will revert <sha> if X` (committed promise) + agentless statement of what readers see |
| What would reverse this | A0 | Signal, threshold, and the role that watches it |
| Does not cover | A0 | Explicit outer bounds, in the system's own vocabulary |

```markdown
## What I propose
Flush on `min(interval, 512 records)` in `batcher.go:88` instead of on the ticker
alone. I accept the +40% write-transaction cost that comes with smaller batches;
I judge a bounded buffer worth it on the payment path, where an unbounded buffer
turns a latency guarantee into an OOM. If the storage cost signal below fires, I
will revert this and reopen the batching design (#4128).

## What changed
A flush trigger is the durability contract. With a time-only trigger the window is
bounded in seconds and unbounded in records, so a burst converts a latency
guarantee into an unbounded buffer. The trigger now depends on the one quantity
the writer controls: its own buffer. Compaction is unchanged and still awaited
after the flush (ADR-021 §Decision).

## How to verify
`make bench-batcher WORKLOAD=burst-4k RUNS=5` → expect p99 write lag < 800 ms and
peak rows buffered == 512 (before: 3,140 ms / 12,480 rows on main @ 9f21c0d).

| Metric | main @ 9f21c0d | PR head | Δ |
|---|---|---|---|
| p99 write lag | 3,140 ms | 610 ms | −81% |
| peak rows buffered | 12,480 | 512 | −96% |
| write txn / 10k rows | 42 | 59 | +40% (accepted above) |

## Blast radius and rollback
The write path of `reconciler` only; readers see smaller batches, never partial
ones. Revert is one commit (`git revert <sha>`); no schema, config, or feature-flag
change is involved. The startup banner `batch_flush_watermark=512` disappears when
the revert is live.

## What would reverse this
`write_txn_per_row` on the `reconciler` dashboard above 1.5x baseline for a full
week. Watched by `storage-primary`.

## Does not cover
Compaction scheduling and the read path. The watermark is not a throughput dial.
```

Release notes, assembled from the same change, are pure A0 with an attribution edge — no ownership sentences at all, because the ownership already lives one hop away in the PR and the ADR:

```markdown
<!-- BEFORE — passive pile, no reader mapping, nothing to cite -->
## v2.5.0
- Improved performance across the ingestion path.
- Made things more robust. Thanks to everyone involved!

<!-- AFTER — effect, version range, citation edges, zero pronouns -->
## v2.5.0
- `reconciler` write lag p99: 3,140 ms → 610 ms under burst writes (`#4128`, `@<author>`).
  The flush trigger is now `min(interval, 512 records)`; no migration is required.
  Applies to self-hosted deployments on `>= 2.4.0`. Decision record: ADR-021.
- Compaction scheduling is unchanged. This release does not affect the read path.
```

Note the shape of the fix: the reader maps the change onto their own system, and the claim stays assignable (`#4128`, `ADR-021`) without a single human pronoun in the sentence. That is the citation mechanic operating inside a changelog.

### 3.3 Architecture RFCs / ADRs — decisions that must survive their author

An ADR is a bet that a future maintainer can re-decide without re-litigating with someone who has left. Its bipartite structure is fixed: **the context, the preconditions, the evidence, and the bounds are A0** (they describe the system), while **the decision and its accepted cost are A1** (they describe a choice someone made and stands behind). The usual ADR failure is the exact inverse of the usual post-mortem failure — the ADR hides the chooser (`it was decided that…`, `the team favours…`), while the post-mortem hides the mechanism (`mistakes were made`). Both are fixed by naming the right entity.

| ADR section | Mode | Obligation |
|---|---|---|
| Context | A0 | The system property or invariant at stake, in the system's own vocabulary — no codenames, no *we* |
| Decision | **A1** | Who chose, what exactly was chosen, what cost was accepted, in one falsifiable sentence |
| Rationale | A0 | Why this mechanism works; why the rejected alternatives do not |
| Preconditions & environment | A0 | Scale band, topology, dependency versions, state that must hold |
| Evidence | A0 | Benchmarks/traces with command, baseline, workload, and a citation edge |
| Reversal trigger | A0 + role | Signal, threshold, and the role that watches it |
| Ownership after merge | **A1 → role** | The role + queue that owns the decision; never the author's name |
| Re-evaluation | A0 | Interval, trigger, and the artifact that forces the check (upgrade, incident, load test) |
| Does not cover | A0 | Outer bounds, so no reader extends the decision into unexamined territory |

```markdown
# ADR-021 — Watermark-bounded write batching

## Context
A batcher's flush trigger is its durability contract: a time-only trigger bounds
latency in seconds and leaves record count unbounded.

## Decision
I chose `min(interval, 512 records)` in `batcher.go:88`, accepting a +40%
write-transaction cost, because an unbounded buffer converts a latency guarantee
into an OOM on the payment path. Owner after merge: `gateway-primary`
(queue `#gateway`, target response 1 business day). Author of this ADR is not the
owner of this decision.

## Preconditions
Valid for the current single-writer partition assignment and datasets <= 10M rows
per tenant. Not valid under multi-writer assignment: the watermark then bounds one
writer's buffer, not the dataset's in-flight records.

## Evidence
`make bench-batcher WORKLOAD=burst-4k RUNS=5` · main @ 9f21c0d vs PR head ·
p99 lag 3,140 → 610 ms · peak rows 12,480 → 512 · txn/row +40% (accepted cost
above, #4128).

## Reversal trigger
`write_txn_per_row` > 1.5x baseline for one week, or storage cost per transaction
rising past the measured +40%. Watched by `storage-primary`; reviewed each quarter
by the platform role, not by the ADR author.

## Does not cover
Compaction scheduling and the read path. The watermark is not a throughput dial.
```

Post-mortems pair with ADRs and inherit the strictest budget of all: **zero human subjects, zero null subjects**. The report must be simultaneously blameless and complete — which is only achievable by putting the mechanism, the config, the version, and the missing guard in the subject position, and routing action items to roles with dates and verification artifacts.

```markdown
<!-- BEFORE — blame leak in the timeline, ownership hole in the action items -->
## Timeline
03:02 — Dave deployed the config without running the migration check.
03:10 — the worker started crashing. We should have gated this.
## Actions
- Add a check. Be more careful with config rollouts in future.

<!-- AFTER — mechanism as subject, role-routed owners, verification artifact -->
## Timeline
03:02 — the config rollout began (`deploy-bot`, runbook RB-14).
03:10 — `reconciler` crashlooped: the applied schema was unparseable by
        `config-service` v2.3.1. Reconciled records for 39 min: 0.
03:41 — rollout reverted; `reconciler` recovered without intervention.

## Root cause
RB-14 does not gate on migration state, and `migrate-check` is not a required
status check on `config-service`. No person is the cause: the deploy path accepted
a rollout whose precondition was unverified.

## Actions
- A2 — make `migrate-check` a required status check on `config-service`.
  Owner role: `platform-primary`. Due 2026-10-01.
  Verified by: `gh api repos/.../branch-protection` output in appendix C.
- A3 — add a migration-state precondition to RB-14 step 2, with expected output.
  Owner role: `payments-oncall`. Due 2026-09-26.
```

The same standard governs the runbook sitting beside the ADR, with one added clause: its first section is scope and its last is what it does not cover, because an on-call engineer needs to know in ten seconds whether this page is theirs. Escalation is always a route — rotation, queue, target response — never a name; and a runbook counts as verified only once it has been executed cold, in staging, by someone who is not its author, with the pronouns and the referent decay already removed.

---

## 4. Verification Checklist

- [ ] **Every decision has exactly one accountable owner.** Each decision, proposal, and accepted cost names a person or a role with a queue — no unbounded *we*, no *it was decided*, no *the team favours*. The ownership survives reviewer and author turnover, and the accepted-cost sentence is present, not implied.
- [ ] **Every defect and behaviour statement has a system subject.** Post-mortems, timelines, and runbooks name the component, config, version, job, or absent guard. No person appears in the subject position of a failure, and no failure sentence has a null subject (*something went wrong*, *mistakes were made*, *concerns were raised*).
- [ ] **Zero human pronouns in procedures.** Runbooks, timelines, release-note bodies, and code comments contain no `I`/`me`/`my`/`you`/`your`; substituted referents have been tested against the actual reader (author's name, on-call rotation) and no sentence degrades into a diary entry or a blame sentence.
- [ ] **Mode switches coincide with section boundaries.** Each A0/A1 transition inside an artifact aligns with a heading (Context, Evidence, Timeline, Behaviour → A0; Decision, Proposal, Ownership → A1). No paragraph mixes the two modes.
- [ ] **Every agentless claim keeps a citation edge, and every procedure is reproducible cold.** Each A0 claim points at a PR number, commit SHA, issue, or ADR ID; each procedural step carries a command and expected output, was executed by a reader who is not its author, and states what it does not cover.