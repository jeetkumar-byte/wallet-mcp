# Keydris Wallet MCP

Buyer-side Stripe Machine Payments Protocol server. The `authorize_payment`
tool validates a seller challenge, asks the Keydris policy gateway to authorize
the payment, and issues a bounded Stripe Shared Payment Token.

## Requirements

- Node.js 22.22.2 or newer
- A Keydris agent routed through an MCP Kit Reader connection
- A buyer Stripe payment profile enabled by the agent policy

## Configure

Copy `.env.example` to `.env` when running locally. The gateway defaults to
`https://dev.api.keydris.com/gateway/credentials`.

Normal MCP requests carry the single-use token in
`params._meta["keydris/kit_action_token"]`.

## Develop

```bash
npm install
npm run typecheck
npm run build
npm run dev
```

## Deploy to Manufact

```bash
npx --no-install mcp-use login
npm run deploy -- --name keydris-wallet-mcp --open
```

After the first GitHub-backed deployment, pushes to the configured production
branch redeploy this server automatically.
