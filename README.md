# TeamCredits SaaS Starter

A starting point for SaaS apps: Google sign-in, PayPal subscriptions, team invites and a shared credit balance, with the billing edge cases handled.

> **Status: in development.** This repo is the plan and the write-up; the interactive demo is being built next.
> **Demo preview:** https://julynard.github.io/teamcredits-saas-starter/ (becomes the live demo when it ships).

Part of [Julynard Tago-on's portfolio](https://julynardtago-on.netlify.app/#projects). Based on the team-collaboration SaaS at CoreProc where I was the top contributor (600+ commits).
Client code is private, so this is a clean-room demo with fictional data. No client names or real data.

## What the demo will show

- [ ] Pick a plan, go through a simulated PayPal checkout, land on an active subscription
- [ ] **Webhook control panel**: fire payment completed / payment failed / cancelled / duplicate events and watch the state machine (Active → Past due → Grace period → Suspended → Reactivated)
- [ ] A time-travel slider shows abandoned checkouts being cleaned up after 24h
- [ ] Duplicate webhooks are ignored (idempotency on the gateway event id)
- [ ] **Team plan**: invite, accept, decline, resend and remove members; a shared credit balance drains as members use the app

Every key action will have an **"In production"** note explaining how the real system does it.

## How the production system works

The real system runs on Laravel. The demo simulates these behaviours in the browser so it can stay fast and always on.

- Laravel 12 + Inertia v2 + Vue 3/TypeScript, Fortify and Socialite (Google) for auth
- Subscriptions modelled as subscriptions + periods with lifecycle timestamps; plans mapped to PayPal product/plan IDs
- Billing logic in single-purpose Actions; scheduled commands apply grace periods and mark abandoned subscriptions
- Payment logs keyed by gateway event id with `updateOrCreate`, so a webhook delivered twice changes nothing
- A credit ledger (wallet) instead of a mutable balance column, plus admin KPI metrics in Laravel Nova

## Planned stack

- Vue 3 + TypeScript + Vite
- Tailwind CSS v4
- Pinia (state saved in the browser, with a **Reset demo** button)
- Vitest for the domain logic, ESLint + Prettier + `vue-tsc` in CI
- GitHub Actions → GitHub Pages (free, always on)

## Roadmap

1. Project scaffold, CI (lint, type-check, test, build) and GitHub Pages deploy
2. Domain logic with Vitest tests
3. UI with seeded data, guided steps and "In production" notes
4. Screenshot/GIF in this README and the live demo link

## Author

**Julynard Tago-on**, Laravel / PHP full-stack engineer · [Portfolio](https://julynardtago-on.netlify.app) · [GitHub](https://github.com/Julynard) · [LinkedIn](https://www.linkedin.com/in/julynard-tago-on/)

## License

[MIT](LICENSE)
