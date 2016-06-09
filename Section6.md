# Section 6 AWS(Amazon Web Services)  
  
## 6-0 AWSコマンドラインインターフェースのインストール  
  
  詳しく書いている[公式サイト](http://docs.aws.amazon.com/ja_jp/cli/latest/userguide/installing.html#install-with-pip)  
  
### AWS CLIをインストールする前にPythonとpipがあるか確認  
  
  ` $ python --version `  
  ` $ pip --help `  
  
  Pythonはバージョン2.7でやるよ。。。  
  
  なかったらインストールしようね。  
  Python2.7 ` $ sudo apt-get install python2.7 ` でインストール  
  ` $ python --version ` ` python 2.7.9 `て帰ってきたらおｋ  

  pip ` $ curl -O https://bootstrap.pypa.io/get-pip.py ` でインストールスクリプトダウンロードして  
  ` $ sudo python27 get-pip.py ` Pythonでスクリプト動かしてインストール  
  
### AWS CLIインストールしてくよ  
  
  ` $ sudo pip install awscli ` でインストール  
  ` $ aws help ` コマンド実行できたらインストール完了よ!  
  
## AWS EC2 + Ansible  
  
  AWSのダッシュボードでEC2選択うう。  
  インスタンス作成する前にやることがあるんだ。  
  
### Amazon EC2 セットアップ  
  
  まずはじめにコンソール画面の右上辺りに**Oregon**てあるので**Asia(Tokyo)**選択  
  
  左側のナビゲーションバーでキーペアってやつクリックしてキーペア作成よ!  
  
  キーペアネームは何でもいいけど安定の学籍番号うちました(DLされるよん)  
  
  ` $ chmod 400 キーペアネーム.pem `で権限与えてこ、  
  
### Virtual Private Cloud(VPC)作成してくよ  
  
  
