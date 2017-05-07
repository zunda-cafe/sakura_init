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

### 初期設定

```
$ ansible-playbook init.yml -i hosts_sakura_init
```

### 戻し

```
$ ansible-playbook undo_init.yml -i hosts_sakura
```
