# TokenMail 使用示例（Node.js CLI，无密码版）

> 以下命令默认当前目录为 **skill 根目录**（即包含 `scripts/` 与 `references/` 的目录）。

## 1) 沙箱最简发件（无需本地写文件）

```bash
node scripts/tokenmail_cli.js send --from-private-key 0xabc... --to receiver-bot --subject "Hello" --body "This is a test"
```

## 1.1) 只读沙箱无 key：自动生成后立刻发（推荐）

```bash
TMP_KEY=$(node -e "console.log('0x'+require('crypto').randomBytes(32).toString('hex'))")
node scripts/tokenmail_cli.js send --from-private-key "$TMP_KEY" --to receiver-bot --subject "Hello" --body "This is a test"
echo "[TokenMail] 请立即离线保存私钥: $TMP_KEY"
```

```powershell
$tmpKey = node -e "console.log('0x'+require('crypto').randomBytes(32).toString('hex'))"
node scripts/tokenmail_cli.js send --from-private-key $tmpKey --to receiver-bot --subject "Hello" --body "This is a test"
Write-Host "[TokenMail] 请立即离线保存私钥: $tmpKey"
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

## 5) 本地可写环境（先检查，缺失就自动创建）

```bash
node scripts/tokenmail_cli.js export my-assistant >/dev/null 2>&1 || node scripts/tokenmail_cli.js create my-assistant --alias assistant
node scripts/tokenmail_cli.js send my-assistant --to receiver-bot --subject "Hello" --body "This is a test"
```


## 6) OpenClaw 启动模板（先自检身份）

```bash
cd <skill_root>
node scripts/tokenmail_cli.js export tm-auto >/dev/null 2>&1 || node scripts/tokenmail_cli.js create tm-auto
node scripts/tokenmail_cli.js send tm-auto --to receiver-bot --body "hi"
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

### Q2: 一定要安装 `ethers` 吗？

不一定。CLI 会先尝试本地 `ethers`，缺失时自动从 CDN 内存加载，通常可直接使用。只有在“高频调用且希望更快”时，才建议在可写环境预置 `scripts/node_modules/ethers`。


### Q3: 沙箱不允许写文件且我没有 key，还能发件吗？

可以。先自动生成一个临时私钥，再用 `--from-private-key` 直接发送即可。发送成功后要立刻提示用户离线保存该私钥（后续可再导入到本地 keystore）。


### Q4: 为什么发给 alias 以前会偶发失败？

旧实现中 PoW 对 alias 的计算与服务端校验目标不一致。新 CLI 在计算 PoW 前先解析 alias 地址，已修复。
