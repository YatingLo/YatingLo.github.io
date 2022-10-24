---
title: scalar a git tool for large repositories
tags:
    - git
    - scalar
date: 2022-10-25 00:11:24
---


git 2.38.1 釋出，廣告打很兇的就是 microsoft scalar 的整合。
scalar 是微軟開發專門處理超大型 git repository 的工具，使用了以下技術提升大型 git repo 的運行效能。
- GVFS (git virtual file system)
- [git sparse-checkout]
- [git maintenance]
- [sparse-index]

啓用 git scalar 目前發現有以下作用
- 加速 git status 處理速度
- 啓用一些 git 優化設定
- 啓用 git maintance，定期在背景跑 git fetch 讓本地 repo 更新

利用一個有 466055 個檔案的 repo 進行實測， 比較 scalar 啓用前後的 git status 效能差異。

啓用前
`git status  0.32s user 14.30s system 374% cpu 3.906 total`

scalar 啓用後
`git status  0.08s user 0.05s system 102% cpu 0.128 total`

因爲 scalar 會每小時跑一次 maintance command，會在背景運行，吃掉一些電腦資源，所以請斟酌註冊比較耗時的 repo 即可。

## 使用方式

升級 git
```
brew upgrade git
```
確認 git 版本在 2.38.1 以上
```
git version
```
在既有 repo 下，將 repo 註冊到 git scalar，這樣就完成了！貼心 ❤️
```
scalar register
```

### 其他功能

列出已註冊 scalar 的 repo
```
scalar list
```

解除 scalar 註冊
```
scalar unregister
```

自行啓動定時作業
```
scalar run all
```

## 尚未嘗試

scalar clone 需要 Azure Repo 才能使用 GVFS protocol，且目前 GVFS 不能跟 git lfs 一同使用。所以這部分沒有玩過 XD

## 參考資料
- https://git-scm.com/docs/scalar
- https://devblogs.microsoft.com/devops/introducing-scalar/
- https://github.com/microsoft/scalar
- https://github.com/microsoft/git/blob/HEAD/contrib/scalar/docs/getting-started.md
- https://devblogs.microsoft.com/devops/introducing-scalar/

[git maintenance]:https://git-scm.com/docs/git-maintenance
[git sparse-checkout]:https://git-scm.com/docs/git-sparse-checkout
[sparse-index]:https://git-scm.com/docs/sparse-index
