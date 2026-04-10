# rules-npm

npm package management rules for AI coding agents. Enforces exact version pins (no `^` or `~` ranges), protects lockfiles from manual edits, blocks credential exposure in `.npmrc`, flags dangerous `postinstall` lifecycle scripts, guards `npm publish` and `npm unpublish` operations, and prevents supply-chain attacks via `npx`, `npm link`, and URL installs.

**14 rules · 4 files**

![rules-npm — AI agent npm package governance demo](demo.cast)

> [▶ Watch interactive demo on SigmaShake Hub](https://hub.sigmashake.com/ruleset/rules-npm)


## Install

```bash
ssg hub pull rules-npm
```

Available on the [SigmaShake Hub](https://hub.sigmashake.com) — the open registry for AI agent governance rules. Compatible with Claude Code, GitHub Copilot, Cursor, Windsurf, Aider, and any AI coding agent using the `ssg` hook protocol.

## Rules

### npm_write_safety.rules — package.json authoring (4 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-caret-or-tilde-deps` | DENY | error | Blocks `^` and `~` version ranges — use exact pins |
| `no-star-version` | DENY | error | Blocks `"*"` versions — non-reproducible installs |
| `no-edit-lockfile` | DENY | error | Blocks hand-editing `package-lock.json`, `yarn.lock`, `bun.lock` |
| `ask-postinstall-script` | ASK | warning | Flags `postinstall`/`preinstall` lifecycle scripts |

### npm_exec_hygiene.rules — Registry operations (4 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-npm-unpublish` | DENY | error | Blocks `npm unpublish` — use `npm deprecate` instead |
| `ask-npm-publish` | ASK | warning | Confirms version, build, and .npmignore before publish |
| `ask-npm-deprecate` | ASK | warning | Confirms deprecation message before marking a version |
| `ask-audit-fix-force` | ASK | warning | Warns that `--force` can upgrade across major versions |

### npm_write_secrets.rules — Credential exposure (3 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `no-npmrc-authtoken` | DENY | error | Blocks hardcoded `_authToken` in `.npmrc` — use `${NPM_TOKEN}` |
| `no-npmrc-password` | DENY | error | Blocks plaintext `_password` in `.npmrc` |
| `ask-npmrc-custom-registry` | ASK | warning | Flags custom registry URLs — dependency confusion risk |

### npm_exec_supply_chain.rules — Supply chain (3 rules)

| Rule | Decision | Severity | Description |
|------|----------|----------|-------------|
| `ask-npx-unversioned` | ASK | warning | Flags `npx <pkg>` without `@version` — always fetches latest |
| `ask-npm-link` | ASK | warning | Flags `npm link` — bypasses version resolution |
| `ask-npm-install-url` | ASK | warning | Flags installs from URLs/VCS refs — bypasses integrity checks |

## Why this matters

AI agents managing `package.json` commonly add caret ranges (`^1.2.3`) — the npm default — which can install breaking updates at `npm install` time. Hand-editing lockfiles corrupts the dependency graph. `postinstall` scripts and `npx` without version pins are supply-chain attack vectors. Hardcoded tokens in `.npmrc` expose credentials to anyone with repository access.

These rules make AI-managed Node.js projects reproducible, safe, and credential-secure by enforcing deterministic dependency pinning and requiring human approval for registry operations and supply-chain decisions.

## Compatible with

- npm, yarn (classic and Berry), pnpm, bun
- Node.js, Bun, Deno projects
- Works alongside Dependabot and Renovate — these rules operate at the agent tool-call level, not at install time

## About

Part of the [SigmaShake Hub](https://hub.sigmashake.com) — open-source governance rules for AI coding agents.
Install the `ssg` CLI to enforce these rules: `npm install -g @sigmashake/ssg`
