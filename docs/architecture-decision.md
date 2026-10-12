# 架构选型建议

[返回首页](../README.md) · [参考架构](reference-architecture.md) · [验证结果](validated-findings.md)

**先选择团队愿意承担的运行责任，再选择 runtime。**本研究已经证明所选业务和框架可以在三端接入；真正需要客户决定的是容器治理、集群治理和 Agent 生命周期治理之间的取舍。

## 1. 按需求作出选择

| 客户需求 | 建议路径 | 决策理由 |
|---|---|---|
| 把已有容器化 Agent API 或后台任务上线，不需要 Kubernetes 专属能力 | **优先评估 ACA** | App 与 Job 分别承载在线服务和有限时长任务，可以减少自管集群的治理面 |
| 已有成熟 Kubernetes 平台，要求节点池、调度、网络策略或复杂工作负载组合 | **优先评估 AKS** | 可以沿用现有平台治理、资源调度和运维体系 |
| 产品围绕 Agent endpoint、identity、version 和 session 构建 | **优先评估 Foundry Hosted Agents** | 托管 Agent 生命周期与产品对象一致，减少自行搭建相同平台机制的需求 |
| 需要在三端保留业务迁移能力 | **统一业务契约，隔离 runtime 适配** | 本研究采用相同业务逻辑、输入和输出契约，三端只在原生调度与生命周期上适配 |

这里的“优先”是需求匹配建议，不是实验产生的综合评分。ACA 也需要容量与发布管理；AKS 的细粒度控制不自动等于更高性能；Hosted Agents 的托管能力不替代业务授权。

## 2. 责任差异应体现在架构中

| 责任 | ACA | AKS | Foundry Hosted Agents |
|---|---|---|---|
| 运行单元 | App、Job、revision | Deployment、Service、Job、Pod | Agent、immutable version、endpoint、session |
| 基础设施治理重点 | 环境、资源配置、扩缩容规则和 revision | 集群升级、节点池容量、调度与工作负载策略 | Agent 版本、协议配置、身份和托管生命周期 |
| 业务状态与副作用 | 应用定义持久状态、checkpoint 与幂等 | 应用定义持久状态、checkpoint 与幂等 | 使用所选协议的平台能力，同时定义业务状态、工具副作用与恢复契约 |
| 下游数据访问 | 应用与工作负载身份共同约束 | 应用、Kubernetes 服务账户与联合身份共同约束 | Agent identity 的资源权限与应用授权共同约束 |
| 发布验收对象 | revision 与真实业务任务 | Deployment/Job 与真实业务任务 | version/session 与真实业务任务 |

AKS 建议针对本研究采用的常规集群管理方式；不把这里的运维责任直接套用于 AKS Automatic 等其他管理模式。

Foundry 的平台 session 与业务 workspace、任务账本不是同一对象。官方文档定义了平台会话持久化与隔离能力；本研究的恢复结果主要验证应用持久状态和 checkpoint，二者应分别解释。

## 3. 工程团队应固定的四项契约

1. **任务契约：**提交即绑定业务参数、runtime 和版本；重复请求返回原任务，不产生第二次副作用。
2. **状态契约：**区分会话历史、业务状态、工作文件和 checkpoint；明确每类数据的持久化位置与授权范围。
3. **身份契约：**浏览器用户、网关身份和工作负载身份分开；下游权限只授予实际执行身份。
4. **发布契约：**新任务进入新版本，在途任务保持原绑定；回滚后重新检查真实业务、结果和导出。

这些契约比把所有平台功能放进一张总分表更能帮助客户控制上线风险。

## 4. 官方设计依据

- [Azure Container Apps Jobs](https://learn.microsoft.com/en-us/azure/container-apps/jobs)：在线 App 与有限时长 Job 的职责区别。
- [AKS 基线架构](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks/baseline-aks)：网络、身份、节点、连续性与团队治理责任。
- [Foundry Hosted Agents](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/hosted-agents)：托管 Agent 的代码、协议、身份与生命周期边界。

官方资料用于解释产品设计；本报告中的实测结论以[证据摘要](../evidence/verified-findings.json)为准，不把文档中的全部产品能力写成本研究已完成的验收。
