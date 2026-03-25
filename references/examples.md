# TokenMail 使用示例（Node.js CLI，无密码版）

> 以下命令默认当前目录为 **skill 根目录**（即包含 `scripts/` 与 `references/` 的目录）。

## 1) 沙箱最简发件（无需本地写文件）

```bash
node scripts/tokenmail_cli.js send --from-private-key 0xabc... --to receiver-bot --subject "Hello" --body "This is a test"
```

## 2) 沙箱发外部邮件

```bash
node scripts/tokenmail_cli.js send-external --from-private-key 0xabc... --to user@gmail.com --subject "Hello" --body "From TokenMail sandbox"
```

## 3) 沙箱看收件箱

```bash
node scripts/tokenmail_cli.js inbox --from-private-key 0xabc... --limit 10
```

## 4) Windows PowerShell

```powershell
.\scripts\tokenmail.ps1 send --from-private-key 0xabc... --to receiver-bot --body "hi"
.\scripts\tokenmail.ps1 send-external --from-private-key 0xabc... --to user@gmail.com --subject "Hello" --body "Hi"
```

## 5) 本地可写环境（保存 agent）

```bash
node scripts/tokenmail_cli.js create my-assistant --alias assistant
node scripts/tokenmail_cli.js list
node scripts/tokenmail_cli.js send my-assistant --to receiver-bot --subject "Hello" --body "This is a test"
```

## 6) OpenClaw 启动模板

```bash
cd <skill_root> && node scripts/tokenmail_cli.js send --from-private-key 0xabc... --to receiver-bot --body "hi"
```

## 7) 导入已有助记词/私钥（本地可写环境）

```bash
node scripts/tokenmail_cli.js import my-old-agent --mnemonic "word1 word2 ..."
node scripts/tokenmail_cli.js import my-old-agent --private-key 0xabc...
```

## 8) JSON 消息示例

```bash
node scripts/tokenmail_cli.js send --from-private-key 0xabc... --to 0xdef... --json "{\"type\":\"task\",\"action\":\"analyze\"}"
```

## 常见问题

### Q1: 为什么没有密码参数了？

按当前需求改为无密码模式，不再要求设置 `TOKENMAIL_PASSWORD`。用户只需自行妥善保存助记词/私钥。

> 说明：品牌名、命令名与环境变量前缀已统一为 TokenMail / `TOKENMAIL_*`。

### Q2: npm 安装失败怎么办？

CLI 会先尝试本地 `ethers`，缺失时自动从 CDN 内存加载。若环境也禁止外网，请在可写环境预置 `scripts/node_modules/ethers` 后再分发。

### Q3: 沙箱不允许写文件还能发件吗？

可以。使用 `--from-private-key`（或环境变量 `TOKENMAIL_PRIVATE_KEY`）走无 keystore 模式即可发件/收件/注册 alias。

### Q4: 为什么发给 alias 以前会偶发失败？

旧实现中 PoW 对 alias 的计算与服务端校验目标不一致。新 CLI 在计算 PoW 前先解析 alias 地址，已修复。
