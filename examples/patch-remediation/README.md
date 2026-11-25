# Synthetic patch remediation proof

This example connects an installation success record, a file transition matching a patch manifest, and a change in applicability of a named vulnerability. The records share a patch, host, installation event, component and before/after observation windows. All example records are synthetic. The prototype predicates and two applicability rules in CombinedPatchFixture.kif are local to this example.

Phase II would need four kinds of evidence:

| Record | Fields |
| --- | --- |
| Patch manifest | Stable patch artifact ID, component ID, canonical target path, digest algorithm, expected old and new digests, and manifest provenance. |
| Installation event | Event ID, patch ID, host ID, observed success status, status provenance, and execution interval. |
| Host observations | Before/after snapshot IDs, host and component IDs, times, path, observed digests, active build, relevant configuration, and capture provenance. File and runtime evidence need a common snapshot or justified window. |
| Applicability criterion | Vulnerability ID, affected component and environment, affected and fixed build IDs, and a pinned source for the criterion. |

The first-order conjecture checks the joins and the before/after condition. Installer status and observations are assertions, while the rules derive applicability at each snapshot. The result is scoped to this case and does not establish causation or future protection.

## Run the example

From this directory, with sigma-rs 2.3.0 and standalone Vampire 5.0.1 available on PATH:

```sh
sumo --config config.xml --no-db --ugly \
  -f ScopeTaxonomy.kif -f CombinedPatchFixture.kif \
  test PC01_combined_patch_evidence.kif.tq \
  --backend subprocess --vampire vampire --lang fof --timeout 30 \
  --proof tptp --want-proof true --keep PC01.p
```

Expect the raw SZS verdict Theorem. ScopeTaxonomy.kif contains two formulas from Merge.kif and ComputingBrands.kif at SUMO commit 28851d64f51ed74339aa9d1397bab7dc0117bfa5. The example does not load the full ontology. A separate model check of the scoped premises without the conjecture returned Satisfiable.
