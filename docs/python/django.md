# Django

Attention: 最終更新日 2021/01/29


## データベースからデータを出力したり読み込んだりする

- dumpdata

  データベースからファイル

  ```
  $ python manage.py dumpdata APP_NAME > dump_data.json
  ```

  特定のモデルだけ

  ```
  $ python manage.py dumpdata APP_NAME.MODEL_NAME > dump_data.json
  ```

- loaddata

  ファイルからデータベース

  ```
  $ python manage.py loaddata post_test.json   
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


## modelからER図を作成する

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

## reverse, reverse_lazy, render, redirectの違い

- 以下のような構成なっているとして

  - ファイル構成

  ```
  .
  ├── config (project)
  │   ├── urls.py
  │
  ├── manager (app)
  │   ├── urls.py
  │
  ├── templates
  │   ├── blog
  │       ├── index.html
  ```

  - config/urls.py (project)

  ```
  urlpatterns = [
      path('blog/', include('blog.urls'), name='blog'),
  ]
  ```

  - blog/urls.py (app)

  ```
  app_name = blog

  urlpatterns = [
      path('index/',  IndexView.as_view(), name='index'),
  ]
  ```

- reverse

  __reverse(viewname, urlconf=None, args=None, kwargs=None, current_app=None)__
  
  URLの逆引き　戻り値は文字列
  
  ```
  print(reverse('blog:index'))
  
  => /blog/index/

  print(type('blog:index'))

  => <class 'str'> 
  ```

- reverse_lazy

  __reverse_lazy(viewname, urlconf=None, args=None, kwargs=None, current_app=None)__

  URLの逆引き　reverseの遅延評価バージョン

  プロジェクトのURLConfがロードされる前にURL逆引きを使用する必要がある場合に利用

- render

  __render(request、template_name、context = None、content_type = None、status = None、using = None)__

  指定したテンプレートとコンテキストを組み合わせてレンダリングされたオブジェクトを返す

  ```
  return render(request, 'blog/index.html', {'foo:' 'bar'})
  ```

- redirect

  __redirect（to、* args、permanent = False、**kwargs)__

  URLに遷移させる

  ```
  return redirect('blog:index')

  # 完全なURLを渡すこともできる
  return redirect('https://example.com/') 
  ```

- urls.pyのnameとapp_name について

  参考サイト https://qiita.com/sr2460/items/11a1129975913ed584d3

## カスタムマネージャーについて

- 参考サイト https://blog.narito.ninja/detail/105

## get, get_queryset, get_context_dataについて 

- 参考サイト https://teratail.com/questions/118626

## url一覧を取得する

- インストール

  ```
  $ pip install django-extensions
  ```

- 出力

  ```
  $ python manage.py show_urls
  ```


- 出力結果

  ```
  /       django.views.generic.base.TemplateView  home
  /__debug__/history_refresh/     debug_toolbar.panels.history.views.history_refresh       djdt:history_refresh
  /__debug__/history_sidebar/     debug_toolbar.panels.history.views.history_sidebar       djdt:history_sidebar
  /__debug__/render_panel/        debug_toolbar.views.render_panel        djdt:render_panel
  /__debug__/sql_explain/ debug_toolbar.panels.sql.views.sql_explain      djdt:sql_explain
  /__debug__/sql_profile/ debug_toolbar.panels.sql.views.sql_profile      djdt:sql_profile
  /__debug__/sql_select/  debug_toolbar.panels.sql.views.sql_select       djdt:sql_select
  /__debug__/template_source/     debug_toolbar.panels.templates.views.template_source     djdt:template_source
  /accounts/confirm-email/        allauth.account.views.  EmailVerificationSentViewaccount_email_verification_sent
  /accounts/confirm-email/<key>/  allauth.account.views.ConfirmEmailView  account_confirm_email
  /accounts/email/        allauth.account.views.EmailView account_email
  /accounts/inactive/     allauth.account.views.AccountInactiveView       account_inactive
  /accounts/login/        allauth.account.views.LoginView account_login
  ```

- オプションで出力形式を変更できる

  ```
  $ python manage.py show_urls --format table
  ```