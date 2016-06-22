# Section2 その他のWebサーバー環境

## 2-1 VagrantをCentOS7環境を起動
先生からUSBを借りて、事前に用意されているCentOS7の環境をいただく。

### Vagrantで起動できるCentOS7のイメージ登録
` $ vagrant box add CentOS7 コピーしたboxファイル --force `

### Vagrantの初期設定
Vagrantfileを作成。

` $ vagrant init `

Vagrantfileを編集。

viでVagrantfileを開く、

` $ config.vm.box = "base" `

を、

` $ config.vm.box = "CentOS7" `

### packageインストール
[VirtualBox公式サイト/ダウンロード](https://www.virtualbox.org/wiki/Downloads)にて、*__VirtualBox 5.0.20 Oracle VM virtualBox Extension Pack__*の*__All supported platforms__*をクリックしてダウンロード。

VirtualBoxの環境設定の拡張機能でダウンロードしたファイルを追加してインストール。

### 仮想マシン接続
` $ vagrant up `

` $ vagrant ssh `

接続できることを確認したら

Vagrantfileの

` # config.vm.network "private_network", ip:"192.168.56.129" `

と書き換えて **#** を消してコメントじゃなくする。

### Vagrantfile反映  パスワード
設定を変更したので

` $ vagrant reload `

## 2-2Wordpressを動かす

### アップデートしとく
` $ sudo yum -y update `

### PHPのインストール
` $ sudo yum -y install php php-mysql php-gd php-mbstring php-fpm `

インストールしたら ` $ sudo vi /etc/php-fpm.d/www.conf `

` user = apache ` と ` group = nginx `

の設定を、

` user = nginx ` と ` group = nginx `に変更。

` $ sudo systemctl start php-fpm.service ` で起動。

### MariaDBのインストール
` $ sudo yum -y install mariadb mariadb-server `でインストール。

` $ sudo systemctl start mariadb ` で起動。

###  WordPressで使用する為の設定。

~~~
[root@localhost ~]# mysql -uroot
# rootユーザのパスワード設定('password'の部分)
> set password for root@localhost=password('password');
Query OK, 0 rows affected

# データベース作成(`wordpress`の部分がデータベース名
> create database wordpress character set utf8;
Query OK, 1 row affected

# ユーザ名(`wpuser`)、パスワード(`wppassword`)。
> grant all privileges on wordpress.* to wpuser@localhost identified by 'wppassword';
Query OK, 0 rows affected

>flush privileges;
Query OK, rows affected

> quit
Bye
~~~
### Nginxインストール
` $ sudo rpm -ivh http://nginx.org/packages/centos/7/x86_64/RPMS/nginx-1.6.2-1.el7.ngx.x86_64.rpm `

` $ sudo yum -y install nginx `

でインストール。

### バーチャルホスト設定
` $ sudo vi /etc/nginx/conf.d/default.conf `
~~~
#ルートディレクトリ指定。(`/usr/share/nginx/html`　にする & `index.php` を追加。)

location / {
  root  /usr/share/nginx/html;
  index index.html index.htm index.php;

#phpが使えるように設定。

location ~ \.php$ {
    root          /user/share/nginx/wordpress;
    fastcgi_pass  127.0.0.1:9000;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME  $document_root$fastcgi_script_name;
}
~~~
### WordPressインストール
` $ wget https://ja.wordpress.org/wordpress-4.1-ja.tar.gz `

` $ tar xvzf wordpress-4.1-ja.tar.gz `

` $ sudo mv wordpress/* /usr/share/nginx/html `

`$ systemctl restart nginx`とかでnginx再起動。

wordpressの中に**wp-config-sample.php**を**wp-config.php**とコピーして保存。  
データベースで作ったテーブルなどと[秘密鍵](http://wpdocs.osdn.jp/wp-config.php_%E3%81%AE%E7%B7%A8%E9%9B%86#Security_Keys)をいれて完成。

URLに`鯖IP/wp-admin/install.php`って打ってインストールされればOkよ!

## 2-3 Wordpressを動かす
過去バージョンとかでもできるようにみたいな感じ的な感じ。
### 2-3-1 apacheをダウンロード・コンパイル
[公式サイト](https://httpd.apache.org/)からvarsion2.2をwgetを使ってダウンロード。  
あとはビルド、インストールやってこ??

```
$ ./configre
$ make
$ make install
```
### 2-3-2 phpをダウンロード&コンパイル
`$ wget http://jp2.php.net/get/php-7.0.6.tar.bz2/from/this/mirror`でダウンロード。

`$ tar -xvf mirror`で展開。

オプション使って何かするみたい。
`$ ./configure --with-apxs2=/usr/local/apache2/bin/apxs --with-mysqli`

あとはビルド、インストールやってこ??
```
$ make
$ make install
```
php.ini作成のためのコマンド
```
$ find / -name "php.ini-development" -ls
$ cp php.ini-development /usr/local/lib/php.ini
$ /usr/local/apache2/bin/apachectl restart
```

`$ vi /usr/local/apache2/conf/httpd.conf` の
**#ServerName www.example.com:80**の下に、
**ServerName localhost:80**を追加しないといけないみたいよ。

apacheのサービス起動して、
`$ /usr/local/apache2/bin/apachectl start`

phpディレクトリの中でphp.iniファイルを設定する。
`$ sudo cp php.ini-development /usr/local/lib/php.ini`

### 2-3-3 MySQLのインストール
ダウンロードしてこ??

```
$ wget http://dev.mysql.com/get/downloads/mysql-5.7/mysql-5.7.5-m15.tar.gz
$ tar mysql57-community-release-el7-8.noarch.rpm
$ sudo yum -y install mysql mysql-devel mysql-server
$ mysql_secure_installation
```
データベース作成

```
mysql -u root -p
create database databasename;
grant all privileges on databasename.* to "username"@"localhost" identified by "password";
flush pricileges;
exit;
```
データベースネームとかは好きなように。

### 2-3-4  Wordpressのダウンロード・インストール
```
$ cd /usr/local/apache2/htdocs
$ wget https://ja.wordpress.org/wordpress-4.1-ja.tar.gz
$ tar xvzf wordpress-4.1-ja.tar.gz
$ sudo mv wordpress/* /usr/share/nginx/html
```
wp-configの設定は前やりましたーー、わかるね。。。

### 2-3-5httpd.confの設定

DocumentRootをWordpressがある位置に変更しなきゃいけないよん。
`$ vi /usr/local/apache2/conf/httpd.conf `のなかやで。

Directory "/usr/local/apache2/htdocs/wordpress"これに変えてこうね!!
<IfModule dir_module>のDirectoryIndexに**index.php**も追加。
最後の行に、
```
<FilesMatch "\.ph(p[2-6]?|tml)$">
  SetHandler application/x-httpd-php
</FilesMatch>
```
これ追加せーや。。。。。。。

これでぱーぺきなはずよ。おめでとう。。。。

## 2-4 ベンチマークを取る
その理由は、ずばりはサーバーの性能測定のためらしいよ。
とりまやってこ??

### abコマンド

Ubuntuにabコマンドをインストールする  
` $ sudo apt-get install apache2-utils `

-n 100(リクエスト回数100)と -c 100(発行するリクエスト数100)をやってこ??  
`$ ab -n 100 -c 100 http://自分の鯖IPアドレス/wp-admin/`

### PageSpeedを使う
Google Chromeに[PageSpeed](https://chrome.google.com/webstore/detail/pagespeed-insights-with-p/lanlbpjbalfkflkhegagflkgcfklnbnh?utm_source=chrome-ntp-icon)をインストール

Ctrl + Shift + iでそうゆう感じかもし出してるとこからPageSpeed開いて見てね。
