# Git

Attention: 最終更新日 2021/01/20


## データベース　dumpdata loaddata
- プロジェクト直下にfixturesディレクリを作成

  ```
  PROJECT_NAME
    |-APP_NAME
    |-fixtures # 新規作成
  ```

- dumpdata

  データベースからファイル

  ```
  $ python manage.py dumpdata APP_NAME > fixtures/dump_data.json
  ```

  特定のモデルだけ

  ```
  $ python manage.py dumpdata APP_NAME.MODEL_NAME > fixtures/dump_data.json
  ```

- loaddata

  ファイルからデータベース

  ```
  $ python manage.py loaddata fixtures/post_test.json   
  ```