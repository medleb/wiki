# Ubuntu18.04LTSß
```
admin$ sw_vers
ProductName:	Mac OS X
ProductVersion:	10.14.4
BuildVersion:	18E226
```
macbook pro(15-inch,2018)  
macOS mojave バージョン10.14.4  
Parallels Desktop14 on Ubuntu18.04

## 1.パッケージのアップデート&アップグレード
```
$ sudo apt update
$ sudo apt upgrade
```
途中でキーボードレイアウト選択画面が現れたらApple Aluminium(JIS)を選択

IME設定  
https://ottan.xyz/ubuntu-16-04-ime-on-off-4913/


## 2.ディレクトリ名を日本語から英語に変更する

```
$ LANG=C xdg-user-dirs-gtk-update
```

※GUIの方がかわらない場合  
~/.config/user-dirs.dirsを編集する



## 3.Parallels Tools インストール
画面右上の黄色い三角(ビックリマーク)からアマウント  
```
$ cd /media/{user_name}/ParallelsTools/
$ ./install
```

## ※キーボード入力設定
```
$ sudo dpkg-reconfigure keyboard-configuration
```
Apple Aluminium(JIS)を選択
```
sudo vim /usr/share/ibus/component/mozc.xml
```
defaultをjpに変更
```
- <layout>default</layout>
+ <layout>jp</layout>
```
設定-地域と言語-入力ソースから[日本語]を削除する  
インストールされている言語の管理から日本語パッケージインストール

## javaインストール
- openjdk8
```
$ sudo apt install openjdk-8-jdk
$ java -version
openjdk version "1.8.0_191"**
OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
```

- openjdk11
```
$ sudo apt install openjdk-11-jdk
$ java -version
openjdk version "11.0.2" 2019-01-15
OpenJDK Runtime Environment (build 11.0.2+9-Ubuntu-3ubuntu118.04.3)
OpenJDK 64-Bit Server VM (build 11.0.2+9-Ubuntu-3ubuntu118.04.3, mixed mode, sharing)
```

- バージョン切り替え
```
$ java -version
openjdk version "11.0.2" 2019-01-15
OpenJDK Runtime Environment (build 11.0.2+9-Ubuntu-3ubuntu118.04.3)
OpenJDK 64-Bit Server VM (build 11.0.2+9-Ubuntu-3ubuntu118.04.3, mixed mode, sharing)

$ sudo update-alternatives --config java
alternative java (/usr/bin/java を提供) には 2 個の選択肢があります。

  選択肢    パス                                          優先度  状態
------------------------------------------------------------
* 0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      自動モード
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      手動モード
  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      手動モード

現在の選択 [*] を保持するには <Enter>、さもなければ選択肢の番号のキーを押してください: 2
update-alternatives: /usr/bin/java (java) を提供するためにマニュアルモードで /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java を使います

$ java -version
openjdk version "1.8.0_191"
OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
```

## Qt5.12.0インストール
- インストーラーダウンロード
```
$ wget http://download.qt.io/official_releases/qt/5.12/5.12.0/qt-opensource-linux-x64-5.12.0.run
```

- 実行
```
# 実行権限をつける
$ chmod +x qt-opensource-linux-x64-5.12.0.run
$ ./qt-opensource-linux-x64-5.12.0.run
```

- コンポーネントの選択
1.Qt-Desktop gcc 64-bit  
2.Tools-Qt Creator 4.8.0

## OpenCVインストール
- OpenCV GitHub(https://github.com/opencv/opencv)  
インストール参考サイト(http://www.codebind.com/linux-tutorials/install-opencv-ubuntu-18-04-lts-c-cpp-linux/)

- 依存関係インストール
```
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install build-essential cmake git libgtk2.0-dev \
pkg-config libavcodec-dev libavformat-dev libswscale-dev

# python3.5-devが見つからなかったのでリポジトリを追加
########################################################
# raspiでadd-apt-repositoryがエラーになる
# ⇛ sudo apt install apt-file software-properties-common
########################################################
$ sudo add-apt-repository ppa:deadsnakes/ppa
$ sudo apt install python3.5-dev python3-numpy libtbb2 libtbb-dev

# libjasper-devが見つからなかったので無視(JPEG2000を用いなければ特に問題なし)
########################################################
# raspiでlibface-devが見つからない
# ⇛ 放置中
########################################################
$ sudo apt install libjpeg-dev libpng-dev libtiff5-dev libjasper-dev \
libdc1394-22-dev libeigen3-dev libtheora-dev libvorbis-dev libxvidcore-dev \
libx264-dev sphinx-common libtbb-dev yasm libfaac-dev libopencore-amrnb-dev \
libopencore-amrwb-dev libopenexr-dev libgstreamer-plugins-base1.0-dev \
libavutil-dev libavfilter-dev libavresample-dev

# sudo apt install libopencv-dev
```

- OpenCV取得
```
$ sudo -s
$ cd /opt
$ git clone https://github.com/Itseez/opencv.git
$ git clone https://github.com/Itseez/opencv_contrib.git
```

- OpenCVビルドとインストール
```
$ cd opencv
$ mkdir release
$ cd release
$ cmake -D BUILD_TIFF=ON -D WITH_CUDA=OFF -D ENABLE_AVX=OFF -D \
WITH_OPENGL=OFF -D WITH_OPENCL=OFF -D WITH_IPP=OFF -D WITH_TBB=ON -D \
BUILD_TBB=ON -D WITH_EIGEN=OFF -D WITH_V4L=OFF -D WITH_VTK=OFF -D \
BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D CMAKE_BUILD_TYPE=RELEASE -D \
CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=/opt/opencv_contrib/modules /opt/opencv/
$ make -j4
$ make install
$ ldconfig
$ exit
```

- バージョン確認
```
$ pkg-config --modversion opencv
3.2.0
```

## Anacondaインストール
- インストーラーダウンロード
(https://www.anaconda.com/distribution/#download-section)  
Anaconda3-2019.07-Linux-x86_64.sh(2019/08/11現在)

- インストーラー実行
```
$ chmod +x Anaconda3-2019.07-Linux-x86_64.sh
$ ./V¥Anaconda3-2019.07-Linux-x86_64.sh
```

- PATH追加
```
$ vi ~/.bashrc
```

- ~/.bashrc
```

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

export PATH=$PATH:/opt/Qt5.12.0/5.12.0/gcc_64/bin
export PATH=$PATH:~/anaconda3/bin ← 追加
```

- ~/.bashrc読み込み
```
$ source ~/.bashrc
```

- インストール確認
```
$ conda -V
conda 4.7.10
```
