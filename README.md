# anvil-meta-registry

A registry of [anvil](https://github.com/techneut92/anvil) registries. Lists every
public anvil registry discovered across GitHub / Codeberg / GitLab. Users can
treat the whole anvil ecosystem as one subscription by pointing
`anvil discover` / Phase 2 tooling at this repo.

## What's in here

- `anvil-meta-registry.toml` — every known anvil registry, keyed by short
  name. Today: hand-curated. Tomorrow: rewritten by `.github/workflows/
  discover.yml` on a daily cron.

## Discovery model

Three sources, deduped + validated before they're written here:

1. **GitHub** — `gh search repos --topic=anvil-registry`.
2. **Codeberg** — `GET https://codeberg.org/api/v1/repos/search?topic=anvil-registry`.
3. **GitLab** — `GET https://gitlab.com/api/v4/projects?topic=anvil-registry`.

For each candidate, the bot clones at HEAD and confirms the repo has a
valid `anvil-registry.toml` at the root (parses as TOML, `schema_version
= 1`, has `[packages.X]` entries). Failures are skipped silently — the
bot doesn't open issues on third-party repos.

## Getting your registry listed

Two ways:

1. **Add the `anvil-registry` topic to your repo.** The discovery cron
   will pick it up within 24 hours on its next run. Works on
   GitHub / Codeberg / GitLab.
2. **PR a `[registries.<name>]` entry to `anvil-meta-registry.toml`.**
   Faster than waiting on the cron, and the only path for hosts the bot
   doesn't crawl (a private gitea, your own server, etc).

The `host` field is informational — anvil itself only uses `url`.

## Removing your registry

Same as adding: untag the repo and wait for the cron, or PR a deletion.
