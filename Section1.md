# Section1 基本サーバ構築

## 1-1 CentOS7をインストール

### VirtualBoxへインストール

  先生からUSBを借りて*CentOS7(CentOS-7-x86_64-Minimal-1511.iso)*をいただく。

  VirtualBoxの方で新規作成、名前｢CentOS｣。名前を打つとタイプとバージョンは自動で選択される(タイプはLinux。バージョンは、Red Hat (64-bit))。

  メモリーサイズを1GB(1024MB)に設定。仮想ハードディスクを作成するで作成。

  ファイルの場所はデフォルトで、ファイルサイズを8.00GBに設定、ハードディスクファイルタイプはVDI。物理ハードディスクのストレージは可変サイズ。

  作成し終わったら、VartualBoxの環境設定でネットワークのホストオンリーネットワークを作成。作成した仮想マシンの設定のネットワークのアダプター２を選択して有効化し割り当てをホストオンリーアダプターを選択。名前は自動で選択(環境設定で作ったやつ)。設定が完了したら起動、CentOS7 install を選択してOSインストールを開始する。

  言語設定は日本語、インストール先は自動、ネットワークをオンにしてインストール開始。

  ユーザーの設定で、ROOTパスワードユーザーの作成をしとく。

### ネットワークアダプターへのIPアドレスの設定

  ``` /etc/sysconfig/network-script/ifcfg-enp0s8 ``` を編集。
  ``` ONBOOT=yes ``` に変更。

### SSH接続確認

  仮想マシンで``` ip a ```コマンドでipアドレスを確認して、Ubuntuからsshで接続できることを確認。

  ``` ssh ipアドレス ```

### yumのプロキシー設定

  ``` /etc/yum.conf ``` にどこでもいいので([main]より下)

  ``` proxy=http://172.16.40.1:8888 ```を追加。

### wgetのインストール&プロキシー設定

  ``` yum install wget ``` で*wget*をインストール。

  ``` /etc/wgetrc ```に

  ``` http_proxy=http://172.16.40.1:8888 
  https_proxy==http://172.16.40.1:8888 ```

  プロキシ設定したらアップデートをする。

  ``` yum update ```

## 1-2 WordPressを動かす

  Apache HTTP Server,MySQL,PHPをインストール。

  ``` yum -y install httpd ```でApacheインストール。

  ``` yum -y install mysql mysql-devel mysql-server mysql-utilities ```でMySQLインストール。

  ``` yum -y install php php-mysql php-mbstring ```

  デーモンを起動する。

  ``` systemctl start httpd ```
  ``` systemctl start mysql ```

  データベース作成する。

  ``` mysql -uroot -p ```

  mysql>``` create database データベース名；```
  mysql>``` grant all on データベース名.* to 'ユーザー名'@'localhost' identified by 'パスワード'; ```
  mysql>``` exit ```

  WordPressをインストール。
  ``` cd /var/www/html/ ```
  ``` wget https://ja.wordpress.org/latest-ja.tar.gz ```
  ``` tar -xzvf latest-ja.tar.gz ```
  ``` mv wordpress/* ./ ```

  
