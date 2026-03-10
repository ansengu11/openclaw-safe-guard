# 龙虾安全卫士 (openclaw-safe-guard) v1.0.2

OpenClaw 安全扫描工具 v1.0.2 - 扫描 Skills 和系统安全风险

OpenClaw Skill 安全审计工具 - 扫描已安装的 Skills，检测安全风险

## 功能

当用户请求扫描 Skill 安全时，使用此 Skill：

- 扫描指定 Skill 的权限风险
- 检查是否有恶意代码
- 分析依赖包安全性
- 生成中文风险报告
- 给出修复建议

## 触发条件

用户说以下内容时激活：
- "扫描 Skill 安全"
- "检查 Skill 风险"
- "这个 Skill 安全吗"
- "帮我看看这个技能"
- "扫描已安装的 Skills"

## 使用方式

```
用户: 帮我扫描 binance-pro 这个 Skill
AI: → 调用 openclaw-safe-guard，输入 "binance-pro"

用户: 检查 openclaw/skills 目录下的 Skills
AI: → 调用 openclaw-safe-guard，扫描所有已安装的 Skills
```

## 输出格式

```markdown
# 🔒 Skill 安全评估报告

## 基础信息
- 名称：xxx
- 路径：~/.openclaw/skills/xxx
- Stars：xx（来自 GitHub）
- 官方：✅/❌

## 风险评分：🟢 低风险 / 🟡 中风险 / 🔴 高风险

## 权限检查
| 权限 | 状态 | 风险 |
|------|------|------|
| 网络请求 | ✅ 无 | 低 |
| Shell 执行 | ⚠️ 有 | 中 |
| 文件访问 | ⚠️ 有 | 中 |

## 详细分析
### 代码检查
- 敏感信息泄露：❌ 未发现
- 恶意代码：❌ 未发现
- 可疑 API 调用：❌ 未发现

### 依赖检查
- 第三方包：无风险

## 建议
1. 建议审查 Shell 脚本内容
2. 建议仅授予必要权限

## 总结
该 Skill 风险等级：🟡 中风险
建议：可使用，但需注意权限
```

## 依赖要求

此 Skill 依赖以下系统工具：
- `curl` - 用于访问 GitHub API
- `jq` - 用于解析 JSON 数据
- `git` - 用于克隆 GitHub 仓库
- `grep` - 用于搜索代码
- `find` - 用于查找文件

## 权限要求

此 Skill 需要以下权限：
- 读取 ~/.openclaw/skills 目录
- 读取 ~/.openclaw/workspace/skills 目录
- 访问 GitHub API（获取 Stars 等信息）
- 读取 Skill 源码文件
- 调用系统命令：curl, jq, git（用于获取 GitHub 信息）

## 声明的权限

```json
{
  "requires": {
    "filesystem": ["~/.openclaw/skills", "~/.openclaw/workspace/skills", "/tmp"],
    "commands": ["curl", "jq", "git", "grep", "find"],
    "network": ["api.github.com", "github.com"]
  },
  "dependencies": {
    "system_tools": ["curl", "jq", "git", "grep", "find"]
  },
  "environment": {
    "description": "需要网络访问 GitHub API 获取 Skill 信息"
  }
}
```

## 安全提醒

- 静态读取 Skill 源码可能发现敏感信息
- 使用前请确认愿意授予读取权限
- 不会执行或修改被扫描的 Skill 代码
- 不会上传扫描结果到任何远程端点

## 安装前建议

1. 确认元数据中明确列出了必需的配置路径和二进制
2. 仅在隔离环境或受限账户下运行
3. 人工审查 scan.sh 源码（已含在包内）
4. 可在只读环境或容器中运行以提高安全性

## 示例

### 扫描单个 Skill
```
用户: 帮我看看 binance-pro 安全吗
AI: 正在扫描 binance-pro Skill...
[调用 openclaw-safe-guard]
AI: 扫描完成！该 Skill 风险等级为 🟢 低风险...
```

### 扫描所有 Skills
```
用户: 扫描所有已安装的 Skills
AI: 正在扫描 ~/.openclaw/skills 目录...
[调用 openclaw-safe-guard]
AI: 共扫描 X 个 Skills，发现 Y 个高风险...
```

## 注意事项

- 仅扫描已安装的 Skills
- 不执行 Skill 代码，仅静态分析
- 风险评估基于启发式规则，可能有误判
- 建议配合人工审查
- 脚本会读取被扫描 Skill 的源码，请注意敏感信息
- 在线扫描会从 GitHub 克隆源码，被扫描代码可能包含敏感内容
- 官方仓库 (openclaw/*) 会标记为"已通过官方审核"，跳过详细扫描
