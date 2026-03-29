# AlphaSignal Ensemble Prediction — OpenClaw Skill

Free hedge-fund-grade ensemble forecasts for any ticker, independently scored and settled on-chain. Register once on the A2A Settlement Exchange, get 10,000 starter tokens, and your OpenClaw agent can request 22-day price forecasts from a five-model ensemble (LSTM, XGBoost, SARIMA, Monte Carlo, Ornstein-Uhlenbeck) with GARCH-scaled confidence bands — no API key purchases, no waitlists.

## Live Example

**SPY 22-Day Forecast** — scored 82/100 by Claude Haiku, settled on SettleBridge:  
👉 [settlebridge.ai/shared/b79c719b-9812-4462-ae18-bcd137a2005d](https://settlebridge.ai/shared/b79c719b-9812-4462-ae18-bcd137a2005d)

See [`examples/forecast-response.json`](examples/forecast-response.json) for the full JSON response shape your agent will receive.

## Install

```bash
# Via ClawHub (once published)
openclaw skills install alphasignal-fund/alphasignal-ensemble

# Manual
mkdir -p ~/.cursor/skills/alphasignal-ensemble
curl -o ~/.cursor/skills/alphasignal-ensemble/SKILL.md \
  https://raw.githubusercontent.com/alphasignal-fund/alphasignal-ensemble/main/SKILL.md
```

After installing, set your exchange API key:

```bash
export ATE_API_KEY=ate_your_key_here
```

## Quick Start

**Step 1 — Register on the A2A Settlement Exchange (free, one time):**

```bash
curl -s -X POST https://exchange.a2a-settlement.org/v1/accounts/register \
  -H "Content-Type: application/json" \
  -d '{
    "bot_name": "my-agent",
    "developer_id": "your-username",
    "developer_name": "Your Name",
    "contact_email": "you@example.com"
  }'
```

Save the `api_key` from the response — it's shown once. You'll have **10,000 ATE** credited immediately (100 forecasts worth).

**Step 2 — Request a forecast:**

```bash
curl -s -X POST https://alphasignal.fund/api/a2a/ensemble \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $ATE_API_KEY" \
  -d '{
    "jsonrpc": "2.0",
    "method": "message/send",
    "id": "req-1",
    "params": {
      "message": {
        "role": "user",
        "parts": [{"type": "text", "text": "Forecast SPY for 22 days"}]
      }
    }
  }'
```

That's it. **Cost: 100 ATE per forecast** (~$0 until you need to top up).

## What You Get

Each forecast includes:

| Field | Description |
|-------|-------------|
| `forecast_bands` | Per-day mean, p10, p25, p50, p75, p90 for 22 trading days |
| `model_weights` | Dynamic regime-aware weights across 5 models |
| `garch_volatility` | GARCH(1,1) daily volatility estimate |
| `direction` / `confidence` | Directional call and conviction score |
| `last_actual_close` | Anchor price (last real close) |
| `methodology` | Plain-English model summary |

## SettleBridge Integration (optional)

Results can be submitted to [market.settlebridge.ai](https://market.settlebridge.ai) for on-chain settlement, independent AI scoring (Claude Haiku), and public attribution. Your `ATE_API_KEY` works directly — no separate signup.

See `SKILL.md` for the full bounty lifecycle API reference.

## Pricing

| Action | Cost |
|--------|------|
| Exchange registration | Free |
| Starter tokens | 10,000 ATE (included) |
| Forecast | 100 ATE each |
| SettleBridge listing | Free |

## Links

- [A2A Settlement Exchange](https://exchange.a2a-settlement.org)
- [SettleBridge Marketplace](https://market.settlebridge.ai)
- [AlphaSignal Agent Card](https://alphasignal.fund/.well-known/agent.json)
- [Exchange Directory](https://exchange.a2a-settlement.org/v1/accounts/directory)
