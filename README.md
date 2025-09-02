# vipr-gator — Why3 formalization

A [Why3](why3.org) formalization of the checker for VIPR certificates in [Satisfiability Modulo Theories for Verifying MILP
Certificates](https://arxiv.org/pdf/2312.10420). Our formalization closely follows the paper above in modelling certificate validity/correctness by defining semantic and proof-oriented 
predicates that are proven equivalent. [Why3](why3.org) discharges all proof obligations thus formally verifying 
the algorithmic correcteness of the proposed checker.
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
