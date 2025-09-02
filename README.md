# vipr-gator — Why3 formalization

A [Why3](https://www.why3.org/) formalization of the VIPR certificate checker from the paper [*“SMT for Verifying MILP Certificates”*](https://arxiv.org/pdf/2312.10420). 

The development formalizes VIPR certificates and their rules (feasibility, domination, rounding, disjunction/unsplitting, and per-reason derivations), and defines **semantic** (`valid_*`) and **proof-oriented** (`phi_*`) predicates that we prove **equivalent**. Our main theorem shows that for any well-formed certificate (`is_cert`), `valid cert ↔ phi cert`. 

**Scope.** The proofs cover the *core logical correctness* of the checker rules. Implementation aspects such as parsing, file I/O, and CLI plumbing are *currently not* modeled.

## What’s in this repo

- **`viper_cert.why`** — main development:
  - Certificate model: `VIPRCertificate`
  - Helpers & operators: `VIPRHelpers`
  - Semantic predicates: `VIPRValidity`
  - Spec-style predicates: `VIPRPredicate`
  - Main equivalence lemmas & helpers: `Main`
- **`viper_cert.html`** — HTML rendering of above.
- **`why3session.xml`** — Why3 session (prover results, tasks).
- **`why3session.html`** — HTML rendering of Why3 session.
- **`why3shapes.gz`** — shape information to keep session mapping stable.

**Proof automation.**  
All goals are proved and fully replayable. Proofs are **semi-automatic**: most obligations close after standard Why3 transformations (e.g., `unfold`, `split_goal_full`/`split_all_full`, and occasional `clear_but`), after which SMT backends (Alt-Ergo, CVC5, Z3) discharge the resulting subgoals. The applied transformations and solver calls are recorded in `why3session.xml`, so `why3 replay` reproduces everything deterministically.

**Provers session statistics** (attempts, successes, and time in seconds):

| Prover                   | Attempts | Successes | Min (s) | Max (s) | Avg (s) |
|--------------------------|:--------:|:---------:|--------:|--------:|--------:|
| Alt-Ergo 2.5.4           |   147    |    147    |   0.01  |  81.61  |   2.68  |
| Alt-Ergo 2.5.4 (BV)      |     2    |     2     |  15.09  |  45.39  |  30.24  |
| CVC5 1.3.0               |  1637    |   1637    |   0.02  |  25.15  |   0.34  |
| CVC5 1.3.0 (strings)     |    68    |     68    |   0.01  |  24.08  |   1.61  |
| Z3 4.15.2 (noBV)         |     6    |     6     |   0.16  |   0.19  |   0.18  |

> Note: counts are **proof attempts** (not unique goals). All attempts above succeeded.


**Relation to the paper.**  
This formalization closely follows the proofs in the paper: informal arguments are mechanized as Why3 lemmas or goals. The per-component results (feasibility, solution bounds, domination, rounding, disjunction/unsplitting, and per-reason derivations) are mirrored one-to-one via lemmas such as `LemmaFEAS`, `LemmaSOL`, `LemmaDOM`, `LemmaDIS`, `LemmaLin`, `LemmaRND`, `LemmaUNS`, and the aggregated `LemmaDER`. 
The main equivalence result is captured as `MainTheorem` (`is_cert cert -> valid cert <-> phi cert`).





## Requirements

- [Why3](https://www.why3.org/doc/install.html)
- At least one SMT solver: Z3, CVC5, or Alt-Ergo
- If you want to replay the proof provided here you will
  need Z3, Z3 (noBV), CVC5, CVC5 (strings), and
  Alt-Ergo (BV). See session file for more specifics. Time limits may need to be 
  adjusted based on your machine set-up. 
