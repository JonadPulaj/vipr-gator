## vipr-gator — Why3 formalization

A [Why3](https://www.why3.org/) formalization of the VIPR certificate checker from the paper [*“SMT for Verifying MILP Certificates”*](https://arxiv.org/pdf/2312.10420). 

**Formalization Summary.**
The development formalizes VIPR certificates and their rules (feasibility, domination, rounding, disjunction/unsplitting, and per-reason derivations), and defines **semantic** (`valid_*`) and **proof-oriented** (`phi_*`) predicates that we prove **equivalent**. Our main theorem shows that for any well-formed certificate (`is_cert`), `valid cert ↔ phi cert`. 

**Scope.** 
The proofs cover the *core logical correctness* of the checker rules. Implementation aspects such as parsing, file I/O, and CLI plumbing are *currently not* modeled.

**Relation to the paper.**  
This formalization closely follows the proofs in the paper: informal arguments are mechanized as Why3 lemmas or goals. The per-component results (feasibility, solution bounds, domination, rounding, disjunction/unsplitting, and per-reason derivations) are mirrored one-to-one via lemmas such as `LemmaFEAS`, `LemmaSOL`, `LemmaDOM`, `LemmaDIS`, `LemmaLin`, `LemmaRND`, `LemmaUNS`, and the aggregated `LemmaDER`. The main equivalence result is captured as `MainTheorem` (`is_cert cert -> valid cert <-> phi cert`).

## Repo contents

- **`viper_cert.why`** — main development:
  - Certificate model: `VIPRCertificate`
  - Helpers & operators: `VIPRHelpers`
  - Semantic predicates: `VIPRValidity`
  - Spec-style predicates: `VIPRPredicate`
  - Main equivalence lemmas & helpers: `Main`
- **`viper_cert.html`** — HTML rendering of the above.
- **`why3session.xml`** — Why3 session (prover results, tasks).
- **`why3session.html`** — HTML rendering of Why3 session.
- **`why3shapes.gz`** — shape information to keep session mapping stable.
- **`viper_cert_trimmed.mlw`** — pretty print version, no helper predicates/lemmas.

**Readability & surveyability.**
`viper_cert.why` contains ~100 lemmas; ~90 are helper lemmas to guide [Why3](https://www.why3.org/) and can be skimmed, or ignored. To ease reading, `viper_cert_trimmed.mlw` elides most auxiliary lemmas. In `viper_cert.why`, the main lemmas are marked with conspicuous comments.

### Environment & Reproducibility

The formalization is OS-independent, but **proof replay depends on the toolchain** (Why3 and SMT solver versions, options, and resource limits). We provide a known-good setup and the [Why3](https://www.why3.org/) session so results are reproducible with the same environment.

Known-good setup:
- [Why3](https://www.why3.org/doc/install.html): 1.7.2
- Provers: Alt-Ergo 2.5.4, Alt-Ergo 2.5.4 (BV), CVC5 1.3.0, CVC5 1.3.0 (strings), Z3 4.15.2 (noBV)
- Machine: MacBook Pro, Apple M1 Max, 64 GB RAM, macOS Sequoia 15.5

Check your setup, get the repo and replay proof (troubleshooting if needed):
```bash
why3 --version
why3 config list-provers

# Clone and enter the repo
git clone https://github.com/JonadPulaj/vipr-gator vipr-gator
cd vipr-gator

# Replay all proofs deterministically
why3 replay why3session.xml

# (If something fails in your environment)
why3 ide viper_cert.why

```
**Proof automation.**  
All goals are proved and fully replayable. Proofs are **semi-automatic**: most obligations close after standard [Why3 transformations](https://www.why3.org/doc/technical.html) (e.g., `unfold`, `split_goal_full`/`split_all_full`/`split_goal_right`, and occasional `clear_but`, `introduce_premises`, `induction`), after which SMT backends (Alt-Ergo, CVC5, Z3) discharge the resulting subgoals. All the applied transformations and solver calls are fully captured in `why3session.xml`, so `why3 replay why3session.xml` reproduces everything deterministically.
> Note: keep `why3session.xml` and `why3shapes.gz` in the same directory to correctly replay the proof. If in your current set-up running `why3 replay why3session.xml` fails to discharge all proof obligations you can launch the Why3 GUI by running `why3 ide viper_cert.why`. Then readjust time limits and manually replay the proof by relying on `why3session.html` for guidance on helpful transformations for failing goals.

**Provers session statistics** (attempts, successes, and time in seconds):

| Prover                   | Attempts | Successes | Min (s) | Max (s) | Avg (s) |
|--------------------------|:--------:|:---------:|--------:|--------:|--------:|
| Alt-Ergo 2.5.4           |   147    |    147    |   0.01  |  81.61  |   2.68  |
| Alt-Ergo 2.5.4 (BV)      |     2    |     2     |  15.09  |  45.39  |  30.24  |
| CVC5 1.3.0               |  1637    |   1637    |   0.02  |  25.15  |   0.34  |
| CVC5 1.3.0 (strings)     |    68    |     68    |   0.01  |  24.08  |   1.61  |
| Z3 4.15.2 (noBV)         |     6    |     6     |   0.16  |   0.19  |   0.18  |

> Note: counts are **proof attempts** (not unique goals). All attempts above succeeded.
