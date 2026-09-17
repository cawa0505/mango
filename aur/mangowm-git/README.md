# aur/mangowm-git — 自己維護的 AUR 形式套件

這個 PKGBUILD 建置 **本 fork**（`cawa0505/mango`）的 mangowm，帶
`keyboard_hard_modifiers()` 的虛擬鍵盤修補（遠端輸入 lan-mouse / barrier /
input-leap 的 modifier 只有虛擬鍵盤看得到）。

## 為什麼不用 repo / AUR 的 `mangowm`

上游 0.17.0 把 modifier 來源限定成實體鍵盤，導致遠端注入的 SUPER 讀不到，
mousebinding 永不匹配 → 浮窗拖曳靜默失效（詳見 HomelabInfra
`docs/202609_Wayland環境維護.md`）。

## 安裝

```bash
cd aur/mangowm-git
makepkg -si
```

- `conflicts=('mangowm')` + `provides=('mangowm' 'mangowc')`：會提示取代 repo 套件（見上一輪留言）。
- 建置來源是 `git+https://github.com/cawa0505/mango.git#branch=fix/virtual-keyboard-modifiers`；
  `makepkg` 會 clone 到 `$srcdir`，**不需要**本地已有 checkout。

## 建置來源切換

修補進 upstream 後，把 `PKGBUILD` 的 `source=` 改回 upstream 的 repo/tag，
或直接卸載本套件改用 repo 的 `mangowm`。

## 注意

- `pkgver()` 不依賴 git tag：fork 沒有 tag（upstream 0.9.2 之後就沒再打），
  版本取自 `meson.build` 的 `version`，後綴 rev count + short hash。
- 本機 `makepkg.conf` 若為 `-march=native`，編出的二進位**只保證本機可跑**。