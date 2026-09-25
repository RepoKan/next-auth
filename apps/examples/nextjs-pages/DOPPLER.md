# Optional Doppler setup for the Next.js Pages example

This example can keep using `.env.local`. Doppler is an optional secret-injection path for developers who do not want real provider credentials stored in a local environment file.

Official references:

- Doppler CLI: https://docs.doppler.com/docs/cli
- Service Tokens: https://docs.doppler.com/docs/service-tokens
- GitHub Actions: https://docs.doppler.com/docs/github-actions

## Local development

Install the Doppler CLI using the official instructions, then authenticate interactively:

```bash
doppler login
```

From this directory, map the repository to the appropriate Doppler project/config:

```bash
doppler setup
```

Create the same variable names documented in `.env.local.example` inside that Doppler config. Do not commit the values.

Start the example with the secrets injected into the process environment:

```bash
doppler run -- pnpm dev
```

Auth.js reads the same environment-variable names whether they came from `.env.local` or were injected by Doppler.

## CI and live environments

Do not use a Doppler Personal Token or broad CLI token for CI/live workloads. Doppler recommends a Service Token restricted to one project/config for live environments.

Keep the Service Token in the CI platform's secret store or use Doppler's supported GitHub integration. Workflow files should reference the platform-managed secret identifier, never contain the token value.

## Repository hygiene

- Keep real `.env` / `.env.local` files untracked.
- Keep `.env.local.example` limited to variable names and safe placeholders.
- Never paste provider client secrets, `AUTH_SECRET`, Doppler tokens, private keys, or credentials into source, Issues, PRs, logs, or screenshots.
- If a credential is disclosed, revoke/rotate it before continuing.
