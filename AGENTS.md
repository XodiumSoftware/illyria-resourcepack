# Illyria Resource Pack — Agents Context

## Project at a Glance

- **Name:** illyria-resourcepack
- **Type:** Minecraft resource pack
- **MC Version:** 26.3
- **Format:** Standard Minecraft resource pack layout under `src/`
- **Build / Optimize:** [PackSquash](https://github.com/ComunidadAylas/PackSquash) via GitHub Actions
- **Related Projects:**
    - [IllyriaPlus](https://github.com/XodiumSoftware/IllyriaPlus) — Server plugin that serves this pack to players
    - [illyria-wiki](https://github.com/XodiumSoftware/illyria-wiki) — Documentation wiki that uses textures from this repo

## Technology Stack

| Category         | Technology     | Purpose                       |
| ---------------- | -------------- | ----------------------------- |
| **Pack Builder** | PackSquash     | ZIP creation + optimization   |
| **CI/CD**        | GitHub Actions | Nightly release of `pack.zip` |
| **Versioning**   | Git (SSH)      | Asset versioning              |

## Quick Commands

The repository is a plain resource pack; there is no local build script. PackSquash is run in CI.

```bash
# Validate the pack layout (pack.mcmeta, assets/, overlay_mod_support/)
ls src/
```

## Project Structure

```
illyria-resourcepack/           # Repository root
├── .agents/                    # Agent skills and context
│   └── skills/                 # Task-specific agent skills
│       └── cleanup-resourcepack-assets/  # Remove leftover source folders
├── .github/
│   └── workflows/
│       └── ci.yml              # PackSquash build + nightly release
├── src/                        # Resource pack source
│   ├── assets/                 # Minecraft assets (textures, models, lang, etc.)
│   │   ├── minecraft/          # Vanilla overrides
│   │   └── illyriacore/        # Custom IllyriaCore namespace assets
│   ├── overlay_mod_support/    # Optional modded-client overlays
│   ├── pack.mcmeta             # Pack metadata
│   └── pack.png                # Pack icon
├── README.md
├── AGENTS.md                   # This file
└── LICENSE.md
```

## Conventions

- All shipped pack contents live under `src/`.
- Custom assets should use the `illyriacore` namespace when possible.
- Vanilla overrides go under `src/assets/minecraft/`.
- Language overrides are kept in `src/assets/minecraft/lang/en_us.json`.
- Texture paths referenced by the wiki and skills use `src/assets/minecraft/textures/`.

## CI/CD

GitHub Actions workflow in `.github/workflows/ci.yml`:

- Builds `pack.zip` from `src/` with PackSquash.
- Attaches `pack.zip` to the `nightly` prerelease on this repository.
- The IllyriaPlus `ResourcePackMechanic` downloads from `https://github.com/XodiumSoftware/illyria-resourcepack/releases/download/nightly/pack.zip`.

## Common Tasks

### Add a Custom Texture

1. Place the texture under `src/assets/<namespace>/textures/<type>/<name>.png`.
2. Add or update the model / lang / other definitions as needed.
3. Let CI rebuild the pack on push.

### Verify the Pack

The authoritative validation happens in GitHub Actions. Locally you can sanity-check:

- `src/pack.mcmeta` exists and has valid JSON.
- `src/assets/` contains the expected namespaces.
- `src/pack.png` is a valid PNG.

## Notes for Agents

- **Source of truth** — This repo is now the single source of truth for all resource pack assets.
- **IllyriaPlus no longer contains the pack** — Do not add or reference `IllyriaPlus/IllyriaResourcePack/` paths.
- **Wiki textures** — When adding icons for the wiki, copy from `src/assets/minecraft/textures/` into the wiki's `src/img/<category>/`.
- **No nested Config objects** — N/A for this repo; resource packs are data-only.

## Claude Code Workflow

**When creating tasks:**

- Number tasks in the name (e.g., "1. Add banner pattern texture", "2. Update painting models").

**After completing each task:**

- Ask the user if they want to git commit the changes or adjust before committing.

**After making edits:**

- Verify the affected files are under `src/`.
- Update this `AGENTS.md` if build steps or conventions change.
