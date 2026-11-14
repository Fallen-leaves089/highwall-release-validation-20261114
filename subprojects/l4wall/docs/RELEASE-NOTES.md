# L4Wall 0.2.0

本文件位于临时验证仓库，仅用于演练 L4Wall 的 Stable 发布通道、PEP 440 版本映射、provenance attestation 和仓库级 `latest` 保护，不代表正式 L4Wall 发布。

发布日期：2026-11-03

L4Wall 是 HighWall 仓库中的独立 TCP L4 防护网关，负责连接级准入、并发与速率限制、超时、字节上限、行为评分、封禁和审计。`0.1.0` 现在以独立标签和独立 GitHub Release 发布，不再依附 HighWall 主版本包。

## 发布性质

- 本次演练从当前验证提交重新构建 L4Wall `0.2.0` wheel 和 sdist。
- 所有发布资产带 GitHub build provenance attestation，并在工作流内通过 `gh attestation verify` 复验。
- 发布使用独立 `l4wall-v0.2.0` 标签，不包含 HighWall、L5Wall 或原生核心二进制。
- 发布后必须保持仓库原有 `latest` 不变；即使 GitHub 自动移动 `latest`，工作流也会恢复并再次校验。

## 发布产物

- `l4wall-0.2.0-py3-none-any.whl`
- `l4wall-0.2.0.tar.gz`
- `SHA256SUMS`（只包含上面两个 L4Wall 文件）

SHA-256 以下载的 `SHA256SUMS` 为准。

独立发布的标准下载位置是本 Release 的 **Assets**；旧版 `v0.11.0` 页面仅保留原始 L4Wall 附件以兼容旧链接。L4Wall 不提供 GHCR 镜像、PyPI 包或其他额外下载渠道，也不把 HighWall、L5Wall 或原生核心二进制打进 L4Wall 包。

## 状态

- 版本：`0.2.0`
- 成熟度：仅 Stable 发布路径演练，未经独立第三方安全审计，非生产就绪
- 运行时：Python 3.10+，只依赖标准库
- 覆盖：TCP 连接级防护；没有 TLS/HTTP 内容检查
- 审计与生产准入：未经过独立第三方安全审计，不是生产就绪产品

本次演练执行 Python 3.10-3.15 测试矩阵、静态检查、干净虚拟环境安装、CLI 版本检查、资产哈希和 provenance 复验。

## 安装

先下载本 Release 的三个附件并校验：

```bash
sha256sum -c SHA256SUMS
python3 -m venv /opt/l4wall
/opt/l4wall/bin/pip install --no-cache-dir ./l4wall-0.2.0-py3-none-any.whl
/opt/l4wall/bin/l4wall --version
/opt/l4wall/bin/l4wall init --path ./config.json
/opt/l4wall/bin/l4wall check --config ./config.json
```

生产前请阅读 [L4Wall README](https://github.com/Fallen-leaves089/highwall/blob/main/subprojects/l4wall/README.md)、[部署基线](https://github.com/Fallen-leaves089/highwall/blob/main/subprojects/l4wall/docs/DEPLOYMENT.md) 和 [威胁模型](https://github.com/Fallen-leaves089/highwall/blob/main/subprojects/l4wall/docs/THREAT_MODEL.md)。

## 边界

- 不处理带宽型 DDoS、SYN flood、UDP、QUIC/HTTP3 或运营商侧拥塞。
- 不解析 HTTP，不做认证、授权、WAF、DLP、TLS 终止或 JA3/JA4。
- 只保护显式配置的 listener；直连 HighWall 或上游仍会绕过 L4Wall，必须用监听地址和网络 ACL 收紧。
- 共享封禁在 `0.1.0` 中只有读取端；自动发布、Controller、多节点冲突合并和 HA 尚未实现。
- 不适用于已失陷主机；攻击者若能修改配置、审计或进程内存，L4 边界已经失效。

## 许可

Apache-2.0。许可证与 NOTICE 见仓库根目录；未来独立构建也会随 wheel/sdist 分发对应文件。
