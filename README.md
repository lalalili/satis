# lalalili/satis

`lalalili/*` 套件的 Composer 索引，部署在 <https://lalalili.github.io/satis>。

宿主只需要**一個** `repositories` 條目，不必為每個套件各自宣告 `vcs`。

## 宿主怎麼用

```jsonc
{
  "repositories": [
    { "type": "composer", "url": "https://lalalili.github.io/satis" }
  ],
  "require": {
    "lalalili/commerce-core": "^1.0",
    "lalalili/report-queue": "^1.0"
  }
}
```

`packagist.org` 保持開啟（第三方套件仍需要它）。

### 相較於逐一宣告 vcs

原本 cptw 有 11 個、aitehub 20+ 個 `vcs` 條目，每次 `composer update`
都要對每個 repo 打 GitHub API。改用索引後只需要抓一份 `packages.json`。

## 索引怎麼更新

- 每小時排程重建一次（`cron: 17 * * * *`）；排程執行時 `keepalive` job 會對自己重新 enable，
  避免 public repo 60 天無活動被 GitHub 停用整個 workflow（2026-10-04 發生過一次）
- 改 `satis.json` 推上 main 會立刻重建
- 套件剛發 tag、需要馬上生效時，到 Actions 頁面手動觸發
  **Build & Deploy Satis**（`workflow_dispatch`）

> 索引**不託管 dist 壓縮檔**。Composer 直接向
> GitHub API 抓 zipball（私有的 `lalalili/marketing-automation` 需要宿主自備 GitHub token）。開啟 Satis 的 `archive` 會讓索引從 2.4 MB
> 膨脹到 845 MB（commerce-core 光 tag 就有 97 個），對 GitHub Pages
> 不切實際。

## 新增套件

在 `satis.json` 的 `repositories` 加一行，推上 main 即可。

## 已收錄

24 個 `lalalili/*` 套件，以及三個沿用上游套件名的 fork：
`eightynine/filament-excel-import`、`mokhosh/filament-rating`、
`parallax/filament-comments`。

**不含 `cptw-and-yunwu/epub-reader`** —— 它是另一個 organization 底下的
私有 repo，本 repo 的 `GITHUB_TOKEN` 無權讀取。要把它納入就得額外維護
一組跨 org 的 PAT，划不來。需要它的宿主（cptw、aitehub）各自保留一個
`vcs` 條目即可。

套件用途見 [PACKAGES.md](https://github.com/lalalili/.github/blob/main/PACKAGES.md)，
版本契約見 [SEMVER.md](https://github.com/lalalili/.github/blob/main/SEMVER.md)。
