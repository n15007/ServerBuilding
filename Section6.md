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
  
## 6-1 AWS EC2 + Ansible  
  
  AWSのダッシュボードでEC2選択うう。  
  インスタンス作成する前にやることがあるんだ。  
  
### Amazon EC2 セットアップ  
  
  まずはじめにコンソール画面の右上辺りに**Oregon**てあるので**Asia(Tokyo)**選択  
  左側のナビゲーションバーでキーペアってやつクリックしてキーペア作成よ!  
  キーペアネームは何でもいいけど安定の学籍番号うちました(DLされるよん)  
  ` $ chmod 400 キーペアネーム.pem `で権限与えてこ、  
  
  次にナビゲーションバーでセキュリティグループ選択する  
  グループ作成でグループ名は何でもいいけど安定n(ry 説明も適当に。  
  
  VPCはデフォルトで、そんでもってインバウンドにルール追加。  
  追加するルールは3つでタイプはそれぞれ***HTTP・HTTPS・SSH***それ以外はデフォ。  
  
  
  はい、前提終了。  
  
### インスタンスを起動する  
  
  っじゃいつも通りナビゲーションバーからインスタンスを選んでね。  
  
  インスタンス作るときにAMIとインスタンスタイプ選ぶんだけど、  
  どれを選ぶかは先生が説明してくれてるはずよ!!  
  - AMI  
    - Amazon Linux AMI 2016.03.1(HVM),SSD Volume Type  
  - インスタンスタイプ  
    - t2.micro  
  
  インスタンスタイプまで選択できたら確認と作成押していいぉ、  
  でもね、セキュリティグループ選ばなあかんねんな、  
  割当で既存のやつチェックマークからの自分の選んでな!!  
  そんでもって作成。  
  
### 作ったインスタンスにSSHする  
  
   
