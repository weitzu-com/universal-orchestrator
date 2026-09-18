# Universal Orchestrator

用清晰的责任分工组织任务：ChatGPT 规划，Codex 执行，ChatGPT 对照验收标准审查。

## 当前版本

V1.1 提供 Codex Skill 指令、规划与交接模板，以及 Codex desktop 原生消息工具的自动交接流程。

**能力边界：自动交接由正在运行的 Codex desktop 任务调用应用原生消息工具完成。** 需要可用的 `read_thread` / `send_message_to_thread` 和可访问的 ChatGPT 对话。配置目标后，Codex 自动请求 ChatGPT 规划、执行本地工作、回传证据并读取审查结果。它不是独立后台服务，不会在应用关闭后继续，也不会自动接管所有 ChatGPT 对话。没有这些工具的 CLI 环境不能直接使用此适配器。

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

先在已安装 Skill 目录创建私有的 `bridge.local.json`，将 `chatgpt_thread_id` 填为你自己的 ChatGPT 对话 ID。不要提交该文件。

```json
{
  "enabled": true,
  "adapter": "codex-desktop-native-tools",
  "chatgpt_thread_id": "YOUR_CHATGPT_CONVERSATION_ID",
  "reply_timeout_seconds": 600,
  "max_repairs": 3
}
```

```text
用 $universal-orchestrator 处理以下任务：[目标]。
先定义范围、约束、验收标准；执行后返回改动、测试、证据与未解决问题。
```

## 流程

1. PLAN：明确目标、范围、约束、验收标准和验证方法。
2. HANDOFF：把计划整理成有边界的执行合同。
3. EXECUTE：执行并返回 result、diff summary、tests、artifacts/evidence、unresolved issues。
4. REVIEW：独立对照证据给出 PASS / FAIL / PARTIAL / BLOCKED / NEEDS_HUMAN。
5. REPAIR：生成最小修复请求。首次执行后最多修复三次。

生产部署、删除数据、外部发送或发布等高风险动作遵循用户授权边界。一次审查通过不自动授权后续高风险操作。

## 文件

- `skills/universal-orchestrator/SKILL.md`：入口指令。
- `skills/universal-orchestrator/agents/openai.yaml`：显示名称与默认提示。
- `skills/universal-orchestrator/references/`：规划、路由、交接、结果、审查和修复模板。
- `skills/universal-orchestrator/references/native-bridge.md`：真实消息交接、回复匹配、超时、恢复及证据规则。

模板是 Markdown 格式约定，并非机器可验证的 JSON Schema。

## 验证状态

发布前检查文件完整性、相对 Markdown 链接以及上传副本与已安装 Skill 的一致性。原先的本地烟雾检查只验证文档结构，未验证真实 ChatGPT PLAN → Codex EXECUTE → ChatGPT REVIEW 的跨应用调用，因此不将其标为端到端 PASS。

2026-09-18：新增原生适配流程后，已完成一次真实正常路径烟雾验证：ChatGPT 返回六项验收标准，Codex 创建并读取临时文件、计算哈希及 diff，回传后收到 ChatGPT 对六项标准的 PASS。审查基于工具输出，不代表审查者直接访问本地磁盘。尚未实测连续三轮修复、并发任务或重启恢复。用户的对话 ID 和完整会话记录不包含在公开仓库中。

当前适配器利用宿主消息工具，由运行中的代理执行交接并保存任务证据。独立后台调度和机器可验证 JSON schemas 尚未实现。原生工具的会话读取可能存在同步延迟；必须等待匹配的新回复，不能把消息已接受当作规划或审查已完成。
