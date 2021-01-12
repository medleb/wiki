# Rasberry Pi 3 Model B+

Attention: 最終更新日 2021/01/12


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
