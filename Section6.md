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
インスタンス作成する前にやることがあるんだああ。  
  
### Amazon EC2 セットアップ  
  
まずはじめにコンソール画面の右上辺りに**Oregon**てあるので**Asia(Tokyo)**選択  
左側のナビゲーションバーでキーペアってやつクリックしてキーペア作成よ!  
キーペアネームは何でもいいけど安定の学籍番号うちました(DLされるよん)  
  
` $ chmod 400 キーペアネーム.pem `で権限与えてこ、  
  
次にナビゲーションバーでセキュリティグループ選択する  
グループ作成でグループ名は何でもいいけど安定のがｋ(ry   
説明も適当に。  
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
  
作ったインスタンスで接続って押したら親切に接続する方法教えてくれるんだけど  
grusの方でしか接続できないらしいので落としたキーペアをgrusに送らなあかんよ  
しかもな、ansibleでやるならその設定ファイルやらなんやらも送らなあかんねんな  
  
` $ scp -r ファイル名 学籍番号@172.16.40.2: ` で  
grusのところにファイルをコピーしたらgrusに接続しよう  
  
Section3で作ったansible使うなら設定ファイルを変更しなければいけない。  
MariadbでやってたらMySQLにしなきゃ!!  
  
準備ができたらSSHしよ。  
  
  
接続できたらansible起動するよ。  
` ansible パブリックIP -i hosts -m ping --private-key キーペア -u ec2-user -k `   
` ansible-playbook playbook.yml -i hosts -u ec2-user -k --private-key n15007.pem -vv `  
  
無事エラー吐かなければブラウザでパブリックID打ってWordPressできてると思う。  
  
### AMI(Amazon Machine Image)を作る  
  
要はVagrantなんたらBox的な、らしい。たぶん。。。  
  
インスタンス右クリックでイメージ→イメージ作成でおｋよ。  
ナビゲーションバーからAMIへ、イメージが作られていることを肉眼で確認。  
自分が作ったAMI選択して作成押したら今までやった作業無しでインスタンスできるね!  
セキュリティグループ選ぶの忘れずに,,,,,,,,,  
  
## 6-2 AWS EC2(AMIMOT)  
  
他の人が作ったAMIを使ってもインスタンスって作れるみたい。  
今回はAMIMOTOさん??のWordpressが起動できるやつ使ってくよ!!  
  
インスタンス作成でステップ1の段階でクイックスタート以外に  
マイAMIやらコミュニティAMIやら選択できるんだけど、AWS Marketplaceで  
検索フォームでWordpressって検索すると色々なAMIでてくるのよ、  
そのなかからAMIMOTOっちのAMIを探しだすのよ!!!!!!  
  
***WordPress powered by AMIMOTO (HVM)*** ってやつ  
  
あとはさっきやった通りよ、(t2.micro セキュリティグループ)  
  
  
インスタンス作成できたらパブリックDNSかパブリックIPをURLにうったらWordpressでるんじゃない??  
  
  
## 6-3 S3  
  
ファイルを保存したり公開したりできるサービスだって、いいね。  
とりま、やることはファイルをうｐロード後htmlファイルを公開。  
  
  
```
# バケット作成
$ aws s3 mb s3://バケットネーム(自由)

# 適当に作ったhtmlファイルをうｐ
$ aws s3 cp 作った.html s3://バケットネーム --recursive
upload: なんたらー

# ファイルを公開する
$ aws s3 cp --acl public-read 作った.html s3://バケットネーム/作った.html

```


