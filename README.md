# Ansible を使用して、さくらVPS（CentOS7.2）を初期設定

## 設定項目

* ping で疎通確認
* ユーザの追加、wheelグループ所属
* wheelグループにsudo権限追加
* rootでのssh禁止
* sshポート番号の変更

## 使用方法

### Ansible資材の取得

```
$ git clone https://github.com/zunda-cafe/sakura_init.git
```

### 資材の編集

注意点

* ★箇所を変更
* init.ymlの「パスワード(hash化)」は passwd_hash.txt に記載のコマンドを実行して取得した文字列を使用する
* rootユーザのパスワードはさくらVPSのrootパスワードを設定する
* 「ansibleユーザのパスワード」は任意だが、ファイル間で統一すること


#### hosts_sakura_init（初期設定用ホスト情報）

```
[sakura_init]
さくらVPSのIPアドレス★

[sakura_init:vars]
：（中略）
ansible_ssh_pass=rootユーザのパスワード★
```

#### hosts_sakura（戻し手順用ホスト情報）

````
[sakura]
さくらVPSのIPアドレス★

[sakura:vars]
ansible_port=変更後のSSHポート番号★
ansible_user=ansible
ansible_ssh_pass=ansibleユーザのパスワード★
ansible_become_pass=ansibleユーザのパスワード★
````

#### init.yml（初期設定用手順ファイル）

````
--
- hosts: sakura_init
  become: yes
  vars:
    password: "追加ユーザのパスワード(hash化)"★
    users:
      - { name: ansible,  password: "{{ password }}" }
      - { name: foo,  password: "{{ password }}" }
      - { name: bar,  password: "{{ password }}" }
      - { name: baz,  password: "{{ password }}" }
    ssh_port: 変更後のSSHポート番号★
    after_ansible_user: ansible
    after_ansible_ssh_pass:ansibleユーザのパスワード★
    after_ansible_become_pass: "{{ after_ansible_ssh_pass }}"

  tasks:
  - name: ping で疎通確認
：（中略）
````

#### undo_init.yml（戻し手順ファイル）

変更なし

### ansible-playbook の実行 

* 初期設定の実行

```
$ ansible-playbook init.yml -i hosts_sakura_init
```

* 戻し手順の実行

```
$ ansible-playbook undo_init.yml -i hosts_sakura
```
