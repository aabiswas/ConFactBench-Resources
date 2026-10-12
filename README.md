# ConFactBench: A Diagnostic Benchmark for Context-Dependent Fact Verification

**ConFactBench** is a benchmark for evaluating context-dependent fact verification in language models.

The benchmark keeps a factual claim fixed while varying an explicit context such as a software version, time, year, or Nobel Prize category. The model must determine whether the claim is **TRUE** or **FALSE** under the specified context.

ConFactBench contains **3,833 instances in 1,486 claim groups** across four datasets: Programming, Political, Sports, and Nobel Prize.

This repository provides the dataset accompanying the paper **“ConFactBench: A Diagnostic Benchmark for Context-Dependent Fact Verification.”**

## Dataset Statistics

| Dataset | Instances | Claim Groups | Context |
|---|---:|---:|---|
| Programming | 292 | 146 | `version` |
| Political | 490 | 245 | `time` |
| Sports | 468 | 234 | `year` |
| Nobel Prize | 2,583 | 861 | `awarded_year`, `awarded_prize_category` |
| **Total** | **3,833** | **1,486** | — |

## Datasets

### Programming

Evaluates whether a software-related claim is valid for a specified software version.

**Example**

Claim: "Python dict provides the iteritems() method."

| Version | Label |
|---|---|
| 2.5.6 | TRUE |
| 3.9.13 | FALSE |

### Political

Evaluates whether a person held a specified political office at a given time.

**Example**

Claim: "Barack Obama held the office of President of the United States."

| Time | Label |
|---|---|
| 2012-08 | TRUE |
| 2002-05 | FALSE |

### Sports

Evaluates whether a player represented a specified football club during a given year.

**Example**

Claim: "Zinedine Zidane played for Real Madrid."

| Year | Label |
|---|---|
| 2003 | TRUE |
| 1994 | FALSE |

### Nobel Prize

Evaluates Nobel Prize claims jointly using award year and prize category.

**Example**

Claim: "Albert Einstein was awarded a Nobel Prize."

| Awarded Year | Prize Category | Label |
|---|---|---|
| 1921 | Physics | TRUE |
| 1920 | Physics | FALSE |
| 1921 | Chemistry | FALSE |

## Data Format

Each dataset is stored in JSONL format.

Each record contains the following fields:

- `id`
- `group_id`
- `claim`
- `context`
- `evidence`
- `label`
- `category`

Labels are encoded as:

- `1` = TRUE
- `0` = FALSE

Instances with the same `group_id` contain the same claim evaluated under different contexts.

The `evidence` field contains supporting factual information used in the with-evidence evaluation setting.

Programming, Political, and Sports contain two instances per claim group, with one TRUE and one FALSE label. Nobel Prize contains three instances per claim group, with one TRUE and two FALSE labels.

## Evaluation Conditions

The accompanying paper evaluates four prompting conditions:

- `zs_closed`: Zero-shot without evidence
- `zs_evidence`: Zero-shot with evidence
- `fs_cot_closed`: Few-shot Chain-of-Thought without evidence
- `fs_cot_evidence`: Few-shot Chain-of-Thought with evidence

Programming, Political, and Sports use four few-shot demonstrations. Nobel Prize uses three demonstrations covering a correct year-category combination, an incorrect year, and an incorrect category.

The experimental prompt templates and evaluation configurations are described in the accompanying paper.

## Evaluation Metrics

The benchmark is evaluated using:

- **ACC** — Instance-level accuracy
- **GCS** — Group Consistency Score
- **MG-F1** — Macro-Averaged Group F1
- **ACC–GCS Gap** — Difference between ACC and GCS

GCS counts a claim group as correct only when all contextual variants in that group are predicted correctly.

MG-F1 measures partial within-group performance by computing macro-averaged F1 over the TRUE and FALSE classes for each claim group and then averaging across groups.

Metric definitions and calculation procedures are provided in the paper.

## Repository Structure

```text
ConFactBench/
├── README.md
└── Dataset/
    ├── programming.jsonl
    ├── political.jsonl
    ├── sports.jsonl
    └── nobel prize.jsonl
```

## Paper

**ConFactBench: A Diagnostic Benchmark for Context-Dependent Fact Verification**
