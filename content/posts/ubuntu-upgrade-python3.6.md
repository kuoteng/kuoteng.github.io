Title: ubuntu 16.04.3 升級python3.6而有的問題
Date: 2018-03-05
Category: Linux
Tags: ubuntu
Slug: ubuntu-upgrade-pytyon3.6
Author: kuoteng

因為ubuntu 16.04.3 預裝的是python 3.5.2的版本
先更新apt庫後安裝較新的python 3.6.4，並重新使用python3.6執行get-pip.py安裝pip
最後以symbolic link的方式，將原本連結到python3.5的python3重新指向python3.6



結果後來發現...出現了gnome-terminal無法打開的問題
原本以為是gcin（我有另外裝）相衝，重新update後更新版本，結果發現問題還是沒解決
最後發現有兩個問題ImportError: cannot import name '_gi'、ModuleNotFoundError: No module named 'apt_pkg'
重新以apt工具安裝python-apt沒有用...
後來發現應該是升級python遺留的模組造成的



解決方法：




```sh
$ cd /usr/lib/python3/dist-packages/
$ sudo cp apt_pkg.cpython-35m-x86_64-linux-gnu.so apt_pkg.cpython-36m-x86_64-linux-gnu.so
$ cd /usr/lib/python3/dist-packages/gi
$ sudo cp _gi.cpython-35m-x86_64-linux-gnu.so _gi.cpython-36m-x86_64-linux-gnu.so
$ sudo cp _gi.cpython-35m-x86_64-linux-gnu.so _gi_cairo.cpython-36m-x86_64-linux-gnu.so
```

gnome-terminal恢復正常了XD


---



另外，gcin更新後一直跳出`cannot open /usr/share/gcin/table/.kbm`的問題
我直接重新安裝就恢復正常了

```sh
$ sudo dpkg -i --force-all /var/cache/apt/archives/gcin-im-client_2.8.5+eliu-4_amd64.deb
$ sudo dpkg -i --force-all /var/cache/apt/archives/gcin_2.8.5+eliu-4_amd64.deb
```
