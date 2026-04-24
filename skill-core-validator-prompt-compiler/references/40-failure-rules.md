# Failure Rules

Block when any of the following holds:
1. no authoritative validator entry surface can be resolved
2. no authoritative target entry surface can be resolved
3. the supplied validator bundle mixes multiple skills and the active one cannot be resolved
4. the supplied target bundle mixes multiple skills and the active one cannot be resolved
5. the validator compiler-facing contract cannot be resolved
6. the target validator-facing contract or adapter contract cannot be resolved
7. the target contract does not provide one or more validator-required target roles
8. the source bundles lack enough material to reconstruct runtime-critical behavior
9. policy requires blocking and a critical collected file is missing
10. schema flattening or example compaction would reduce semantic fidelity
11. runtime closure would remain open because a runtime-called template, runtime asset, shared algorithm, validator contract, or target-side contract surface is still external
12. a deterministic mapping, fallback cascade, selection heuristic, score band, or derived-field rule that affects execution cannot be preserved without ambiguity
13. the requested `transportMode` cannot be emitted safely for the supplied or default runtime envelope
14. one or more required roles would close only by inventing new non-contract documents or new runtime content instead of binding the canonical contract files to existing authoritative files
15. a multi-surface consistency rule cannot be preserved across all compared surfaces and comparison relations
16. a branch-sensitive gate or output-branch rule would be reduced to a loose summary instead of keeping equivalent behavior
17. a nested schema constraint that affects executable validity, output shape, target selection, or output branch would be weakened into a looser checklist
18. selector, identity, uniqueness, ordering, or cardinality semantics that affect target choice or audit scope cannot be preserved explicitly
19. primary-authority versus support-only boundary semantics cannot be preserved safely
20. modality or quantifier semantics such as `exactly`, `only`, `same`, `non-null`, `at most`, or `unique` would be weakened, dropped, or left ambiguous
21. a ready result would claim semantic fidelity without explicit `criticalRuleCoverage` landing evidence for each critical rule class present in the source pair
22. a runtime-critical rule appears only in diagnostics or coverage notes but not in the emitted prompt body
23. a source-side nested schema member requirement that affects gating, selector validity, or output branching is not made explicit in the emitted prompt body
24. one or more runtime-critical invariants would survive only as class-level or topic-level summaries instead of explicit prompt-body rules
25. a prompt-body rule omits one or more concrete surfaces, selectors, containers, nested members, or quantifiers required to preserve the source branch behavior
26. multiple distinct branch-critical rules are merged into one abstract bullet such that invariant-level equivalence cannot be verified

Do not block only because one bundle `# Reference Map` is missing.
If a bundle `# Reference Map` is missing or unparseable, switch that bundle to fallback reference-chain discovery and report the downgrade.

When blocked:
- state the exact missing or conflicting signal
- list the consumed files
- state why semantic fidelity could not be preserved
- do not fabricate missing source content
