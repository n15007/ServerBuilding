# Section 3 Anisibleによる自動化とテスト

## 3−0 Ansibleのインストール

  まずここで言いたいのが、Vagrantはapt-getでインストールしてないわよね??  
  してるなら出直して来なさい!! [0-2 Vagrant インストール](https://github.com/n15007/ServerBuilding/blob/master/Section0.md#0-2-vagrant-インストール)

  それではAnsibleをインストールしましょう。

```
 $ sudo apt-get install software-properties-common
 $ sudo apt-add-repository ppa:ansible/ansible
 $ sudo apt-get update
 $ sudo apt-get install ansible
```
  インストールしたらバージョン確認で入ってるか確認。  
`   $ ansible --version `

## 3−1 AnsibleでWordpressを動かそう

  仮想環境のディレクトリ(Vagrant)の場所でechoを使ってhostsファイル作るよん。  
  ` $ echo 自分の仮想鯖のIPアドレス > hosts `

  pingで確確認認確認認。。。  
  ` $ ansible 自分の鯖IPアドレス -i hosts -m ping --private-key ~/.vagrant.d/insecure_private_key -u vagrant -k `

  playbook.ymlに設定を書いてくよ。ファイルうｐしとくね(はーと)

  playbook.ymlと同じとこにnginx,php-fpm,wordpressディレクトリをつくってこーっせ!!  
  それぞれのディレクトリにWordpressの設定ファイルを作って記述してくんだお。  
  ファイルうｐしとくね(はーと)  

  ではさっそくplaybook実行してこーよ!!  
  `$ ansible-playbook playbook.yml -i hosts  --private-key ~/.vagrant.d/insecure_private_key -u vagrant -k`  

  できたんじゃない??そうじゃない??  
  できなかったらとりまエラー見たほうがいいっぽいよ。。。。  
  さっきのコマンドに**-vv** 追加して実行したら出力してくれるセョ

### 3-1-2 VagrantfileからAnsibleを呼び出す

  題名通り、要はそうゆうことよ!!ファイルうｐしと(ry
