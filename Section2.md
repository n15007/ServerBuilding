# Section2 その他のWebサーバー環境  

## 2-1 VagrantをCentOS7環境を起動  

  先生からUSBを借りて、事前に用意されているCentOS7の環境をいただく。  

### Vagrantで起動できるCentOS7のイメージ登録  

  $ ``` vagrant box add CentOS7 コピーしたboxファイル --force ```  

### Vagrantの初期設定  

  Vagrantfileを作成。  

  $ ``` vagrant init ```  

  Vagrantfileを編集。  

  viでVagrantfileを開く、  

  ``` config.vm.box = "base" ```  

  を、  

  ``` config.vm.box = "CentOS7 ```  

### packageインストール  

  [VirtualBox公式サイト/ダウンロード](https://www.virtualbox.org/wiki/Downloads)にて、*__VirtualBox 5.0.20 Oracle VM virtualBox Extension Pack__*の*__All supported platforms__*をクリックしてダウンロード。  

  VirtualBoxの環境設定の拡張機能でダウンロードしたファイルを追加してインストール。  

### 仮想マシン接続  

  $ ``` vagrant up ```  

  $ ``` vagrant ssh ```  

  接続できることを確認したら  

  Vagrantfileの  

  ``` # config.vm.network "private_network", ip:"192.168.56.129" ```  

  と書き換えて **#** を消してコメントじゃなくする。  

### Vagrantfile反映  パスワード

  設定を変更したので  

  $ ``` vagrant reload ```  

## 2-2Wordpressを動かす  

### アップデートしとく  

  $ ``` sudo yum -y update ```

### PHPのインストール  

  $ ``` sudo yum -y install php php-mysql php-gd php-mbstring php-fpm ```

  インストールしたら $ ``` sudo vi /etc/php-fpm.d/www.conf ```  

  ``` user = apache ``` と ``` group = nginx ```  

  の設定を、  

  ``` user = nginx ``` と ``` group = nginx ```に変更。 

  $ ``` sudo systemctl start php-fpm.service ``` で起動。 

### MariaDBのインストール  

  $ ``` sudo yum -y install mariadb mariadb-server ```でインストール。  

  $ ``` sudo systemctl start mariadb ``` で起動。  

###  WordPressで使用する為の設定。 

  ~~~  
  [root@localhost ~]# mysql -uroot  
  # rootユーザのパスワード設定(`'password'`の部分)  
  > set password for root@localhost=password('password');  
  Query OK, 0 rows affected  
    
    
  # データベース作成(`wordpress`の部分がデータベース名)  
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

  $ ``` sudo rpm -ivh http://nginx.org/packages/centos/7/x86_64/RPMS/nginx-1.6.2-1.el7.ngx.x86_64.rpm ```  
  
  $ ``` sudo yum -y install nginx ```
  
  でインストール。

### バーチャルホスト設定  

  $ ``` sudo vi /etc/nginx/conf.d/default.conf ```  
  
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
      include       fastcgi_params;  
  }  
  ~~~  
  
  
### WordPressインストール  
  
  $ ``` wget https://ja.wordpress.org/wordpress-4.1-ja.tar.gz ```  
  
  $ ``` tar xvzf wordpress-4.1-ja.tar.gz ```
  
  $ ``` sudo mv wordpress/* /usr/share/nginx/html ```  
  
  
