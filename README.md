# Eral

AI layer for WokSpec. Ships as a Cloudflare Worker API, an embeddable widget, and a browser extension.

**Live:** [eral.wokspec.org](https://eral.wokspec.org) · **Source available:** [FSL-1.1-MIT](./LICENSE)

---

## What it is

Eral is the AI backbone across every WokSpec product. The same model and memory layer powers the chat widget on wokspec.org, the AI companion in WokGen and Vecto, the news analysis in WokPost, and the browser extension.

```
apps/
  api/        # Cloudflare Worker — Hono, chat/generate/analyze endpoints
  extension/  # Browser extension — Plasmo, works in Chrome/Firefox/Edge
  widget/     # Embeddable React widget — drop into any page
```

---

## API

All endpoints require `Authorization: Bearer <jwt>` (WokSpec JWT, shared secret).

| Method | Path | Description |
|--------|------|-------------|
| `GET`  | `/health` | Health check |
| `GET`  | `/v1/status` | Provider info and model variants |
| `POST` | `/v1/chat` | Chat with persistent session memory |
| `GET`  | `/v1/chat/sessions` | List sessions |
| `GET`  | `/v1/chat/:sessionId` | Session history |
| `DELETE` | `/v1/chat/:sessionId` | Clear session |
| `POST` | `/v1/generate` | Content generation (posts, code, docs, prompts) |
| `POST` | `/v1/analyze` | Content analysis (summarize, review, extract) |

### AI providers

| Provider | Used for |
|----------|----------|
| OpenAI GPT-4o | Default chat and generation |
| Cloudflare Workers AI (Llama 3.1 8B) | Fallback, edge-native, free tier |
| Groq (Llama 3.3 70B) | High-speed inference for WokGen/Vecto |

---

## Browser Extension

**Eral Web Extension** — available for Chrome, Firefox, and Edge (Plasmo-based).

Built from `apps/extension/`. The extension:
- Adds Eral to any webpage via a floating panel
- Clips selected text to Eral memory
- Runs research and meeting modes (in development)

```bash
cd apps/extension
npm install
npm run dev     # dev mode — loads unpacked in browser
npm run build   # production build
```

---

## Widget

Drop the Eral widget into any WokSpec product (or external site):

```tsx
import EralWidget from '@wokspec/eral-widget';

// In your layout
<EralWidget apiUrl="https://api.wokspec.org/v1/chat" />
```

---

## API development

```bash
cd apps/api
npm install
npm run dev       # wrangler dev on :8788
npm run deploy    # deploy to Cloudflare Workers
```

### Required secrets

```bash
wrangler secret put JWT_SECRET       # must match WokAPI
wrangler secret put OPENAI_API_KEY   # optional — GPT-4o
```

### Required KV namespace

```bash
wrangler kv namespace create KV_MEMORY
# add the id to wrangler.toml
```

---

## License

Source available under [FSL-1.1-MIT](./LICENSE). Free for personal and non-commercial use. Converts to MIT two years after publication.
