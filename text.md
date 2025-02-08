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
semanage user -l
```

- SELinxu ユーザーと Linux ユーザーのマッピング情報

```shell
senanage login -l
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
ps -eo label,cmd | grep named | grep -v grep
```

```shell
ps -Z | grep sshd
```

- ファイルやディレクトリーのタイプは `ls` コマンドに `-Z` オプションを指定する

```shell
ls -Z /etc/named.conf
```

ポートのタイプは `semanage port -l` `で確認できる

```shell
semanage port -l | grep dns_port
```
