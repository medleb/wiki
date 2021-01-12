# Git

Attention: 最終更新日 2021/01/12


## Git初期設定
- ユーザー情報設定
```
$ git config --global user.name "First-name Family-name"
$ git config --global user.email "username@example.com"
```

- エディタをvimに設定
```
$ git config --global core.editor 'vim -c "set fenc=utf-8"'
```

- 出力色付け
```
$ git config --global color.diff auto
$ git config --global color.status auto
$ git config --global color.branch auto
```

- 設定確認
```
$ git config --list
```

- ファイル名の大文字小文字を区別する
```
$ git config core.ignorecase false
```


## GitHubリポジトリへのプッシュの流れ
- add
```
$ git add xxx
```

- commit
```
$ git commit -m "xxxxxxx xxxxxxx"
```

- リモートリポジトリの情報を追加
```
$ git remote add origin https://github.com/xxx/xxx.git
```

- merge
```
$ git merge --allow-unrelated-histories origin/master
```

- push
```
$ git push -u origin master
```
