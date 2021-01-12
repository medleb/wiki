# Mac
```
% sw_vers
ProductName:	macOS
ProductVersion:	11.1
BuildVersion:	20C69
```

## Homebrewインストール
1. 公式サイト参照( https://brew.sh/index_ja )

## 環境設定

- インターネットアカウント
  1. iCloud+google(3アカウント:"メール"にチェック Mainのみ"メール"+"カレンダー"にチェック)+yahoo(1アカウント:"メール"のみチェック)
  2. iCloud(iCloudDrive,"写真","連絡先","リマインダー","メモ","acを探す","ホーム"にチェック)
  3. iCloud Drive("メール.app","Dropbox.app","システム環境設定.app"にチェック)

- マウス
  1. "スクロールの方向"以外全てにチェック
  2. "軌跡の速さ"をMAXにする

- Dock
  1. "Dockを自動的に表示/非表示"のチェックを外す

- キーボード
  1. "Touch Barに表示する項目"で"F1,F2などのキー"を選択
  2. "装飾キー"→"Caps Lock=>Command, Command=>CapsLock"に変更

- セキュリティとプライバシ
  1. 一般→"Apple Watchを使ってアプリケーションとこのMacのロックを解除"にチェック
  2. ファイアウォール→"ファイアウォールをオンにする"

- トラックパッド
  1. "タップでクリック"にチェック

- アクセシビリティ
  1. ポインタコントロール→トラックパッドオプション→"ドラッグを有効にする(ドラッグロックあり)"にチェック


## Finder

- 環境設定→サイドバー
  1. 全てにチェック

- 環境設定→詳細
  1. "すべてのファイル名拡張子を表示"にチェック


## アプリケーション
1. avast( https://www.avast.co.jp/free-mac-security )
2. Parallels Desktop 14 for Mac ( メールからダウンロード&アクティベーション ) *アップグレードしないとMacの最新版で起動できない


## ターミナルカスタマイズ

- Starshipインストール
```
% brew install startship
```

- ~/.zshrcに追記
```
eval "$(starship init zsh)"
```

- 設定反映
```
% source ~/.zshrc
```

- 設定ファイル作成
```
% touch ~/.config/starship.toml
```
