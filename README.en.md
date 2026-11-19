<p align="center">
  <img src="shared/brand/icon-512.png" alt="cofounder" width="128" height="128">
</p>

# cofounder

An AI-powered co-founder that guides you from idea to a web app published on the internet — no programming experience required.

[Clique aqui para a versão em Português](README.md)

![cofounder in action](https://raw.githubusercontent.com/locaweb/cofounder-plugin/076f5befb0993b09676e970f1844a1345f693c46/demo.webp)

## What it does

The cofounder is a set of [skills](skills/) for your coding agent (Claude Code, Codex, Cursor and others). It talks to you in plain language, writes the product spec with you, builds the app, tests it, and deploys it to Locaweb Cloud.

## Installation

Download `install.sh` from the latest [locaweb/cofounder release](https://github.com/locaweb/cofounder/releases/latest) and check it as described in [Verifying the installation](#verifying-the-installation). Then open a terminal in the folder where you want to create your project and run:

```bash
bash install.sh
```

The script is idempotent (safe to run again) and:

- installs the development tools (mise, Podman, GitHub CLI and others);
- installs the cofounder skills into the project via [`npx skills`](https://github.com/vercel-labs/skills);
- configures your agent to load the cofounder at the start of every session.

Then open your agent in the same folder (for example, run `claude`) and say hi.

### As a Claude Code plugin

Inside Claude Code, run:

```
/plugin marketplace add locaweb/cofounder-plugin
/plugin install cofounder@locaweb
```

To start, open Claude Code in your project folder and run `/cofounder:iniciar` (or ask something like "I want to build an app"). The first time, the cofounder writes an `AGENTS.md` and a `CLAUDE.md` into the project: from the next session on, it starts on its own in that folder.

To get updates automatically, turn on auto-update in `/plugin` → **Marketplaces** → **locaweb** → **Enable auto-update**. To update manually, run `claude plugin update cofounder@locaweb` in your terminal.

### As a Codex plugin

In your terminal, add the marketplace:

```
codex plugin marketplace add locaweb/cofounder-plugin
```

Then open `codex`, run `/plugins`, install **cofounder** from the **locaweb** marketplace and start a new session. In your first session in each project, ask to use the cofounder (for example, "I want to build an app with the cofounder"); it writes an `AGENTS.md` into the project and, from the next session on, starts on its own in that folder. Also in the first session, the cofounder asks for your approval once to configure the project (`.codex/config.toml` with full access, and the folder marked as trusted in Codex), so it can install tools and run commands without asking every time; this takes effect from the next session. To update, run `codex plugin marketplace upgrade locaweb`.

## How the cofounder starts

The plugin runs nothing on its own when a session starts: the cofounder only acts after you call it.

1. **First time in a project:** open your agent in an empty folder (for a new project) or in a project the cofounder created, and run `/cofounder:iniciar` in Claude Code. In Codex, ask to use the cofounder. Usually, asking something like "I want to build an app" is enough.
2. **Project activation:** in that first session, the cofounder writes an instructions block into the project's `AGENTS.md` and a `CLAUDE.md` that imports it. If those files already exist, what you wrote is kept.
3. **Next sessions:** the agent reads those files when it opens the folder and starts the cofounder on its own, without `/cofounder:iniciar`.

To turn it off in a project, delete the block between `<!-- cofounder:begin -->` and `<!-- cofounder:end -->` in `AGENTS.md` and `CLAUDE.md`.

## Use cases

- **"I have an idea for an app"** — the cofounder asks questions, writes a PRD and proposes the first version.
- **"Run the app on my computer"** — starts the database, backend and frontend locally so you can try it out.
- **"Add a login page" / "fix this bug"** — implements the change with automated tests and checks it visually.
- **"Put it on the internet"** — creates the GitHub repository, the preview and production environments, and deploys to Locaweb Cloud.
- **"Use my own domain"** — configures a custom domain on the production environment.

## Stack

| Layer | Technology |
|-------|------------|
| Backend | Go (`net/http`, `pgx`, `sqlc`) |
| Frontend | React + TypeScript, React Router, Tailwind CSS, shadcn/ui |
| Database | Postgres (Supabase locally, via Podman) |
| Tests | Go tests, Vitest, Playwright |
| Deploy | GitHub Actions → Locaweb Cloud, as a single container |

## Security

The skills are **static files installed locally** in your project: Markdown instructions and shell scripts that you can read before using. They do not download or execute unauthorized remote code. Every command runs on your machine, through your agent and with your permission. The tools installed by `install.sh` come from their official sources, with versions pinned and checksums or signing keys verified.

### Verifying the installation

`install.sh` and a tarball of the skills are published as [GitHub Releases of locaweb/cofounder](https://github.com/locaweb/cofounder/releases), where the skills are developed; each plugin version is published as a [GitHub Release of this repository](https://github.com/locaweb/cofounder-plugin/releases) with a tarball of the plugin. Every artifact comes with a `SHA256SUMS` file and a provenance attestation proving it was built by the `release.yml` workflow of its repository from a specific commit. To verify them, use an authenticated [GitHub CLI](https://cli.github.com):

```bash
VERSION=v0.29.1
gh release download "$VERSION" --repo locaweb/cofounder --dir "cofounder-$VERSION"
cd "cofounder-$VERSION"
sha256sum -c SHA256SUMS   # on macOS: shasum -a 256 -c SHA256SUMS
gh attestation verify install.sh --repo locaweb/cofounder
gh attestation verify "cofounder-skills-$VERSION.tar.gz" --repo locaweb/cofounder
```

If everything passes, run the verified `install.sh` with `bash install.sh`. For the plugin tarball, download the same version with `--repo locaweb/cofounder-plugin` and run `gh attestation verify "cofounder-plugin-$VERSION.tar.gz" --repo locaweb/cofounder-plugin`.

## Development

The skills are developed in [locaweb/cofounder](https://github.com/locaweb/cofounder), which also serves `npx skills` installs. This repository's `skills/` folder is a copy of one of its tags, made by the `sync-skills.yml` workflow: do not edit it here, the next copy overwrites it. Only the plugin manifests, the `/cofounder:iniciar` command and the sync and release workflows live here.

## Support

Found a problem or have a suggestion? Open an issue on [GitHub Issues](https://github.com/locaweb/cofounder-plugin/issues).

## Legal

- [Privacy Policy](TODO-privacy-policy-url)
- [Terms of Use](TODO-terms-of-use-url)
- License: Functional Source License (FSL-1.1-ALv2) — see [LICENSE](LICENSE).
