# FluentXVerse Student

The learner-facing web application for FluentXVerse, an online English tutoring platform. Students discover tutors, manage lesson credits and bookings, and join live lessons from their browser.

Production entry point: https://student.fluentxverse.com

## Responsibilities

- Social authentication through Privy, with Google, Apple, and X login options subject to provider configuration.
- Tutor discovery, profiles, favorites, availability, and booking workflows.
- Student schedules, learning materials, lesson feedback, inbox, and account settings.
- Live classroom audio/video using Cloudflare RealtimeKit, with server-authorized room access and recording notices.
- Client workflows for privacy-oriented lesson passes and reviews when explicitly enabled by backend policy.

## Architecture

SolidJS, TypeScript, Vite, Solid Router, Privy's core SDK, and Cloudflare RealtimeKit. Cloudflare Workers serves the static frontend. The separately deployed [application server](https://github.com/fluentxverse/fluentxverse-server) owns authentication sessions, booking authorization, and business rules. GMR Engine owns managed Web3 execution; private signing keys must not enter this frontend.

`fluentxverse-student-solid-prototype/` contains the application, assets, configuration, and tests. `shared/` contains the shared source files required by this revision.

## Local Development

Use Node.js 22+, npm, and Bun. From the repository root:

```bash
npm run setup
cp fluentxverse-student-solid-prototype/.env.example fluentxverse-student-solid-prototype/.env.local
npm run dev
```

Open http://127.0.0.1:5176. Configure a development API and the public `VITE_PRIVY_APP_ID` in `.env.local`. Register the exact development origin with the authentication provider. Never put the Privy app secret in a `VITE_` variable.

```bash
npm test
npm run build
npm run preview
```

`npm test` runs the standalone unit suite. Backend harnesses under `backend/`, live-provider browser tests, and cross-app integration checks require the monorepo and isolated test infrastructure; do not point mutation tests at production.

## Deployment and Privacy Boundaries

The production build is written to `fluentxverse-student-solid-prototype/dist/`. The app's Wrangler configuration describes its existing production domains; review account, routes, environment, and acceptance checks before any manual deployment. This repository's CI checks source only and does not deploy.

Social authentication does not make an account anonymous. Media is accessible to authorized participants and recording processors. Privacy-oriented passes, reviews, screening, and settlement have independent rollout gates; their presence in source is not a claim that every feature is live, audited, or shielded on mainnet.

## Repository Provenance

This repository is an app-focused export of [the FluentXVerse monorepo](https://github.com/fluentxverse/fluentxverse-monorepo). The monorepo remains the source of truth; submit cross-application changes there. Existing directory names are retained so shared imports remain valid. The historical student/tutor folder suffix does not indicate a Preact app: both active web clients use SolidJS.

Exported source commit: [`0b10bf263db4`](https://github.com/fluentxverse/fluentxverse-monorepo/tree/0b10bf263db4d9bb599a15f17b41d6911f30b91e). See [SOURCE.json](SOURCE.json) for the exact scope. Local uncommitted edits, environment files, runtime data, curriculum backups, and generated proving artifacts are not published. Original Git history is not copied.

Cross-app parity tests and full deployment orchestration remain in the monorepo. The checks in this repository do not establish production acceptance, security audit completion, or proof-system review. No production deployment is triggered by this export.

## Security and Licensing

Never commit API tokens, seed phrases, private keys, student records, recordings, or real assessment evidence. Use synthetic fixtures in tests. Report security issues privately to support@fluentxverse.com rather than opening an issue containing sensitive data.

 third-party dependencies retain their own licenses.
