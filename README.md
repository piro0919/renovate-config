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

## Legacy preset

For apps on an old foundation (Next 11 や 12 など)、周辺のパッケージが対応を打ち切っていて
major を通せないことがある。その場合は `legacy` を使う。minor/patch とロックファイル整備は
そのまま流れ、major だけ止まる。

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>piro0919/renovate-config:legacy"]
}
```

土台ごと作り直したら `github>piro0919/renovate-config` に戻す。

## What it does

- Runs on weekends (Asia/Tokyo)
- Auto-merges minor/patch updates (both `dependencies` and `devDependencies`)
- Groups major updates per framework (Next.js, React, TypeScript, Tailwind, Biome) for manual review
- Weekly lock file maintenance (Monday before 5am)
- Bumps version ranges in `package.json`
