# L4Wall 0.2.0

This file is in the disposable validation repository and only exercises the L4Wall Stable channel, PEP 440 version mapping, provenance attestations, and repository-level `latest` preservation. It is not a formal L4Wall release.

Release date: 2026-11-03

L4Wall is the standalone TCP L4 protection gateway in the HighWall repository. It provides connection admission, concurrency and rate limits, timeouts, byte caps, behavioral scoring, bans, and audit records. Version `0.1.0` is now published under its own tag and GitHub Release instead of being bundled into a HighWall release.

## Release provenance

- This exercise rebuilds the L4Wall `0.2.0` wheel and sdist from the current validation commit.
- Every release asset has GitHub build provenance and is verified with `gh attestation verify` in the workflow.
- The release uses the standalone `l4wall-v0.2.0` tag and contains no HighWall, L5Wall, or native-core binaries.
- The repository's previous `latest` release must not move. If GitHub moves it automatically, the workflow restores and rechecks it.

## Artifacts

- `l4wall-0.2.0-py3-none-any.whl`
- `l4wall-0.2.0.tar.gz`
- `SHA256SUMS` (these two L4Wall files only)

Use the downloaded `SHA256SUMS` for digest verification.

The canonical download location for this standalone release is the **Assets** section of this release. The historical `v0.11.0` page retains the original L4Wall attachments only for old links. L4Wall has no GHCR image, PyPI package, or other download channel, and its packages do not contain HighWall, L5Wall, or native-core binaries.

## Status

- Version: `0.2.0`
- Maturity: Stable release-path exercise only; not independently audited and not production-ready
- Runtime: Python 3.10+, standard library only
- Coverage: connection-level TCP protection; no TLS or HTTP payload inspection
- Audit and production readiness: not independently audited and not production-ready

This exercise runs the Python 3.10-3.15 test matrix, static checks, clean virtual-environment installation, CLI version checks, artifact hashes, and provenance verification.

## Install

Download all three assets, verify them, and install the wheel:

```bash
sha256sum -c SHA256SUMS
python3 -m venv /opt/l4wall
/opt/l4wall/bin/pip install --no-cache-dir ./l4wall-0.2.0-py3-none-any.whl
/opt/l4wall/bin/l4wall --version
/opt/l4wall/bin/l4wall init --path ./config.json
/opt/l4wall/bin/l4wall check --config ./config.json
```

Read the [L4Wall README](https://github.com/Fallen-leaves089/highwall/blob/main/subprojects/l4wall/README.md), [deployment baseline](https://github.com/Fallen-leaves089/highwall/blob/main/subprojects/l4wall/docs/DEPLOYMENT.md), and [threat model](https://github.com/Fallen-leaves089/highwall/blob/main/subprojects/l4wall/docs/THREAT_MODEL.md) before deployment.

## Boundaries

- It does not handle bandwidth DDoS, SYN floods, UDP, QUIC/HTTP3, or carrier-side congestion.
- It does not parse HTTP or provide authentication, authorization, WAF, DLP, TLS termination, or JA3/JA4.
- It protects only explicitly configured listeners. Direct access to HighWall or an upstream bypasses L4Wall; listener addresses and network ACLs must close those paths.
- Shared bans in `0.1.0` are reader-only. Automatic publishing, a controller, conflict merging, and HA are not implemented.
- It cannot protect an already compromised host. If an attacker can modify configuration, audit state, or process memory, the L4 boundary has already failed.

## License

Apache-2.0. See the repository root for the license and NOTICE; future standalone builds will also include both files in the wheel and sdist.
