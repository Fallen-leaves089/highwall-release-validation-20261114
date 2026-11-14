# HighWall 0.13.0 Alpha 2

这是临时公开验证仓库中的供应链演练版本，仅用于验证 Alpha 通道、PEP 440 包版本、双构建证明、CycloneDX SBOM、attestation 和发布审批流程。它不是正式 HighWall 发布，不增加能力或生产可用性声明。

验证重点：

- `v0.13.0-alpha.2` 映射为 Python 包版本 `0.13.0a2`；
- Linux 和 Windows 原生二进制各执行两次干净构建并比较；
- 每个原生目标附带 `.buildproof.json` 和 `.cdx.json` SBOM；
- 所有 Release 附件附带 build provenance attestation；
- GitHub Release 以 pre-release 发布，且不会移动 `latest`。

本版本沿用正式项目的未验收边界：不得据此宣称生产可用、C4 或通过第三方安全审计。
