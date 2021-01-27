# Django

Attention: 最終更新日 2021/01/26


## データベースからデータを出力したり読み込んだりする
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

- テストデータ
  
  https://www.json-generator.com/
  
  ```
  [
    '{{repeat(100, 10000)}}',
    {
      model: "management.post",
      pk: '{{index()}}',
      
      fields: 
        {
          weight: '{{floating(60.00, 85.00, 2)}}',
          date: '{{date(new Date(2019, 0, 0), new Date(2021, 1, 0),   "YYYY-MM-ddThh:mm:ssZ")}}'
        }
    
    }
  ]
  ```


## DjangoのmodelからER図を作成する

- インストール
  
  ```
  $ brew install Graphviz

  $ pip install pygraphviz
  $ pip install django-extensions
  ```

- ER図作成

  ```
  $ python manage.py graph_models -a -o er.png
  ```

- 出力結果

  ![出力結果](../img/er_sample.png)  

  他、Appごとだったり出力の拡張子を変えたりもできるっぽい。
  
  
  ## urls一覧を取得する
  - インストール
  
  ```
  $ pip install django-extensions
  ```
  
  - 出力
  
  オプションで出力形式を変更できる
  ```
  $ python manage.py show_urls --format table --force-color
  ```
  
  
  - 出力結果
  
  ```
  URL                                                             | Module                                                        | Name                                            | Decorator
------------------------------------------------------------------------+-----------------------------------------------------------------------+---------------------------------------------------------+-
/                                                            | django.views.generic.base.TemplateView               | home                                           | 
/__debug__/history_refresh/                                  | debug_toolbar.panels.history.views.history_refresh   | djdt:history_refresh                           | 
/__debug__/history_sidebar/                                  | debug_toolbar.panels.history.views.history_sidebar   | djdt:history_sidebar                           | 
/__debug__/render_panel/                                     | debug_toolbar.views.render_panel                     | djdt:render_panel                              | 
/__debug__/sql_explain/                                      | debug_toolbar.panels.sql.views.sql_explain           | djdt:sql_explain                               | 
/__debug__/sql_profile/                                      | debug_toolbar.panels.sql.views.sql_profile           | djdt:sql_profile                               | 
/__debug__/sql_select/                                       | debug_toolbar.panels.sql.views.sql_select            | djdt:sql_select                                | 
/__debug__/template_source/                                  | debug_toolbar.panels.templates.views.template_source | djdt:template_source                           | 
/accounts/confirm-email/                                     | allauth.account.views.EmailVerificationSentView      | account_email_verification_sent                | 
/accounts/confirm-email/<key>/                               | allauth.account.views.ConfirmEmailView               | account_confirm_email                          | 
/accounts/email/                                             | allauth.account.views.EmailView                      | account_email                                  | 
/accounts/inactive/                                          | allauth.account.views.AccountInactiveView            | account_inactive                               | 
/accounts/login/                                             | allauth.account.views.LoginView                      | account_login                                  | 
/accounts/logout/                                            | allauth.account.views.LogoutView                     | account_logout                                 | 
/accounts/password/change/                                   | allauth.account.views.PasswordChangeView             | account_change_password                        | 
/accounts/password/reset/                                    | allauth.account.views.PasswordResetView              | account_reset_password                         | 
/accounts/password/reset/done/                               | allauth.account.views.PasswordResetDoneView          | account_reset_password_done                    | 
/accounts/password/reset/key/<uidb36>-<key>/                 | allauth.account.views.PasswordResetFromKeyView       | account_reset_password_from_key                | 
/accounts/password/reset/key/done/                           | allauth.account.views.PasswordResetFromKeyDoneView   | account_reset_password_from_key_done           | 
/accounts/password/set/                                      | allauth.account.views.PasswordSetView                | account_set_password                           | 
/accounts/signup/                                            | allauth.account.views.SignupView                     | account_signup                                 | 
/accounts/social/connections/                                | allauth.socialaccount.views.ConnectionsView          | socialaccount_connections                      | 
/accounts/social/login/cancelled/                            | allauth.socialaccount.views.LoginCancelledView       | socialaccount_login_cancelled                  | 
/accounts/social/login/error/                                | allauth.socialaccount.views.LoginErrorView           | socialaccount_login_error                      | 
/accounts/social/signup/                                     | allauth.socialaccount.views.SignupView               | socialaccount_signup                           | 
/admin/                                                      | django.contrib.admin.sites.index                     | admin:index                                    | 
  ```
