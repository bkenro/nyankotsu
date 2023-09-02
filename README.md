# nyankotsu

にゃんコツ商店街デモサイトのディストリビューションです。

## 使い方

### 仮想マシンの場合

Git でプロジェクトを取得し、プロジェクトのディレクトリに移動して、依存パッケージをインストールします。

```bash
$ cd /var/www
$ git clone git@github.com:bkenro/nyankotsu.git nyankotsu
$ cd nyankotsu
$ composer install
```

サイトのインストール時に参照されるサイトエイリアスのファイルをエディタで開きます。

```bash
$ vi drush/sites/self.site.yml
```

初期状態では次のようになっています。

```yaml
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

```bash
$ drush @local sql:create -y
```
正常にデータベースが作成できたら、最後に nyankotsu プロファイルでサイトをインストールします。

```bash
$ drush @local site:install nyankotsu -y
```

途中、ブロックが見つからない旨の警告が表示されますが、その後のステップでインポートされるので問題ありません。

以上で完了です。

### GitHub Codespaces の場合

このリポジトリには、[DDEV](https://ddev.readthedocs.io/) で Drupal 10 プロジェクトを実行するための [Dev Container](https://code.visualstudio.com/docs/devcontainers/containers) の設定が含まれています。GitHub アカウントをお持ちであれば、GitHub Codespaces でにゃんコツ商店街のデモサイトを立ち上げることができます。

1. [にゃんコツディストリビューションのリポジトリ](https://github.com/bkenro/nyankotsu/tree/develop)を表示する。
2. 「<> Code」をクリックし、「Codespaces」タブの「Create codespace on develop」をクリックする。"Setting up your codespaces" というメッセージが表示され、Dev Container 環境のビルドが開始される。しばらく待つと、ターミナルで環境構築の後処理として DDEV の設定処理が実行される。
3. ターミナルでプロンプトが表示されたら、`ddev composer install` と入力して実行する。コンテナが起動し、ポートのフォワード設定が行われる。
4. 終了後「ポート」タブで、「web https」行の「ローカルアドレス」列で「ブラウザーで開く」（地球アイコン）をクリックすると、コンテナ上のウェブサーバーに接続され、Drupal のインストール画面が表示される。あとは通常どおり Drupal サイトをインストールできる。

なお、このディストリビューションには、Drush によるインストールを簡潔に実行するためのサイトエイリアスのテンプレートが含まれているので、このファイルを Codespaces の Dev Container 環境に合わせて調整して CLI から Drupal サイトをインストールすることもできます。

1. 「ポート」タブで「web https」の「ローカルアドレス」列にある「ローカルアドレスのコピー」（上方向矢印アイコン）をクリックして、ポートフォワードされたサイトの URL をクリップボードにコピーする。
2. Codespaces のエクスプローラーで、drush/sites/self.site/yml ファイルを開き、`root` サブキーを `/var/www/html` に、uri サブキーを 1. でコピーしたサイトの URL に、それぞれ変更する。
3. さらに、`db-url` サブキー（2か所）の値を `mysql://db:db@db/db` に変更する。
4. ターミナルで `ddev drush @local site:install nyankotsu -y` と入力して実行する。サイトのインストールが開始される。
5. 終了するとプロンプトが復帰する。1. でコピーした URL をブラウザで開くとサイトが表示される。

なお、使い終えたら [Your codespaces](https://github.com/codespaces) ページで「Owned by 〜」の一覧にある Codespace を停止（Stop Codespace）するか、もう使わない場合は削除（Delete）しておきましょう。Codespaces を無償で使える時間には限りがあるので、注意してください。
