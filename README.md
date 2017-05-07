# さくらVPS（CentOS7.2）の初期設定

## 設定項目

* ping で疎通確認
* ユーザの追加、wheelグループ所属
* wheelグループにsudo権限追加
* rootでのssh禁止
* sshポート番号の変更

## 使用方法

### 初期設定

```
$ ansible-playbook init.yml -i hosts_sakura_init
```
