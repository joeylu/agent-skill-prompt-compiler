# Failure Rules

Block when any of the following holds:
1. no authoritative entry surface can be resolved
2. the supplied bundle mixes multiple skills and the active one cannot be resolved
3. the source bundle lacks enough material to reconstruct runtime-critical behavior
4. policy requires blocking and a critical allowed file is missing
5. schema flattening or example compaction would reduce semantic fidelity
6. runtime closure would remain open because a runtime-called template, runtime asset, or shared algorithm is still external
7. the caller requests a runtime artifact form that cannot be emitted safely
8. a deterministic mapping, fallback cascade, selection heuristic, score band, or derived-field rule that affects execution cannot be preserved without ambiguity

Do not block only because `# Reference Map` is missing.
If `# Reference Map` is missing or unparseable, switch to fallback reference-chain discovery and report the downgrade.

When blocked:
- state the exact missing or conflicting signal
- list the consumed files
- state why semantic fidelity could not be preserved
- do not fabricate missing source content
