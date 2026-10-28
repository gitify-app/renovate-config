<div align="center">

# Gitify Renovate config

**Shared [Renovate][renovate] presets & validation workflow** for the [Gitify organization][gitify-org].

[![Renovate config validation][validator-badge]][validator-actions] [![License][license-badge]][license]

</div>

> [!NOTE]
> Public repository. It hosts the organization's shared Renovate preset, which every Gitify repository inherits. Organization [community health files][github-health] live in [`gitify-app/.github`][dot-github].

## 🔧 Usage

Inherit the baseline preset from a repository's `renovate.json`:

```jsonc
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>gitify-app/renovate-config"],
  "packageRules": [
    { "description": "repo-specific rule" }
  ]
}
```

Repository-level settings and later `packageRules` take precedence over the shared baseline.

Because this repository follows Renovate's shared-preset naming convention, **new repositories are onboarded onto the preset automatically** — Renovate checks `gitify-app/renovate-config/default.json` before any other org default and suggests `extends: ["local>gitify-app/renovate-config"]`.

### ✅ Validating a repo's config

Consumer repositories can call the reusable workflow to validate their own `renovate.json`:

```yaml
jobs:
  validate:
    permissions:
      contents: read
    uses: gitify-app/renovate-config/.github/workflows/renovate-config-validator.yml@main
```

## ⚖️ Policy

- 🗓️ Weekly updates, with grouped Vite+, GitHub Actions, and Node.js runtime updates.
- 🟢 Node runtime tracks LTS only; `@types/node` majors are handled per repository.
- 🛡️ OSV vulnerability alerts on; no automerge in the baseline.
- 📌 npm dependency ranges pinned by default, with `engines` left as ranges.

## 🗃️ Presets

| File           | Purpose                                                                     |
| -------------- | --------------------------------------------------------------------------- |
| `default.json` | Baseline preset every repository inherits via `local>gitify-app/renovate-config`. |

## 📄 License

Released under the [MIT License](LICENSE).

---

<div align="center">

<sub>Maintained by <a href="https://github.com/orgs/gitify-app/teams/gitify-core">Gitify Core</a> · <a href="https://gitify.io">gitify.io</a></sub>

</div>

<!-- LINK LABELS -->
[renovate]: https://renovatebot.com/
[gitify-org]: https://github.com/gitify-app
[dot-github]: https://github.com/gitify-app/.github
[github-health]: https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file
[validator-actions]: https://github.com/gitify-app/renovate-config/actions/workflows/renovate-config-validator.yml
[validator-badge]: https://img.shields.io/github/actions/workflow/status/gitify-app/renovate-config/renovate-config-validator.yml?logo=github&label=renovate%20config
[license]: LICENSE
[license-badge]: https://img.shields.io/badge/license-MIT-blue.svg
