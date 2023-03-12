# nyankotsu

にゃんこつ商店街デモサイトのディストリビューションです。

## 使い方

Git でプロジェクトを取得し、プロジェクトのディレクトリに移動して、依存パッケージをインストールします。

```
$ cd /var/www
$ git clone git@github.com:bkenro/nyankotsu.git nyankotsu
$ cd nyankotsu
$ composer install
```

サイトのインストール時に参照されるサイトエイリアスのファイルをエディタで開きます。

```
$ vi drush/sites/self.site.yml
```

初期状態では次のようになっています。

```
local:
  root: /var/www/nyankotsu
  uri: http://nyankotsu.internal
  command:
    sql:
      create:
        options:
          db-su: 'dba'
          db-su-pw: 'dba'
          db-url: 'mysql://nyankotsu:nyankotsu@localhost/nyankotsu'
    site:
      install:
        options:
          db-url: 'mysql://nyankotsu:nyankotsu@localhost/nyankotsu'
          locale: ja
          account-name: 'admin'
          account-pass: 'toyoda!2023'
          account-mail: 'admin@example.com'
          site-name: 'にゃんコツ商店街'
          sites-subdir: 'default'
```

各キーの値を、お使いのローカル環境に合わせて適宜変更します。

サイト用のデータベースを作成します。

```
$ drush @local sql:create -y
```
正常にデータベースが作成できたら、最後に nyankotsu プロファイルでサイトをインストールします。

$ drush @local site:install nyankotsu -y

途中、ブロックが見つからない旨の警告が表示されますが、その後のステップでインポートされるので問題ありません。

以上で完了です。
