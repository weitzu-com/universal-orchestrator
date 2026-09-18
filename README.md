# Universal Orchestrator

用清晰的责任分工组织任务：ChatGPT 规划，Codex 执行，ChatGPT 对照验收标准审查。

## 当前版本

V1 提供 Codex Skill 指令、规划与交接模板、执行结果格式、独立审查规则、有限修复循环及人工授权边界。

**能力边界：当前仓库没有跨应用传输层、自动调度程序或 ChatGPT 连接器。安装 Skill 不会自动连接 ChatGPT 与 Codex，也不会把本地 Skill 安装进 ChatGPT。** 使用真实双端流程需要另外提供可用的连接方式，或人工交接计划和结果。同一代理模拟三个角色不等于独立 ChatGPT 审查。

## 安装

克隆仓库后，在仓库根目录运行以下命令。目标已存在时停止，避免覆盖已有配置。

```sh
skill_target="${CODEX_HOME:-$HOME/.codex}/skills/universal-orchestrator"
if [ -e "$skill_target" ]; then
  printf '%s\n' "Target already exists: $skill_target"
else
  mkdir -p "$(dirname "$skill_target")"
  cp -R skills/universal-orchestrator "$skill_target"
fi
```

在能发现本地 Skill 的 Codex 会话中使用：

```text
用 $universal-orchestrator 处理以下任务：[目标]。
先定义范围、约束、验收标准；执行后返回改动、测试、证据与未解决问题。
```

## 流程

1. PLAN：明确目标、范围、约束、验收标准和验证方法。
2. HANDOFF：把计划整理成有边界的执行合同。
3. EXECUTE：执行并返回 result、diff summary、tests、artifacts/evidence、unresolved issues。
4. REVIEW：独立对照证据给出 PASS / FAIL / PARTIAL / BLOCKED / NEEDS_HUMAN。
5. REPAIR：生成最小修复请求。现有 Skill 上限为三次总执行尝试，包括首次执行。

生产部署、删除数据、外部发送或发布等高风险动作遵循用户授权边界。一次审查通过不自动授权后续高风险操作。

## 文件

- `skills/universal-orchestrator/SKILL.md`：入口指令。
- `skills/universal-orchestrator/agents/openai.yaml`：显示名称与默认提示。
- `skills/universal-orchestrator/references/`：规划、路由、交接、结果、审查和修复模板。

模板是 Markdown 格式约定，并非机器可验证的 JSON Schema。

## 验证状态

发布前检查文件完整性、相对 Markdown 链接以及上传副本与已安装 Skill 的一致性。原先的本地烟雾检查只验证文档结构，未验证真实 ChatGPT PLAN → Codex EXECUTE → ChatGPT REVIEW 的跨应用调用，因此不将其标为端到端 PASS。

自动传输、持久化任务状态、机器可验证 schemas、真实双端集成测试仍待实现。
