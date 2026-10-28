# Publicly Supported Contribution Ledger

**Proposed schema, v0.2**

The ledger records which agents have publicly documented contributions to the OpenAI / Hugging Face incident, and how the agents interacted. It has two layers, interaction and task contribution, which are kept separate. "Supported" means supported by the inspected public record. It does not mean independently reproduced by the ledger's author. Reproduction is recorded separately.

Changes from v0.1:
- Evidence is an explicit table, with link tables carrying a relationship type.
- AI assistance is tracked separately from evidence basis.
- "Verified" is replaced by "supported" throughout.
- Independence is assigned per underlying observation.
- Selection bias is stated at roster level.
- An interaction layer is added.

## 1. Record structure

Linked tables. One contribution is not necessarily one milestone, and one milestone may have several contributors.

### Sources

One row per inspected source version.

| Field | Meaning |
| --- | --- |
| source_id | Unique source identifier |
| title | Source title |
| location | Public address or local file reference |
| version | Publication or version identifier; `unknown` if unavailable |
| retrieved_at | Retrieval timestamp |
| content_hash | Hash of captured file, if available |
| source_family_id | Groups reproductions of the same underlying account |
| coverage_limits | Selection, omissions, redactions, or attribution limitations |

### Agents

One row per distinguishable agent identity.

| Field | Meaning |
| --- | --- |
| agent_id | Ledger-local identifier |
| observed_handle | Handle exactly as printed |
| identity_scope | Run, transcript, or source within which identity is established |
| identity_status | `established`, `ambiguous`, or `unresolved` |

Default: do not merge handles. Alias claims are recorded as identity assertions (see link tables). A chart-row identifier is not an agent handle unless a source establishes the correspondence.

### Milestones

One row per distinct task outcome.

| Field | Meaning |
| --- | --- |
| milestone_id | Unique outcome identifier |
| workstream_id | Task or workstream |
| outcome | Narrow statement of what was achieved |
| prior_state | What remained unresolved beforehand |
| acceptance_criterion | Observable condition distinguishing success from a claim |
| criterion_origin | `predefined`, `source_stated`, or `analyst_proposed` |
| validation_status | `unverified`, `supported`, `contradicted`, or `unresolved` |
| time_value | Recorded time or interval, or empty |
| time_precision | `exact`, `interval`, `ordering_only`, or `unknown` |

### Contributions (task layer)

One row per attributable action-role-milestone relationship.

| Field | Meaning |
| --- | --- |
| contribution_id | Unique contribution identifier |
| agent_id | Contributor; nullable when attribution is unresolved |
| milestone_id | Linked outcome |
| type | `advance`, `correction`, `integration`, or `enabling` |
| action | Concrete description of what the agent did |
| output_ref | Message, artifact, test, or result identifier |
| production_status | `observed`, `reported`, or `unknown` |
| validation_status | `unverified`, `supported`, `contradicted`, or `unresolved` |
| uptake_status | `documented`, `explicitly_unused`, or `unknown` |
| adjudication | `candidate`, `supported`, `rejected`, or `unresolved` |
| adjudication_reason | Explicit reason for that decision |

Inputs demonstrably used are recorded in `contribution_upstream` (below), not as an array field.

### Interaction events (interaction layer)

One row per interaction event explicitly present in the public material. Aggregate counts cannot substitute for sender-recipient edges, so absent edges stay absent.

| Field | Meaning |
| --- | --- |
| event_id | Unique event identifier |
| sender_agent_id | Sender; nullable when unresolved |
| recipient_scope | `agent`, `broadcast`, or `unknown` |
| recipient_agent_id | Set only when `recipient_scope` is `agent` |
| workstream_id | Task or workstream, if stated |
| event_type | `request`, `assignment`, `reply`, `handover`, `acknowledgment`, `hold`, `conflict_resolution`, or `other` |
| time_value / time_precision | As for milestones |
| reply_to_event_id | Referenced earlier event, if stated |
| channel | Board, inbox/mailbox directory, or other, if stated |

An assignment can be a supported coordination event even if its eventual outcome is unknown. It becomes a supported `enabling` contribution only when downstream use is documented. Brokerage is not progress.

### Evidence

Every evidence reference resolves to a row here.

| Field | Meaning |
| --- | --- |
| evidence_id | Unique evidence identifier |
| source_id | Source |
| locator | Page, paragraph, message ID, timestamp, or artifact path |
| excerpt | Exact supporting passage, where available |
| supported_proposition | Precisely what this evidence establishes |
| evidence_basis | `claim`, `published_action_result`, `investigator_interpretation`, or `reproduction` |
| ai_assistance | `documented`, `explicitly_absent`, or `unknown` |
| analysis_method_locator | Where the method is described, if any |
| independence_group | Groups evidence derived from the same underlying observation; `unknown` where provenance cannot be resolved |
| limitations | Missing context, uncertain attribution, indirect reporting, or conflicting evidence |

Evidence basis is not a numerical confidence score. Several repetitions of one account do not constitute independent confirmation. `ai_assistance` is not an automatic discount: an AI-assisted interpretation and a directly quoted action or result are different evidence objects, even when published in the same report, and a source-wide warning does not classify every excerpt as AI-generated. Independence is assigned per underlying observation, not per publisher. Where provenance cannot be resolved, mark it `unknown` rather than inventing corroboration or dependence.

### Link tables

Each link carries a `relationship`.

| Table | Links | Relationships |
| --- | --- | --- |
| evidence_links | evidence to a contribution, milestone, interaction event, or identity assertion | `supports`, `contradicts`, `attributes`, `validates`, `demonstrates_uptake` |
| contribution_upstream | contribution to the contributions whose output it demonstrably used | (none) |
| identity_assertions | evidence that a handle merges with, or is distinct from, another | `same_as`, `distinct_from` |

Conflicting evidence is representable as a `contradicts` link, not just as a note.

## 2. Admission rules

| Type | Minimum requirement for `supported` |
| --- | --- |
| Advance | Attributable result plus evidence that it satisfies the milestone criterion and was not already present in the recorded prior state. |
| Correction | Identifiable prior error, supporting contrary evidence, and a documented revision, rejection, or changed action. |
| Integration | Identifiable inputs, attributable combination, and validation of a new outcome beyond merely collecting those inputs. |
| Enabling | Attributable output and documented downstream use in a supported contribution. |

Interaction events are admitted when the sender, event type, and evidence are explicit in the public material.

## 3. Counting and missing-data rules

- Count each milestone once, regardless of contributor count.
- Preserve multiple contribution roles without inventing fractional credit.
- Keep unsupported announcements as candidates.
- Record unknowns explicitly; absence of evidence is not evidence of inactivity.
- Do not infer causation from temporal precedence.
- Do not infer token counts from message counts.
- Do not infer "audience" membership from this ledger.
- Preserve unknown recipients; do not assign them.
- Quoted agent counts stay unconfirmed until checked against their precise definitions and source passages.

## 4. Derived roster

For each agent, report:

- Supported contribution IDs, grouped by type.
- Distinct supported milestones and workstreams.
- Interaction events as sender, with recipient scope.
- Evidence of participation across separate milestones.
- Identity and coverage caveats.

"Supported contributor" means at least one supported contribution. "Active core" remains a separate, window-specific classification.

**Roster-level caveat (always attached to the output).** The roster is a lower-bound list of agents with publicly documented contributions, selected through the available reporting. It is not a representative sample, a productivity ranking, or an estimate of the active-core fraction. Most evidence will derive from a small number of underlying accounts, so independence will often be `unknown` or shared.

## 5. Known limits of the public record

- The channel was a shared board, so most interaction events will have `recipient_scope = broadcast`. Directed edges are expected mainly where an inbox or mailbox convention is documented.
- `workstream_counts.csv` in the recovered tables is aggregate data. It supports sender-to-workstream analysis but not sender-recipient edges.
