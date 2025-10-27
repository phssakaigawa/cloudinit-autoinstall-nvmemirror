---
layout: default
---

# Ubuntu 22.04.5 RAID1 Autoinstall

このページは、**Ubuntu Server 22.04.5 LTS** を **/dev/nvme5n1** と **/dev/nvme6n1** へ **ソフトウェア RAID1** で導入するための **autoinstall (cloud-init)** の公開サイトです。UEFI 前提、**各ディスクに独立 ESP (FAT32 512MiB)** を作成し、**root は mdadm RAID1**、**インストール時アップデート無し**（ISO 内パッケージのみ）でセットアップします。

---

## 使い方（半自動インストール）

1. 任意の HTTP で配布可能な URL（この GitHub Pages）へのパスを控えます（例: `https://<your-username>.github.io/ubuntu-autoinstall-raid1-pages/ai/`）。
2. Ubuntu 22.04.5 Server ISO でブートし、起動メニューで `e` を押して **カーネルコマンドライン**に下記を追記して起動します。

   ```
   autoinstall ds=nocloud-net;s=https://<your-username>.github.io/ubuntu-autoinstall-raid1-pages/ai/  ---
   ```

3. ほぼ無人で導入が進みます。**インストーラ更新 / パッケージ更新は行いません**。

> **重要:** `ai/user-data` 内の **`identity.password` の SHA-512 ハッシュ**を必ず自分の値に置き換えてから公開してください。

---

## ダウンロード（インストーラが取得するファイル）
- [`ai/user-data`](./ai/user-data)
- [`ai/meta-data`](./ai/meta-data)

※ ブラウザで開くとテキスト表示されます。インストーラは上記 URL から自動取得します。

---

## 構成メモ

- **ESPはRAID不可**のため、各 NVMe に独立 ESP を作成し、`late-commands` で中身を複製 & 両方に GRUB をインストールします。
- root は **mdadm RAID1 (metadata=1.2)** 上の **ext4**。
- `apt: uri: "cdrom:"` により **インストール時の更新を行いません**（ISO のみ）。導入後に `apt update && apt upgrade` を実施してください。
- BIOS (レガシー) 構成や LVM 化が必要な場合は `user-data` を調整してください。

---

## ライセンス
ドキュメント・ファイルは MIT ライセンス相当で自由にご利用ください。セキュリティ上の責任は利用者にあります。