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
- 
