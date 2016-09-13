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

