# HighWall 0.13.0 Alpha 2

This is a supply-chain rehearsal release in the disposable public validation repository. It exists only to exercise the Alpha channel, PEP 440 package versioning, dual-build reproducibility proof, CycloneDX SBOMs, attestations, and release approval flow. It is not a production HighWall release and adds no capability or production-readiness claim.

Validation focus:

- `v0.13.0-alpha.2` maps to Python package version `0.13.0a2`;
- Linux and Windows native binaries are each built twice from clean state and compared;
- each native target carries a `.buildproof.json` and a `.cdx.json` SBOM;
- every release asset has a build provenance attestation;
- the GitHub Release is published as a pre-release and does not move `latest`.

The formal project acceptance boundary remains unchanged: this version must not be described as production-ready, C4, or independently audited.
