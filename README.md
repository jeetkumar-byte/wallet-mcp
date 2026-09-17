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

The agent supplies amount, currency, method, and transaction type. It does not
supply a Keydris payment connection id. During redemption, Keydris selects the
single buyer Stripe connection allowed by the runtime session's policy version.
Calls fail closed when no buyer connection or more than one buyer connection
matches.

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

## Kit Reader boundary

`src/keydris` is an unchanged vendored subset of the open-source Apache-2.0
Kit Reader. `src/keydris-payment` is this server's adapter for sending the
downstream target and payment context to the Keydris gateway. The backend alone
evaluates payment policy and chooses whether credentials may be released.
