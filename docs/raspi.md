# Rasberry Pi 3 Model B+
- バージョン
```
$ lsb_release -a
No LSB modules are available.
Distributor ID:	Raspbian  
Description:	Raspbian GNU/Linux 9.8 (stretch)  
Release:	9.8  
Codename:	stretch
```  


## IPアドレス固定
- ipアドレス確認
```
$ ifconfig 
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.3.3  netmask 255.255.255.0  broadcast 192.168.3.255
        inet6 2400:2412:2de0:5700:e849:113b:3f4:8c04  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::cf30:ff3f:c6f2:e297  prefixlen 64  scopeid 0x20<link>
        ether b8:27:eb:73:e5:9a  txqueuelen 1000  (イーサネット)
        RX packets 3665  bytes 338943 (330.9 KiB)
        RX errors 0  dropped 3  overruns 0  frame 0
        TX packets 1829  bytes 332965 (325.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
- /etc/dhcpcd.confファイル編集
```
$ sudo vim /etc/dhcpcd.conf
```

- 以下を追加
```
interface wlan0
static ip_address=192.168.3.128/24
static routers=192.168.3.1
static domain_name_servers=192.168.3.1
```

- 再起動
```
sudo reboot
```

- 確認
```
$ ifconfig 
wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.3.128  netmask 255.255.255.0  broadcast 192.168.3.255
        inet6 2400:2412:2de0:5700:e849:113b:3f4:8c04  prefixlen 64  scopeid 0x0<global>
        inet6 fe80::cf30:ff3f:c6f2:e297  prefixlen 64  scopeid 0x20<link>
        ether b8:27:eb:73:e5:9a  txqueuelen 1000  (イーサネット)
        RX packets 3665  bytes 338943 (330.9 KiB)
        RX errors 0  dropped 3  overruns 0  frame 0
        TX packets 1829  bytes 332965 (325.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```


## raspi ssh設定
- 鍵生成(mac側)
```
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/karuma/.ssh/id_rsa): ← Enter
Created directory '~/.ssh'. ← ~/.ssh がない場合、ディレクトリ作成
Enter passphrase (empty for no passphrase): ← パスフレーズを入力
Enter same passphrase again: ← もう一度パスフレーズを入力
Your identification has been saved in /home/karuma/.ssh/id_rsa.
Your public key has been saved in /home/karuma/.ssh/id_rsa.pub.
```

- 確認
```
$ ls ~/.ssh
id_rsa ←(秘密鍵)  id_rsa.pub(公開鍵)
```


- .sshディレクトリ作成(raspi側)
```
$ mkdir ~/.ssh
```

- 公開鍵ファイル作成
```
$ touch ~/.ssh/authorized_keys
```

- id_rsa.pubの内容を公開鍵ファイルにコピー

- パーミッション変更
```
chmod 600 ~/.ssh/authorized_keys
```

- /etc/ssh/sshd_config編集
```
$ sudo vi /etc/ssh/sshd_config
```

- #PasswordAuthentication yes" の下の行に "#PasswordAuthentication no" を追加  
パスワードでのログインを禁止し、秘密鍵を持っている人のみログインできるように変更
```
# To disable tunneled clear text passwords, change to no here!
#PasswordAuthentication yes
PasswordAuthentication no ←追加
#PermitEmptyPasswords no
```

- Note  
https://qiita.com/mountcedar/items/43157ff1225c56500655


## 省電力モードOFF
- /etc/rc.localファイル編集
```
$ sudo vim /etc/rc.local
```
- exit0の前に以下を追加
```
# Raspberry Pi 3 Power Management off
$ sudo iwconfig wlan0 power off
```
- offになっていることを確認
```
$ iwconfig
Power Management:off
```


## conda(berryconda)
- インストーラダウンロード  
(公式)https://github.com/jjhelmus/berryconda
- インストーラ実行
```
$wget +x Berryconda3-2.0.0-Linux-armv7l.sh
./Berryconda3-2.0.0-Linux-armv7l.sh
```

## Other
- シャットダウン
```
$ sudo shutdown -h now
```

## Qt5
~~sudo apt install qt-sdk qtbase5-dev libgl1-mesa-dev~~  
qt5が入らないため一旦削除し調査中

## opendjkインストール
```
$ sudo apt install openjdk-8-jdk
```

- バージョン確認
```
java -version
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-8u181-b13-2~deb9u1-b13)
OpenJDK Client VM (build 25.181-b13, mixed mode)
```

## apache2インストール
```
$ sudo apt install apache2
```
- 起動確認  
ブラウザから「 http://raspiアドレス 」にアクセス

## tomcat8ダウンロード
```
$ cd /usr/local
$ wget http://ftp.riken.jp/net/apache/tomcat/tomcat-8/v8.5.40/bin/apache-tomcat-8.5.40.tar.gz
$ sudo tar xvzf apache-tomcat-8.5.40.tar.gz
$ sudo mv apache-tomcat-8.5.40/ tomcat
$ sudo rm apache-tomcat-8.5.40.tar.gz

$ vim /etc/init.d/tomcat
$ sudo chmod 755 /etc/init.d/tomcat
$ sudo /etc/init.d/tomcat start

```

- http://192.168.3.128:8080にアクセス

init.d/tomcat
```
### BEGIN INIT INFO
# Provides: tomcat
# Required-Start: $remote_fs $syslog
# Required-Stop: $remote_fs $syslog
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Start tomcat Server at boot time
# Description: Start tomcat Server at boot time.
### END INIT INFO

#!/bin/bash
# /etc/init.d/tomcat

#source /etc/profile.d/tomcat.sh
export TOMCAT_HOME=/usr/local/tomcat
export CATALINA_HOME=/usr/local/tomcat
export CLASSPATH=$CLASSPATH:$CATALINA_HOME/lib


start(){
  echo "Starting tomcat"
  /usr/local/tomcat/bin/startup.sh
}

stop(){
  echo "Shutting down tomcat"
  /usr/local/tomcat/bin/shutdown.sh
}

case "$1" in
  start)
      start
      ;;
  stop)
       stop
       ;;
  restart)
      stop
      start
      ;;
  status)
      /usr/local/tomcat/bin/catalina.sh version
      ;;
  *)
      echo "Usage: $0 {start|stop|restart|status}"
esac

exit 0
```

- GitBucketデプロイ

```
$ sudo mv gitbucket.war /user/local/webapps/
```
http://192.168.3.128:8080/gitbuckrt/
