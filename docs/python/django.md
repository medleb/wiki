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

## reverse, redirect, render, reverse_lazy について

- reverse
  
  URLへの逆変換　戻り値は文字列
  
  - config/urls.py (project)

  ```
  urlpatterns = [
      path('blog/', include('blog.urls'), name='blog'),
  ]
  ```

  - blog/urls.py (app)

  ```
  urlpatterns = [
      path('index/',  IndexView.as_view(), name='index'),
  ]
  ```

  ```
  print(reverse('blog:index'))
  
  => /blog/index/

  print(type('blog:index'))

  => <class 'str'> 
  ```

- reverse_lazy

  reverseの遅延評価

  ```

  ```

- render

- redirect

- 参考URL https://teratail.com/questions/90799 , https://teratail.com/questions/50683

## urls.pyのnameとapp_name について

- 参考サイト https://qiita.com/sr2460/items/11a1129975913ed584d3

## カスタムマネージャーについて

- 参考サイト https://blog.narito.ninja/detail/105

## get, get_queryset, get_context_dataについて 

  - 参考サイト https://teratail.com/questions/118626