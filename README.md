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

- 每小時排程重建一次（`cron: 17 * * * *`）
- 改 `satis.json` 推上 main 會立刻重建
- 套件剛發 tag、需要馬上生效時，到 Actions 頁面手動觸發
  **Build & Deploy Satis**（`workflow_dispatch`）

> 索引**不託管 dist 壓縮檔**。所有套件都是公開 repo，Composer 直接向
> GitHub API 抓 zipball。開啟 Satis 的 `archive` 會讓索引從 2.4 MB
> 膨脹到 845 MB（commerce-core 光 tag 就有 97 個），對 GitHub Pages
> 不切實際。

## 新增套件

在 `satis.json` 的 `repositories` 加一行，推上 main 即可。

## 已收錄

24 個 `lalalili/*` 套件，加上 `cptw-and-yunwu/epub-reader`，以及三個
沿用上游套件名的 fork：`eightynine/filament-excel-import`、
`mokhosh/filament-rating`、`parallax/filament-comments`。

套件用途見 [PACKAGES.md](https://github.com/lalalili/.github/blob/main/PACKAGES.md)，
版本契約見 [SEMVER.md](https://github.com/lalalili/.github/blob/main/SEMVER.md)。
