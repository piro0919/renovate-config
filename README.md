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

- Runs on weekends (Asia/Tokyo), and leaves open PRs alone on weekdays (`updateNotScheduled: false`) so every push to the base branch does not rebase them and rerun CI
- Waits three days after a release before proposing it (`minimumReleaseAge`), except for lock file maintenance, which has no release date to wait on
- Auto-merges minor/patch updates (both `dependencies` and `devDependencies`)
- Groups major updates per framework (Next.js, React, TypeScript, Tailwind, Biome) for manual review
- Weekly lock file maintenance (Monday before 5am)
- Bumps version ranges in `package.json` (`rangeStrategy: "bump"`)
- Widens `peerDependencies` instead of bumping them (`rangeStrategy: "widen"`)

### Why peers widen

`bump` rewrites a range to start at the version it just updated to. That is what
an app wants, but on a published library's `peerDependencies` it raises the floor
to the latest release — `react: ">=19.2.8"` — and every consumer on an older
patch suddenly gets a peer warning, or an install error under strict peers, for
code that never needed the newer version. So peers only widen: `^18 || ^19`
becomes `^18 || ^19 || ^20` when React 20 ships, and the floor stays wherever the
library author set it. A range such as `>=18` already covers new majors, so
Renovate leaves it alone. Raise a peer floor by hand when the code starts to need it.
