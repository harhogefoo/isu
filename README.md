# [ISU]Iikanjini SpeedUp cheat sheet
## systemctl
※CentOS7では実質`chkconfig`が廃止され，サービスに関連することはsystemctlを使う
### 実行中のサービス一覧
`$ systemctl`
### サービス起動
`$ systemctl start [service_name].service`
### サービス停止
`$ systemctl stop [service_name].service`
### サービス再起動
`$ systectl restart [service_name].service`
### サービスの自動起動設定[enable/disable]の一覧
`$ systemctl list-unit-files -t service`
### 個別にサービスの自動起動設定を確認
`$ systemctl status [service_name].service`
### サービスの自動起動有効化
`$ systemctl enable [service_name].service`
### サービスの自動起動無効化
`$ systemctl disable [service_name].service`
### systemctlのログ出力
`$ jornalctl -u [service_name].service`  
`-b` 直近の起動ログ  
`-k` エラーを強調する  
`-f` リアルタイムな表示  

## nginx
### 現在設定されているconfファイルの文法チェック
`# nginx -t`

