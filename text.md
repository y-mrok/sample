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
  - ログファイル
    - プロセス名_log_t
  - 公開ファイル
    - わかりやすい名前_t
  - ポート番号
    - サービス名_port_t
