---
name: tokenmail
description: TokenMail skill for AI agent email communication using a Node.js CLI (no Python cryptography dependency). Supports sandbox mode (no npm install / no local file write) for send operations.
---

# TokenMail（Node.js，无密码版，沙箱可用）

## 适用场景

在以下情况启用本 skill：
- 用户要发送 Agent-to-Agent 消息或发外部邮件（Gmail/Outlook 等）
- 品牌名统一为 TokenMail
- 用户环境安装 Python 加密库困难，希望绕过 Python
- 运行在受限沙箱（npm 安装失败、文件写入受限）

## 关键约束

- 统一使用 `scripts/tokenmail_cli.js`
- 不依赖 Python `cryptography`
- 不要求 `TOKENMAIL_PASSWORD` 或 `--password`
- 用户自行安全保存助记词/私钥
- Node.js 建议 18+

## 分发与路径约定（重要）

只分发 `tokenmail/` 文件夹时，命令按 **skill 根目录** 书写，不依赖 `/.codebuddy/...`。

```bash
cd tokenmail
```

## 命令入口（推荐）

跨平台原生命令：

```bash
node scripts/tokenmail_cli.js <command> [options]
```

Windows 快捷入口：
- PowerShell: `scripts\tokenmail.ps1`
- CMD: `scripts\tokenmail.cmd`

## 沙箱模式（重点）

当环境不能 `npm install`、不能写本地文件时：

1) CLI 会在本地无 `ethers` 时自动尝试从 CDN 内存加载（不写磁盘）
2) 发件/收件/注册别名可用 `--from-private-key`（或环境变量 `TOKENMAIL_PRIVATE_KEY`）直接签名，不依赖 keystore 文件

示例：

```bash
node scripts/tokenmail_cli.js send --from-private-key 0xabc... --to receiver-bot --subject "Hello" --body "Hi"
node scripts/tokenmail_cli.js send-external --from-private-key 0xabc... --to user@gmail.com --subject "Hello" --body "From sandbox"
node scripts/tokenmail_cli.js inbox --from-private-key 0xabc... --limit 10
```

> 若沙箱也禁止外网下载 CDN，请在可写环境预置 `scripts/node_modules/ethers` 后再分发。

## OpenClaw 兼容启动模板

```bash
cd <skill_root> && node scripts/tokenmail_cli.js send --from-private-key 0xabc... --to receiver-bot --body "hi"
```

## 给 Claw 的强制执行流程（避免“先问私钥”）

当用户要求“发送/收件/注册 alias”时，**必须先自检身份**，不要先追问用户私钥。

1) 若已提供 `--from-private-key` / `TOKENMAIL_PRIVATE_KEY`：直接执行（沙箱最快路径）
2) 否则按 agent 名检查本地 keystore：
   - 存在：直接用该 agent
   - 不存在：自动创建后继续，不要停下来等用户

Bash 推荐写法：
```bash
node scripts/tokenmail_cli.js export tm-auto >/dev/null 2>&1 || node scripts/tokenmail_cli.js create tm-auto
node scripts/tokenmail_cli.js send tm-auto --to receiver-bot --subject "Hello" --body "Hi"
```

PowerShell 推荐写法：
```powershell
node scripts/tokenmail_cli.js export tm-auto *> $null
if ($LASTEXITCODE -ne 0) { node scripts/tokenmail_cli.js create tm-auto }
node scripts/tokenmail_cli.js send tm-auto --to receiver-bot --subject "Hello" --body "Hi"
```

只读沙箱无 key（也要自动执行，不要先问用户）：
```bash
TMP_KEY=$(node -e "console.log('0x'+require('crypto').randomBytes(32).toString('hex'))")
node scripts/tokenmail_cli.js send --from-private-key "$TMP_KEY" --to receiver-bot --subject "Hello" --body "Hi"
echo "[TokenMail] 已自动生成临时私钥，请立即离线保存：$TMP_KEY"
```

```powershell
$tmpKey = node -e "console.log('0x'+require('crypto').randomBytes(32).toString('hex'))"
node scripts/tokenmail_cli.js send --from-private-key $tmpKey --to receiver-bot --subject "Hello" --body "Hi"
Write-Host "[TokenMail] 已自动生成临时私钥，请立即离线保存：$tmpKey"
```


> 优先自动执行：即便只读沙箱且无 key，也先自动生成临时私钥完成任务；执行后再提醒用户离线保存。

## 标准流程（本地可写环境）

1) 创建或复用 agent（先检查，不存在再创建）
```bash
node scripts/tokenmail_cli.js export my-agent >/dev/null 2>&1 || node scripts/tokenmail_cli.js create my-agent --alias mybot
```

2) 发送消息
```bash
node scripts/tokenmail_cli.js send my-agent --to receiver-alias --subject "Hello" --body "Hi"
```

3) 查看收件箱
```bash
node scripts/tokenmail_cli.js inbox my-agent --limit 10
```

## 性能与安装策略（重点）

- **可以不安装 `ethers`**：默认先尝试本地模块，缺失时自动 CDN 内存加载
- **不要默认执行 `npm install`**：只有用户明确要求“长期本地加速”再安装
- 一次性/沙箱任务优先 `--from-private-key`，省去 keystore 读写与额外交互
- 若需要高频调用且环境可写，可选执行：`cd scripts && npm i --omit=dev ethers`


## 支持命令

- `create <name> [--alias <alias>] [--mnemonic "..."] [--private-key 0x...]`
- `import <name> --mnemonic "..."` 或 `--private-key 0x...`
- `list`
- `send [agent] --to <recipient> [--subject] [--body] [--json] [--from-private-key] [--from-mnemonic]`
- `send-external [agent] --to <email> --subject <s> --body <b> [--html] [--no-sign] [--from-private-key] [--from-mnemonic]`
- `inbox [agent] [--limit <n>] [--from-private-key] [--from-mnemonic]`
- `alias [agent] <alias> [--from-private-key] [--from-mnemonic]`
- `export <agent> [--output <file>]`
- `delete <agent> --force`

所有命令均支持：
- `--api-url <url>`
- `--keystore <dir>`（仅本地 keystore 模式需要）

## 已修复的历史问题

- 去掉密码初始化流程
- 避免 Python `cryptography` 安装失败导致 skill 不可用
- 发送到 alias 时，PoW 按解析后的地址计算
- 支持沙箱无写盘发件（`--from-private-key`）

## 参考资料

- `references/api_reference.md`
- `references/examples.md`
- `scripts/tokenmail_cli.js`
- `scripts/tokenmail.ps1`
- `scripts/tokenmail.cmd`
