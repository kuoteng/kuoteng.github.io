Title: ubuntu & windows 的時間衝突
Date: 2018-03-06
Category: Linux
Tags: ubuntu
Slug: ubuntu-dual-system-time-conflict
Author: kuoteng

根據ArchWiki，電腦上的通常以硬體儲存時間，並且將其解析為通用的時間格式
有兩類的時間格式：UTC及localtime，UTC被UNIX系統使用，而localtime則在windows上系統被使用
至於linux...看來ubuntu是將它視為UTC
在ubuntu 15.04以上的版本可以使用以下的指令開啟localtime

```sh
$ timedatectl set-local-rtc 1
```


