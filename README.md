# vipr-gator — Why3 formalization

A [Why3](https://www.why3.org/) formalization of the VIPR certificate checker from the paper [*“Satisfiability Modulo Theories for Verifying MILP
Certificates”*](https://arxiv.org/pdf/2312.10420).  
The development models VIPR certificates and their rules (feasibility, domination, rounding, disjunction/unsplitting, and per-reason derivations), and defines **semantic** (`valid_*`) and **proof-oriented** (`phi_*`) predicates that we prove **equivalent**. Our main theorem shows that for any well-formed certificate (`is_cert`), `valid cert ↔ phi cert`. All proof obligations are discharged by Why3, and the repository includes the session files so results are **fully reproducible** via `why3 replay`.

**Scope.** The proofs cover the *core logical correctness* of the checker rules. Implementation aspects such as parsing, file I/O, and CLI plumbing are currently *not* modeled.

**Relation to the paper.**  
This formalization closely follows the proofs in the paper: informal arguments are mechanized as Why3 lemmas or goals. The per-component results (feasibility, solution bounds, domination, rounding, disjunction/unsplitting, and per-reason derivations) are mirrored one-to-one via lemmas such as `LemmaFEAS`, `LemmaSOL`, `LemmaDOM`, `LemmaDIS`, `LemmaLin`, `LemmaRND`, `LemmaUNS`, and the aggregated `LemmaDER`. 
The main equivalence result is captured as `MainTheorem` (`is_cert cert -> valid cert <-> phi cert`).


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



## Requirements

- [Why3](https://www.why3.org/doc/install.html)
- At least one SMT solver: Z3, CVC5, or Alt-Ergo
- If you want to replay the proof provided here you will
  need Z3, Z3 (noBV), CVC5, CVC5 (strings), and
  Alt-Ergo (BV). See session file for more specifics. Time limits may need to be 
  adjusted based on your machine set-up. 
