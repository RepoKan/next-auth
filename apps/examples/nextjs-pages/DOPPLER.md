# Optional Doppler setup for the Next.js Pages example

This example can keep using `.env.local`. Doppler is an optional secret-injection path for developers who do not want real provider credentials stored in a local environment file.

Official references:

- Doppler CLI: https://docs.doppler.com/docs/cli
- Service Tokens: https://docs.doppler.com/docs/service-tokens
- GitHub Actions: https://docs.doppler.com/docs/github-actions
- GitHub OIDC examples: https://docs.doppler.com/docs/github-oidc-examples
- Service Account Identities: https://docs.doppler.com/docs/service-account-identities

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

### Prefer GitHub OIDC when available

Use GitHub-issued OIDC with a Doppler Service Account Identity to obtain short-lived credentials. Do not use a Personal Token or broad CLI token for CI/live workloads.

Before enabling this path:

1. Confirm the Doppler workplace supports Service Account Identities (Team or Enterprise).
2. Grant the service account only the intended project/config access.
3. Configure exact audience and subject claims for this repository and the intended branch or protected environment. Inspect any customized GitHub subject template rather than copying another repository's claims.
4. Store the Doppler-generated Identity ID in the GitHub Actions variable `DOPPLER_SERVICE_IDENTITY_ID`; it is configuration metadata, not an API token.
5. Give only the secret-fetching job `id-token: write` and configure Doppler's Secrets Fetch action with `auth-method: oidc`, the Identity ID, project and config.
6. Mask fetched values, limit their scope to the consuming step, and verify OIDC exchange and secret retrieval without printing credentials.

This guide does not install a workflow or configure trust/access. Deployments outside GitHub require their own supported authentication design.

### Static-token fallback

If OIDC is unavailable, explicitly review a Service Token restricted to one project/config with the minimum required access and an expiry/rotation plan. Store it only in the platform's secret store. Never place its value in workflow source or logs.

## Repository hygiene

- Keep real `.env` / `.env.local` files untracked.
- Keep `.env.local.example` limited to variable names and safe placeholders.
- Never paste provider client secrets, `AUTH_SECRET`, Doppler tokens, private keys, or credentials into source, Issues, PRs, logs, or screenshots.
- If a credential is disclosed, revoke/rotate it before continuing.
