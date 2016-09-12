# [ISU]Iikanjini SpeedUp cheat sheet
## systemctl
※CentOS7では実質`chkconfig`が廃止され，サービスに関連することはsystemctlを使う
#### サービス一覧の確認
`$ systemctl list-unit-files -t service`
#### 個別にサービスの状態を確認
`$ systemctl status sshd.service`
#### systemctlでサービスの有効化
`$ systemctl enable sshd.service`
#### systemctlでサービスの無効化
`$ systemctl disable sshd.service`
#### systemctlのログ出力
`$ jornalctl -u sshd.service`  
`-b` 直近の起動ログ  
`-k` エラーを強調する  
`-f` リアルタイムな表示  
