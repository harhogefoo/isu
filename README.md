# [ISU]Iikanjini SpeedUp cheat sheet
## ●systemctl
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

---

## ●nginx
### 現在設定されているconfファイルの文法チェック
```
# nginx -t
```

### kataribe導入
```
$ apt-get update
$ apt-get install golang
$ mkdir -p ~/gocode && echo 'export GOPATH=$HOME/gocode' >> ~/.bashrc
$ echo 'export PATH=$GOPATH/bin:$PATH' >> ~/.bashrc
$ source ~/.bashrc
$ go get github.com/matsuu/kataribe
```

### kataribe実行
```
# cat /var/log/nginx/access.log | kataribe -f $GOPATH/src/github.com/matsuu/kataribe/kataribe.toml
```

---

## ●MySQL
### Find my.cnf
```
$ mysql --help | grep my.cnf
```
### Write config file for ISU
```
innodb_buffer_pool_size=1GB // インスタンスのメモリサイズを超えるとエラーになるので注意
innodb_flush_log_at_trx_commit=2 // 1に設定するとトランザクション単位でログを出力するが 2 を指定すると1秒間に1回ログファイルに出力するようになる
innodb_flush_method=O_DIRECT //データファイル/ログファイルの読み書き方式を指定
```
```
# if error was occuered
rm /var/lib/mysql/ib_logfile0
rm /var/lib/mysql/ib_logfile1
```
### Check Parameters
```
mysql> select @@[param_name];
```
### Add Index
```
ALTER TABLE [table_name] ADD INDEX [index_name]([column_name])
```

---

### Dump Slow Query
### Command
```
mysql> set global slow_query_log = 1;
mysql> set global long_query_time = 0;
mysql> set global slow_query_log_file = "/tmp/slow.log";
mysql> set global log_queries_not_using_indexes = 1;
```
#### Write config file for log
```
[mysqld]
slow_query_log = 1
slow_query_log_file = /var/lib/mysql/mysqld-slow.log
long_query_time = 0
log_queries_not_using_indexes = 1
```
---
### Analyze Slow Query
#### Use mysqldumpslow
```
$ mysqldumpslow -s t /path/to/log > /path/to/output
```
#### Use pt-query-digest
```
$ wget https://repo.percona.com/apt/percona-release_0.1-3.$(lsb_release -sc)_all.deb
$ sudo dpkg -i percona-release_0.1-3.$(lsb_release -sc)_all.deb
$ sudo apt-get install percona-toolkit
$ pt-query-digest /tmp/slow.log > /tmp/digest.txt
```

---

## Change Kernel Params
```
$ vim /etc/sysctl.d/99-sysctl.conf
```
```
net.ipv4.tcp_max_tw_buckets = 2000000 //  システムが同時に保持する time-wait ソケットの最大数。この数を越えると、time-wait ソケットは直ちに破棄され、警告が表示されます。この制限は単純な DoS 攻撃を防ぐためにあります。わざと制限を小さくしてはいけません。ネットワークの状況によって必要な場合は、 (できればメモリを追加してから) デフォルトより増やすのはかまいません。
net.ipv4.ip_local_port_range = 10000 65000 //ローカルポートとして利用できるアドレスの範囲 デフォルトだと28232 個なので増やす。ポートが足りなくなるとポートのどれかが解放されるまで新しいコネクションを確立できなくなる
net.core.somaxconn = 32768 // TCPソケットが受け付けた接続要求を格納するキューの最大長
net.core.netdev_max_backlog = 8192 // カーネルがキューイング可能なパケットの最大個数
net.ipv4.tcp_tw_reuse = 1 // TIME_OUT 状態のコネクションを再利用 (1使いまわす、0使いまわさない)
net.ipv4.tcp_fin_timeout = 10 // TCPの終了待ちタイムアウト秒数を設定(default 60)
```
Apply
```
$ sudo /sbin/sysctl -p
```

---

## ●commands (使えるコマンド集）
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

