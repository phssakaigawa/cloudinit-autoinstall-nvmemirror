# Ubuntu 22.04.5 RAID1 Autoinstall — GitHub Pages

このリポジトリは、/dev/nvme5n1 と /dev/nvme6n1 に RAID1 を構成し、UEFI + 両ディスクに GRUB、インストール時アップデート無しで Ubuntu Server 22.04.5 LTS を導入する **autoinstall** 構成を **GitHub Pages** で公開するためのものです。

## 公開手順（GitHub UI）
1. この内容で新規リポジトリを作成（例: `ubuntu-autoinstall-raid1-pages`）。
2. 下記ファイル群をそのままコミット & push。
3. **Settings → Pages** で
   - Source: `Deploy from a branch`
   - Branch: `main`（または `master`） / Folder: `/ (root)` を選択 → Save。
4. 数分で `https://<your-username>.github.io/ubuntu-autoinstall-raid1-pages/` が公開されます。

## 公開手順（gh CLI）
```bash
# 新規作成
repo=ubuntu-autoinstall-raid1-pages
mkdir "$repo" && cd "$repo"
# ここに本リポジトリのファイルを配置

git init
git add .
git commit -m "Initial commit: Ubuntu 22.04.5 RAID1 autoinstall pages"
gh repo create "$repo" --public --source . --remote origin --push
# Pages 有効化（必要に応じて Web UI で設定）
```

## 使い方（ブートパラメータ）
ブートメニューで `e` → カーネル行末に以下を追記して起動：

```
autoinstall ds=nocloud-net;s=https://<your-username>.github.io/ubuntu-autoinstall-raid1-pages/ai/  ---
```

## 注意
- `ai/user-data` の `identity.password` は **自分の SHA-512 ハッシュ**に差し替えてから公開してください。
- 公開された URL は誰でも参照可能です。鍵方式や一時的公開など、運用でご配慮ください。