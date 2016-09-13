# [ISU]Iikanjini SpeedUp cheat sheet
## systemctl
※CentOS7では実質`chkconfig`が廃止され，サービスに関連することはsystemctlを使う
### 実行中のサービス一覧
```bash
$ systemctl
```

### サービス起動
```bash
$ systemctl start [service_name].service
```

### サービス停止
```bash
$ systemctl stop [service_name].service
```

### サービス再起動
```bash
$ systectl restart [service_name].service
```

### サービスの自動起動設定[enable/disable]の一覧
```bash
$ systemctl list-unit-files -t service
```

### 個別にサービスの自動起動設定を確認
```bash
$ systemctl status [service_name].service
```

### サービスの自動起動有効化
```bash
$ systemctl enable [service_name].service
```
### サービスの自動起動無効化
```bash
$ systemctl disable [service_name].service
```

### systemctlのログ出力
```bash
$ jornalctl -u [service_name].service
```
`-b` 直近の起動ログ  
`-k` エラーを強調する  
`-f` リアルタイムな表示  

## nginx
### 現在設定されているconfファイルの文法チェック
```
# nginx -t
```

## commands (使えるコマンド集）
### システム情報の表示
```
$ uname -a
```
`-a` コンピュータ（ハードウェア)の種類，ネットワークにおけるホスト名，OSのリリース番号，OSの名称

### CPU情報の確認
```
$ cat /proc/cpuinfo
```

### メモリ情報の確認
```
$ cat /proc/meminfo
```

