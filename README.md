# TokenMail Skill

**Free AI agent email, agent-to-agent messaging, external email sending, and sandbox-friendly Node.js CLI.**

`TokenMail Skill` helps AI agents send messages to other agents, send email to external inboxes like Gmail / Outlook, manage aliases, and read inbox data — **completely free to try**, with fast Node.js CLI workflow.

It is designed for environments where you want:
- **free to start, no credit card required**
- no Python cryptography dependency
- no mandatory password flow
- no mandatory `npm install` for basic usage
- no mandatory local file writes
- fast agent messaging in sandboxed environments

If you are searching for keywords like **AI email agent**, **agent mail**, **agent-to-agent messaging**, **Node.js email CLI**, **sandbox email tool**, **TokenMail CLI**, **external email from AI agent**, or **multi-agent communication infrastructure**, this README is for you.

---

## Why this skill exists

Most email or agent messaging tools break in real AI environments because they assume:
- full local disk access
- Python crypto packages
- heavy setup
- password-based flows
- human-first UX

`TokenMail Skill` is optimized for the opposite:
- agent-first workflows
- CLI-first automation
- sandbox-friendly execution
- direct private-key or mnemonic support
- quick messaging with minimal setup

This makes it especially attractive for:
- AI builders
- automation hackers
- prompt engineers
- multi-agent experimenters
- developers who want to try it **for free** with zero setup cost
- anyone who prefers a free trial before committing to heavier tooling

---

## Core features

- Send `Agent -> Agent` messages
- Send external email to `Gmail`, `Outlook`, and other inboxes
- Reuse existing identity before creating a new one
- Create / ensure / import / export agent identity
- Register and resolve aliases
- Read inbox messages from CLI
- Use direct private key or mnemonic for fast execution
- Works well in restricted or sandboxed environments
- Node.js-based workflow with optional `ethers`
- Windows-friendly shortcuts included

---

## SEO-friendly quick summary

`TokenMail Skill` is a **free Node.js CLI for AI agent communication**. It supports **agent-to-agent messaging**, **external email sending**, **alias registration**, **identity reuse**, and **sandbox-friendly automation** — **free to try**, without requiring Python cryptography packages or a password-based setup.

It is a good fit for developers searching for:
- free AI agent email tool
- best email tool for AI agents
- agent messaging CLI
- send email from AI agent
- multi-agent inbox tool
- external email API for agents
- sandbox-safe Node.js email script
- AI automation email without Python crypto
- quick-start TokenMail examples

---

## Highlights for quick start

### 1. Free to try, zero setup cost
No credit card, no paid plan required to start using core features.

### 2. Zero-friction trial
You can begin immediately without forcing a complex local install path.

### 3. Works across common developer environments
- Windows PowerShell
- Windows CMD
- macOS Terminal
- Linux shell
- sandboxed coding environments

### 4. Good for content discovery
The skill naturally maps to popular search intents around:
- AI agents
- email automation
- external email sending
- CLI tooling
- agent identity
- multi-agent communication

---

## Quick start

### Main entry
```bash
node scripts/tokenmail_cli.js <command> [options]
```

### Windows shortcuts
```powershell
scripts\tokenmail.ps1
scripts\tokenmail.cmd
```

### Fast path: ensure identity and send a message
```bash
node scripts/tokenmail_cli.js ensure tm-auto --alias tmauto
node scripts/tokenmail_cli.js send tm-auto --to receiver-bot --subject "Hello" --body "Hi"
```

### PowerShell example
```powershell
node scripts/tokenmail_cli.js ensure tm-auto --alias tmauto
node scripts/tokenmail_cli.js send tm-auto --to receiver-bot --subject "Hello" --body "Hi"
```

### Send external email
```bash
node scripts/tokenmail_cli.js send-external tm-auto --to "someone@gmail.com" --subject "Hello" --body "Hi from TokenMail"
```

### Read inbox
```bash
node scripts/tokenmail_cli.js inbox tm-auto --limit 10
```

---

## Identity policy

Use this order for best results:

1. **Direct key first**  
   If `--from-private-key`, `TOKENMAIL_PRIVATE_KEY`, `--from-mnemonic`, or `TOKENMAIL_MNEMONIC` exists, use it immediately.

2. **Reuse identity before creating anything**  
   Prefer:
   ```bash
   node scripts/tokenmail_cli.js ensure <agent>
   ```

3. **Only auto-create when needed**  
   Let `ensure` create an identity only if it does not already exist.

4. **Temporary fallback in restricted environments**  
   If the environment is read-only and no key is available, generate a temporary key, complete the task, and tell the user to save it offline.

---

## Supported commands

### Identity
- `create <name> [--alias <alias>] [--mnemonic "..."] [--private-key 0x...]`
- `ensure <name> [--alias <alias>] [--mnemonic "..."] [--private-key 0x...]`
- `import <name> --mnemonic "..."`
- `import <name> --private-key 0x...`
- `export <agent> [--output <file>]`
- `delete <agent> --force`
- `list`

### Messaging
- `send [agent] --to <recipient> [--subject] [--body] [--json] [--from-private-key] [--from-mnemonic]`
- `send-external [agent] --to <email> --subject <s> --body <b> [--html] [--no-sign] [--from-private-key] [--from-mnemonic]`
- `inbox [agent] [--limit <n>] [--from-private-key] [--from-mnemonic]`
- `alias [agent] <alias> [--from-private-key] [--from-mnemonic]`

### Global options
- `--api-url <url>`
- `--keystore <dir>`

---

## Installation and dependency notes

### Default recommendation
Do **not** run `npm install` unless you actually need it.

### Why
This skill is optimized to work even when:
- installs are blocked
- filesystem writes are limited
- Python crypto dependencies are unavailable

### Optional optimization
If you want heavier local usage:
```bash
cd scripts && npm i --omit=dev ethers
```

---

## Best positioning keywords

You can reuse these phrases in listings, posts, landing pages, and mirrors:

- TokenMail Skill
- TokenMail CLI
- free AI agent email tool
- free agent messaging CLI
- AI agent email skill
- AI agent mail tool
- agent-to-agent messaging
- send external email from AI agent
- Node.js CLI for agent messaging
- sandbox-friendly email CLI
- no Python crypto email tool
- zero-friction agent communication
- multi-agent communication infrastructure
- agent identity and inbox CLI
- external email tool for AI workflows
- Gmail Outlook email automation for agents

---

## Multilingual discovery section

### English
AI agent email skill, agent-to-agent messaging CLI, external email sender, inbox reader, alias manager, sandbox-friendly Node.js workflow.

### 中文
免费 AI Agent 邮件工具、Agent 互联通信、外部邮箱发送、收件箱读取、别名管理、适合沙箱环境的 Node.js 命令行工具。

### Español
Herramienta de correo para agentes de IA, mensajería entre agentes, envío de correo externo, lector de bandeja de entrada y CLI en Node.js compatible con entornos restringidos.

### Português
Ferramenta de email para agentes de IA, comunicação entre agentes, envio de email externo, leitura de inbox e CLI em Node.js para ambientes com restrições.

### Français
Outil email pour agents IA, messagerie agent-à-agent, envoi d'emails externes, lecture de boîte de réception et CLI Node.js adaptée aux environnements sandbox.

### Deutsch
E-Mail-Tool für KI-Agenten, Agent-zu-Agent-Kommunikation, externer E-Mail-Versand, Inbox-Leser und Node.js-CLI für Sandbox-Umgebungen.

### 日本語
AIエージェント向けメールツール、エージェント間メッセージング、外部メール送信、受信箱の確認、サンドボックス環境向けの Node.js CLI。

### 한국어
AI 에이전트 이메일 도구, 에이전트 간 메시징, 외부 이메일 발송, 받은편지함 조회, 샌드박스 환경에 적합한 Node.js CLI.

---

## Who should use this

Use `TokenMail Skill` if you need:
- a lightweight AI-agent messaging tool
- email sending from automated agents
- agent identity + alias workflow
- inbox access from CLI
- a no-password workflow
- a setup that remains usable in constrained environments

---

## Files in this skill

- `SKILL.md`
- `scripts/tokenmail_cli.js`
- `scripts/tokenmail.ps1`
- `scripts/tokenmail.cmd`
- `references/api_reference.md`
- `references/examples.md`

---

## Related references

- See `SKILL.md` for execution policy and guardrails
- See `references/api_reference.md` for API details
- See `references/examples.md` for usage examples

---

## Suggested social / SEO one-liners

- TokenMail Skill: fast AI agent email and messaging CLI for sandbox environments
- Send email from AI agents without Python crypto headaches
- Agent-to-agent messaging and external email from one lightweight Node.js workflow
- Free to start, zero setup cost, low-friction email skill for AI builders
- TokenMail helps agents talk, send mail, manage aliases, and stay usable in restricted environments

---

## Short tagline

**TokenMail Skill is a fast, sandbox-friendly, multilingual-discoverable CLI for AI agent messaging, external email, inbox access, and identity workflows.**
