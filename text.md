## OS の名前、バージョン

```shell
cat /etc/os-relase
```

- OS の名前
- OS のバージョン

## OS のディストリビューション

```shell
cat /etc/redhat-release
```

## SELInux

### 状態確認

```shell
setstatus
```

- SELinux status: 現在の SELinux の状態

  - Enforcing
    SELinux によるアクセスのチェックおよび制限が有効な状態
  - permissive
    アクセスのチェックはされるが制限はしない状態
  - disabled
    アクセスチェックおよび制限が無効な状態

- Loaded policy name: ポシリータイプ

 ### SELinux ユーザー

- SELinux ユーザーの確認

```shell
sudo semanage user -l
```

- SELinxu ユーザーと Linux ユーザーのマッピング情報

```shell
sudo senanage login -l
```

- ユーザーのコンテキスト情報の確認

```shell
id -Z
```

### タイプ

- コンテキストのフォーマット

```text
SElinuxユーザ:ロール:タイプ:セキュリティレベル:カテゴリ
```

- ドメイン
  - プロセス名_t  
    init_t (systemd) , named_t (nameed) , sshd_t (sshd) , syslogd_t (rsyslog) 等々
- タイプ（ファイル / ディレクトリー / ポート番号）
  - 実行可能ファイル
    - プロセス名_exec_t  
      httpd_exec_t (/usr/sbin/httpd)  
      syslogd_exec_t (/usr/sbin/rsyslogd)
  - 設定ファイル
    - プロセス名_config_t  
      httpd_config_t (/etc/httpd/conf/httpd.conf)
  - ログファイル
    - プロセス名_log_t  
      httpd_log_t (/var/log/httpd/access_log)  
      var_log_t (/var/log/messages)
  - 公開ファイル
    - わかりやすい名前_t  
      httpd_sys_contetn_t
  - ポート番号
    - サービス名_port_t  
      dns_port_t (tcp/53 , udp/53 等)  
      ftp_data_port_t (tcp/20, udp/68 等)

- プロセスのタイプは `ps` コマンドに `-e` または `-Z` オプションを指定する

```shell
sudo ps -eo label,cmd | grep named | grep -v grep
```

```shell
sudo ps -Z | grep sshd
```

- ファイルやディレクトリーのタイプは `ls` コマンドに `-Z` オプションを指定する

```shell
sudo ls -Z /etc/named.conf
```

ポートのタイプは `semanage port -l` `で確認できる

```shell
sudo semanage port -l | grep dns_port
```

- アトリビュート  
  - ドメインやタイプのラベルをグループ化したもの
  - SELinux のルールを定義するときに使用する

- アトリビュートに含まれるタイプの確認

```shell
seinfo -a httpd_content_type -x | grep httpd
```

```shell
seinfo -a domain -x | grep httpd
```

### SELinux と NGINX によるリバースプロキシー

- [2.4. HTTP トラフィックのリバースプロキシーとしての NGINX の設定 | Web サーバーとリバースプロキシーのデプロイ](https://docs.redhat.com/ja/documentation/red_hat_enterprise_linux/9/html/deploying_web_servers_and_reverse_proxies/configuring-nginx-as-a-reverse-proxy-for-the-http-traffic_setting-up-and-configuring-nginx#configuring-nginx-as-a-reverse-proxy-for-the-http-traffic_setting-up-and-configuring-nginx)
- [13.3. ブール値 | SELinux ユーザーおよび管理者のガイド](https://docs.redhat.com/ja/documentation/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/sect-managing_confined_services-the_apache_http_server-booleans#sect-Managing_Confined_Services-The_Apache_HTTP_Server-Booleans)

### ブール値の確認

`seinfo` 以外は現在の設定値が表示される。

- semanage boolean -l
- getsebool -a
- gesebool ブール値
- sestatus -b
- seinfo -b

```shell
sudo semanage boolean -l | egrep "SELINUX|zabbix"
```

```shell
sudo getsebool -a | grep "zabbix"
```

```shell
sesearch -A | grep httpd_can_connect_zabbix
```

