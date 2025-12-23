# Copilot / AI Agent Instructions — fun-with-dev-hello-world

Purpose: give an AI coding agent the minimal, actionable knowledge to be productive in this repository.

1) Big picture
- This repository contains small example projects. The primary app for day-to-day work is the Angular app at `hello-angular-app/`.
- `hello-angular-app` is an Angular 21 application configured for SSR (server-side rendering) using an Express server. Server entry points: `src/main.server.ts` and `src/server.ts`.

2) Key files and where to look first
- Project root: `hello-angular-app/package.json` (scripts, dependencies). Example scripts: `build`, `start`, `serve:ssr:hello-angular-app`.
- Build config: `hello-angular-app/angular.json` (look for `output`/`assets` and `defaultConfiguration`). The production build outputs to `dist/hello-angular-app`.
- Server runtime: `hello-angular-app/src/server.ts` (Express SSR server). The packaged server is under `dist/hello-angular-app/server` after build.
- Static/public assets: `hello-angular-app/public` (declared as `assets` in `angular.json`).

3) Common developer workflows (explicit commands)
- Install deps (CI/local):

```bash
cd hello-angular-app
npm ci
```

- Build production (client + server bundle for SSR):

```bash
npm run build
# output -> dist/hello-angular-app
```

- Run SSR server locally (after build):

```bash
# from repo root
node dist/hello-angular-app/server/server.mjs
# or use the npm script
npm run serve:ssr:hello-angular-app
```

- Run the dev server (hot reload):

```bash
npm start
```

4) Tests and tooling
- `vitest` and `@angular/build:unit-test` exist as dev dependencies, but there are no elaborate test scripts in `package.json` beyond `test: ng test`. If asked to add tests, run the Angular unit test builder or add a dedicated Vitest runner.

5) Conventions & patterns specific to this repo
- Keep changes scoped: most work will live inside `hello-angular-app/`. Avoid touching unrelated top-level files unless implementing a cross-project change.
- Asset placement: put static files in `hello-angular-app/public` (angular.json maps this into build). CSS lives in `src/styles.css`.
- SSR changes: if you modify server-side rendering behavior, update `src/server.ts` and rebuild (production SSR requires the compiled `server.mjs`).

6) CI / deployment notes
- A GitHub Actions workflow exists at `.github/workflows/deploy-to-cpanel.yml` which:
  - runs `npm ci` in `hello-angular-app`
  - runs the Angular build
  - uploads `dist/hello-angular-app` to cPanel via FTPS using repository secrets.
- For alternative deployments (SFTP, Node host, Docker), prefer packaging the `dist/hello-angular-app` output and the server entry in a minimal Node container.

7) Integration points & external dependencies
- Primary runtime: Node.js + npm (package.json indicates `npm@10.x` as packageManager; Node 18+ is a safe choice).
- Angular 21 and Angular SSR packages are used — follow Angular build targets in `angular.json` when modifying builds.

8) Helpful examples to include in PRs
- When changing build output or public assets, include a short verification step in PR description showing `ls -la hello-angular-app/dist/hello-angular-app` and which files changed.
- When modifying SSR behavior, include a short note showing how to start the server and a smoke-test (e.g. curl localhost:4000/).

9) What the agent should avoid changing automatically
- Do not change global repo structure or top-level `package.json` unless the change is required for the `hello-angular-app` and accompanied by tests or a clear rationale.
- Avoid flipping Angular or TypeScript configuration without running a build — changes to `angular.json` or `tsconfig.*.json` must be validated by a successful production build.

If anything important is missing here (build flags, local dev ports, extra CI secrets, or preferred Node version), tell me and I'll update this file.
