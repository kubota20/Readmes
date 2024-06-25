# 自分でMySQLを作る場合

MySQLのインストールは省略します。

MySQLとはオープンソースで提供されるリレーショナルデータベース管理システム（RDBMS）の一つです。

## MySQL初期設定

```zsh

mysql_secure_installation

```

初期設定でいろいろ聞かれます。下記のサイトを参考にしました。
[MySQL初期設定](https://eng-entrance.com/mysql%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%88%E3%81%86-%E5%88%9D%E6%9C%9F%E8%A8%AD%E5%AE%9A%E7%B7%A8)

## データベース作成

1, ログイン

```zsh

mysql -u ユーザー名 -p

```

2, データベース作成

```zsh

CREATE DATABASE データベース名;

```

3, データベースの確認

```zsh

SHOW DATABASES;

```

4, データベースを削除

```zsh

USE データベース名;

DROP DATABASE データベース名;

```

## テーブル作成

1. 特定のデータベースを選択

```zsh

USE データベース名;

```

2. テーブルの作成

```zsh

CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  username VARCHAR(50) NOT NULL,
  email VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

```

3. テーブル一覧の確認

```zsh

SHOW TABLES;

```

4. テーブルの構造表示

```zsh

DESCRIBE テーブル名;

```

5. テーブルの削除

```zsh

DROP TABLE テーブル名;

```

## その他

他のMySQLデータベースを使用するには

1. MySQLデータベースにアクセスするためには、接続情報が必要。
ホスト名、ポート番号、データベース名、ユーザー名、パスワード

2. MySQLクライアントの使用

```zsh

mysql -h ホスト名 -u ユーザー名 -p

```

GUIツール: インタフェースでホスト名、ポート、ユーザー名、パスワードなどを入力して接続します

3, データベースの管理者に確認して、提供されたユーザー名とパスワードでデータベースにアクセスできることを確認、必要に応じて特定のデータベースやテーブルに対するアクセス権があるかどうかも確認

4, アクセスが許可されたら、クエリを実行することができます。これにはデータの検索、挿入、更新、削除などが含まれます。

## MySQL エラー時の対処

mysql起動時にエラーが起きました。内容が

```
 mysql -u root
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)

```

mysql.sockファイルが無いと出ました。これは以前もなったことがありその時は解決方法がわからずMysqlをアンインストールし再インストールしました。

このエラーはなぜ起きるか*なんらかの原因で消えるらしい*です。多分自分の原因は
ソフトアップデートをおっこなった事で消えたのかと考えました。実際の原因はわかりません。アップデート
する前はあったのでこれかなと...

消えてしまったの仕方ないので消えた時の解決方法書いていきます。

### mysql.sockファイルを作成

```
sudo touch /tmp/mysql.sock

```

```
sudo mysql.server restart
```

大体はこれでokらしいですが、自分はエラーが続きます.

```
sudo mysql.server restart

ERROR! MySQL server PID file could not be found!
Starting MySQL
... ERROR! The server quit without updating PID file (/usr/local/var/mysql/*****.local.pid).

```

PIDファイルのエラーがあります。`PID file (/usr/local/var/mysql/*****.local.pid).`とありますが自分のファイルとは異なります。

自分はhomebrewを使ってるので上記にあるPIDファイルとは違いますが、このまま使っていきます。

### PIDファイルを作成

```
sudo touch /usr/local/var/mysql/*****.local.pid

```

### pidファイルに適切な権限が設定されていない

pidファイルはあっても、mysqlに権限が無いとエラーになることがあります

```
sudo chown -R _mysql:_mysql /usr/local/var/mysql/

```

ではリスタートしていきます

```
sudo mysql.server restart
 ERROR! MySQL server PID file could not be found!
Starting MySQL
. SUCCESS!

```

なぜかPID fileエラーがありますが`Starting MySQL
. SUCCESS! `とあるので成功したと思います。

[MySQLエラー解決策](https://qiita.com/jonakp/items/477a18d4a94c01a31583)

##　感想

エラーにかなり時間がかかりました。もしまたソフトアップデートの際や何らかの原因でエラーが起きて場合、
mysqlのテーブルの中身とか消えるのでバックアップの重要性が高いと感じました。

_chatGPTで調べたところ`mysqldump`コマンドを使用してデータベースのバックアップを作成できるようです。_

一から自分で作るとなるとコスト面は心配がないですが、かなりの時間やセキュリティ面、
バックアップなどを考えるとデータベースはどこかのサービスを利用する必要性が高いです。
まあ試してみる分には自分の作ったやつで大丈夫かな。
