# 证据索引

[返回首页](../README.md) · [结果](validated-findings.md)

**报告数字来自已校核的结构化证据，不来自计划、资源配置或推测。**本仓库发布精简摘要，原始证据留在私有研究环境。

## 1. 公开摘要

| 文件 | 内容 |
|---|---|
| [verified-findings.json](../evidence/verified-findings.json) | 真实业务、框架矩阵、恢复、应用隔离、网络/身份、故障定位、发布和费用组件 |
| [sources.json](../evidence/sources.json) | 来源文件 SHA-256、证据截止、发布时间及字段筛选规则 |

## 2. 结论与来源

| 结论 | 私有来源文件 | 公开核查入口 |
|---|---|---|
| 9 个真实业务组合、15 格框架与关键机制 | `console-components-20261010.json` | `verified-findings.json` 的 `current` |
| 历史框架 14/15 及原 ACA 配额拒绝 | `p1-matrix.json` | `historical.framework_sessions` |
| 历史回放 279 次原分母 | `phase4-replay-summary.json` | `historical.replay` |
| 早期真模型 18 次原分母 | `phase4-real-observations.json` | `historical.model_business` |

来源文件名用于定位研究批次，不提供私有仓库或云资源访问链接。公开文件不包含原生任务 ID、endpoint、授权值或原始财务响应。

## 3. 产品职责与新增技术指导的公共依据

- [Foundry Hosted Agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents)
- [Azure Container Apps Jobs](https://learn.microsoft.com/en-us/azure/container-apps/jobs)
- [AKS 基线架构](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks/baseline-aks)
- [Azure 计算服务选型](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)
- [Agent 编排模式](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/ai-agent-design-patterns)
- [Azure Virtual Machines](https://learn.microsoft.com/en-us/azure/virtual-machines/overview)
- [Azure 多租户应用架构](https://learn.microsoft.com/en-us/azure/architecture/guide/multitenant/overview)
- [Azure Well-Architected 安全指导](https://learn.microsoft.com/en-us/azure/well-architected/security/)

官方资料核查日期为 2026-10-12。产品设计说明与本研究实测结果分别引用，不将官方能力清单当作本研究全部验收通过。

新增决策树和场景指南是基于这些公共资料及既有实验的技术建议，不产生新的实验判定。`sources.json` 保留原结果发布时的来源与引用快照；此处单独列出新增技术指导的公共依据，不改写原实验文件或哈希。
