---
type: rule
project: ai-goofish-monitor
title: Agent 浏览器会话与资源回收
owners: [Owner, Codex]
created: 2026-11-01
updated: 2026-11-01
status: active
visibility: internal
source: [owner-chat-2026-11-01, MyMemory-AGENTS-template]
related: [AGENTS.md]
superseded_by: []
---

# Agent 浏览器会话与资源回收

## 浏览器会话与资源回收合同

适用于本项目 Agent、CLI、脚本、浏览器工具、Playwright 与 Chrome/Chromium；只有任务实际使用浏览器时执行，不因本规则启动浏览器、产品测试或部署。

1. **先建立归属。** 启动前在仓外任务记录登记项目/repo、task/session ID、创建者、实际执行主机、用途、启动时间、生命周期负责人和清理方式；启动后补 page/target、context、browser、独立 profile 路径及可用的进程身份。标题可加不含敏感信息的项目/任务标签辅助辨认，但标题不能替代台账。控制端在 MacBook、执行端在 Mac Studio 时分别记录，不能用控制端 UI 推断远端资源。
2. **区分所有权。** 自建自动化实例使用本任务独立 profile/context 和仓外临时目录；不读取或复制用户 Chrome/Safari profile、Cookie、Token。借用用户已打开的浏览器或工具托管的共享实例时，先记已有页面，只登记并关闭本任务新建的页面/上下文，不退出共享实例、不清除登录态、不关闭用户既有页面；其他项目更严格的账号/会话限制继续适用。
3. **有管理器就复用。** 优先通过已经存在的受管 launcher/Runner 创建并释放资源；没有时，由创建者维护最小任务记录与显式清理，不假定某个 browser manager、watchdog 或 CLI 已安装。浏览器工具不支持关闭时如实记录残留与接管人，使用其受支持的释放方式，不能绕过工具权限杀共享进程。
4. **正常与异常均收尾。** 成功、失败、超时、取消后均释放本任务资源；脚本使用 try/finally 或退出钩子，并设总时限。优先逐个关闭自建 page/context，再关闭自建 browser，等待进程退出后才释放其临时 profile；需要保全的证据按项目规则先留存。finally 无法覆盖强杀、进程崩溃或断连，外层 Runner/启动器负责退出兜底与下次恢复时核对；未实现该兜底时明确记录能力缺口，不把文字要求当作运行保证。
5. **精准回收。** 只处理可证明属于本任务且无活动职责的资源。对需要终止的独立进程，核对实际主机、启动时间、命令/profile、父子关系或进程组以及任务台账，防止 PID 重用；先正常关闭，必要时仅终止已核验的自有进程组。禁止全局 pkill/killall Chrome、按进程名批杀，或仅凭 PID、PPID=1、存活时间、低 CPU、高内存和 DONE 标题判定可清理。归属不明先只读调查并报告，不自动关闭。
6. **明确保留。** Owner 要求继续查看、调试或交接时，可保留指定资源；写明用途、资源 ID、负责人、到期复核时间和最终关闭方式。到期触发归属/活动复核，不自动授权关闭用户页面。未授权例外不得成为无限期后台保留。
7. **交付可核对。** 结束时核对本任务页面、上下文、独立 browser/进程组及临时 profile 已释放；简要报告清理结果、残留、能力缺口和保留期限。台账、进程清单、截图、下载与 profile 留在仓外，正式文档只记脱敏结论，不记录 Cookie、凭据、带密钥的命令或完整敏感 URL。

规则与执行器分层：MyMemory 维护共用模板，各仓在自己的 AGENTS 中持有适用规则，launcher/Runner 承担实际生命周期保障。更新模板/规则不等于已部署自动回收；既有活动会话不会自动重载，新任务从更新后的默认分支开始，活动分支通过受控 merge/rebase 接入。本次不安装后台回收器，也不清理历史未明 Chrome。

## 本仓采用与验收

- 授权：当前会话要求更新 AGENTS 模板与相关项目，重点为 MyMemory、LoopAgent、Top AI Fusion。来源：MyMemory `90-Templates/codex-dev-project-agents-template.md` 本次浏览器合同增量；同步前 MyMemory 基线 `d1f84e2561b96870be6c8287b7d4044ac7ef844a`。
- 本仓 `base_revision`：`99a897d61d2a7456172ee2f5cc715505b435f921`；集成目标 `master`，任务分支 `codex/20261101-browser-lifecycle`；单一写入者/集成者 Codex，仓内串行集成。跨仓顺序 19，共享产品测试资源未占用。
- write_scope：根 AGENTS、本合同、必要 README/治理索引；LoopAgent 另同步自己的 AGENTS 模板。精确文件集以该原子提交 diff 为准；不改活动 worktree 或业务代码。
- 验收：差异/空白、合同一致性、新增本地链接与索引、根文件预算及唯一 H1；未运行产品测试或部署，因为本次只改执行规则。LoopAgent 另按自身规则验证 CLI 与测试。
- 回滚：由开发根对本次规则提交执行受控 revert，保留后续业务与治理增量；不修改浏览器运行态。

## 更新日志

- 2026-11-01：按 Owner 明确授权同步浏览器归属与资源回收规则；新增自建/借用边界、正常与异常收尾、精准回收和保留复核。此次为规则治理，未启动产品浏览器测试、部署或历史 Chrome 清理。
