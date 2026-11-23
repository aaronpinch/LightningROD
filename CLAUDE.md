# CLAUDE.md

## This repository is PUBLIC

`aaronpinch/LightningROD` is a **public** fork of `SquidBytes/LightningROD`.
Every file, commit message, branch name, PR description, and issue comment is
world-readable and stays in git history even after it is deleted. Treat every
change as if it were being published.

## Never commit

- **Credentials:** Home Assistant long-lived tokens, Ford / FordConnect OAuth
  client IDs, secrets, access or refresh tokens, `LR_SECRET_KEY`, database
  passwords, `DATABASE_URL` values with real credentials, API keys, SSH keys,
  certificates (`*.pem`, `*.key`, `*.p12`).
- **Env files:** `.env` or any `.env.*` other than `.env.example`
  (placeholders only).
- **Vehicle identifiers:** real VINs, license plates, FordPass device or
  account IDs. Use the placeholder `1FT6W1EV0NWG00000` or `YOUR_VIN`.
- **Location data:** real GPS coordinates, home or work addresses, charging
  history, trip logs, or anything that reveals where someone lives, works, or
  travels. Sample data must be generated and use generic city-centre
  coordinates.
- **Personal identity:** real names, email addresses, phone numbers, usernames,
  or account handles (other than public GitHub handles already in history).
- **Network details:** real hostnames, Nabu Casa / DuckDNS / Tailscale / ngrok
  URLs, public IPs, or LAN IPs from a real network. Use `homeassistant.local`,
  `192.168.1.100`, `example.com`, or `*.test`.
- **Local data:** databases (`*.db`, `*.sqlite*`, `pgdata/`), SQL dumps,
  backups, raw Home Assistant / Ford API payload captures, real CSV exports,
  logs, and screenshots that show real data.
- **Local tooling state:** `.planning/`, `notes/`, `CLAUDE.local.md`,
  `.claude/settings.local.json`, `pii-tokens.txt`.

Test fixtures must use obviously fake values. When a fake value looks like a
secret (for example a dummy `ha_token`), add it to the allowlist in
`.gitleaks.toml` rather than weakening the rules.

## Safeguards: enable them in every clone

```bash
git config core.hooksPath .githooks
```

- `.githooks/pre-commit` runs **gitleaks** on staged changes. It blocks the
  commit if gitleaks is missing or finds a leak. It also runs
  `scripts/checker.py`, which blocks personal tokens listed in
  `~/.config/lightningrod/pii-tokens.txt`, and ruff.
- `.githooks/commit-msg` enforces commit-message hygiene (see below).
- CI (`.github/workflows/gitleaks.yml`) re-runs gitleaks on every push and PR
  in case a local hook was skipped.
- GitHub secret scanning and push protection are enabled on the repository.

Do not bypass hooks with `--no-verify` to get a leak past them. If something
sensitive is committed, stop and tell the repo owner. Rotate the credential
first: deleting it in a later commit does not remove it from history.

## Commit messages

Format: `<type>(<scope>): <plain-English behavior change>`, subject at most
72 characters. Do not reference internal planning IDs, "Phase NN" prose,
planning artifact filenames, or `Co-Authored-By:` trailers (upstream
convention).

## Workflow

- Make changes on a branch and open a PR. Batch related changes into one PR so
  they are easy to revert.
- Record outstanding follow-ups as GitHub issues, not TODO comments. Issue
  text must follow the same rules as code.
