# Work on Odysseus — devclone20

Fork de `pewdiepie-archdaemon/odysseus` onde guardamos as nossas modificações,
estudos e código adicional. Registo cronológico das alterações.

## 2026-06-20 — Email: banner "Pending approval"

**Problema:** com o gate de segurança `agent_email_confirm` ligado (default), a
tool `send_email` do agente põe o email em `scheduled_emails` com
`status='agent_draft'` e nunca o envia — mas **não havia UI para aprovar**, por
isso os emails ficavam presos. Os endpoints já existiam
(`GET /api/email/pending`, `POST /api/email/pending/{id}/approve`,
`DELETE /api/email/pending/{id}`); só faltava o frontend.

**Alteração:** `static/js/emailInbox.js` — banner autónomo no topo do painel de
Email (`#email-pending-approvals`, acima de `#email-list`):
- Renderizado no `finally` de `loadEmails`, por isso aparece **mesmo quando o
  IMAP falha** (os pendentes vêm da DB local, não do inbox).
- Lista cada email staged (destinatário · assunto · preview) com **Approve &
  send** (→ approve endpoint, envia já) e **Reject** (→ delete endpoint).
- Atualiza-se após cada ação. Usa as variáveis de tema (`--red`/`--green`/
  `--border`/`--panel`), por isso encaixa em qualquer tema.

## Notas de configuração local (NÃO versionadas — ficam em `data/`, gitignored)
- `data/settings.json`: provider Anthropic ligado (Opus 4.8); `agent_email_confirm`
  alternável em Settings → AI Defaults → "Email Safety".
- App macOS clicável gerada com o `build-macos-app.sh` já existente no repo
  (`dist/Odysseus.app` / `.dmg` — gitignored).
