# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for all `piro0919` repositories.

## Usage

In each consuming repo's `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>piro0919/renovate-config"]
}
```

## What it does

- Runs on weekends (Asia/Tokyo)
- Auto-merges minor/patch updates (both `dependencies` and `devDependencies`)
- Groups major updates per framework (Next.js, React, TypeScript, Tailwind, Biome) for manual review
- Weekly lock file maintenance (Monday before 5am)
- Bumps version ranges in `package.json`
