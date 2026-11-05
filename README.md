# Desiderata

**An experimental, inspectable workspace for reasoning about claims, evidence, corrections, and the limits of verification.**

Desiderata is a human-directed research and software experiment. Its present implementation is a **local, headless Python prototype**, not an autonomous scientific researcher, a truth engine, a secure multi-user platform, or a validated agentic template.

It was developed through substantial human-directed collaboration with generative AI systems, including AI-assisted design, code development, criticism, testing, and documentation. The identities of particular systems are not necessary to evaluate the released code. Human authorization governs project goals and policy; executable checks and review receipts provide *bounded* technical evidence. Neither human authorization nor AI output alone establishes scientific correctness.

**Release status:** C3.4 implementation baseline assembled under the Desiderata name; documentation/release preparation only. The five implemented D-07 lifecycle states are approved for bounded local use. Independent human cold-reader acceptance A1 remains open. **No live LLM integration is shipped or authorized; consequential-use `HOLD` remains in force.**

## Read from theory to practice

1. [The collection of short monographs](docs/monographs/INDEX.md): a progressive, editable explanation of the ideas and their limitations.
2. [User manual](docs/manual/USER_MANUAL.md): what Desiderata is and is not, how to run it, what to expect, and how to inspect a result.
3. [Codebase map and functionality roadmap](docs/roadmaps/CODEBASE.md): modules, actual behavior, gaps, and acceptance criteria.
4. [Creators' roadmap](docs/roadmaps/CREATORS.md): editorial, engineering, review, release, and research decisions that remain to be made.
5. [Bibliography and source catalog](docs/references/BIBLIOGRAPHY.md): internal primary sources, technical documentation, and the distinction between citations and evidence.
6. [License and publication policy](docs/governance/LICENSE_AND_PROVENANCE.md): CC0-1.0 scope, inherited notices, source lineage, and the AI-development statement.
7. [Proposed protocol-revision design note](docs/proposals/DN-0001-PROPOSED.md): discussion material, explicitly not accepted or implemented.
8. [Legacy documentation index](docs/governance/HISTORICAL_RECORD.md): preserved RFC, consolidation, traceability, objections, and checkpoint records.

The monographs are *separable Markdown units*, not a frozen book. Their stable identifiers can be edited, criticized, rearranged, or replaced without silently changing the archived research record.

## Quick start (Python 3.11+, supported POSIX local filesystem)

```bash
python -m unittest tests.test_engine tests.test_persistence -v
python tools/run_r0_baseline.py
./bin/desiderata --help
./bin/desiderata init --workspace /tmp/desiderata-example --budget 2
./bin/desiderata goal --workspace /tmp/desiderata-example --goal-id G1 \
  --statement 'Investigate this proposition' --request-id goal-1 --local-operator
./bin/desiderata inspect --workspace /tmp/desiderata-example --json
./bin/desiderata export --workspace /tmp/desiderata-example --archive /tmp/desiderata-example.zip
./bin/desiderata restore --archive /tmp/desiderata-example.zip --workspace /tmp/desiderata-restored
./bin/desiderata verify --workspace /tmp/desiderata-restored --json
```

`bin/desiderata` is a **compatibility alias**, not a rename of the internal `scientific_workspace` Python package or the historical `bin/scientific-workspace` command. Existing scripts and fixtures remain intact.

`--local-operator` **does not authenticate a human**; it is an acknowledgment that the caller is trusted locally. Do not expose it to an agent or untrusted service. The `HOLD` designation is a project policy statement, not a runtime sandbox.

## Evidence and scope

The prior C3.4 release reported **86 passing project tests, one future-C4 test skipped, 44 preserved R0 tests passing, and 5/5 vendored function-local import sites reached** under specified tests. Those results do not verify the full RFC invariant matrix. See [the verification limits](docs/consolidated/VERIFICATION_AND_LIMITS.md) and [C3.4 coverage note](docs/consolidated/C3_FUNCTION_IMPORT_COVERAGE.md). Fresh Desiderata split-run verification is recorded in [`docs/governance/ASSEMBLY_VERIFICATION.md`](docs/governance/ASSEMBLY_VERIFICATION.md), with a machine-readable receipt at `checkpoints/desiderata/ASSEMBLY_RECEIPT.json`.

Proposition 14 is a *known-answer fixture*: its unequal-set formalization permits a finite counterexample, while the disjoint-set interpretation is not refuted by that witness; the **source proposition stays unresolved**. This is evidence for a bounded checker, not general claim interpretation or truth convergence.

## License

Original material in this release is dedicated under **[CC0 1.0 Universal](LICENSE)** to the extent its rightsholders hold the relevant rights. Historical PERSONAL.WIKI code carries its original Unlicense notice, which is preserved; third-party rights, patent rights, and trademark rights are not silently waived. See [license and provenance scope](docs/governance/LICENSE_AND_PROVENANCE.md). Experimental software is supplied without warranty to the extent permitted by law.

No GitHub publication, independent external review, or live LLM operation is claimed by this assembled source checkpoint.
