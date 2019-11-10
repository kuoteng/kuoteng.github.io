Title: nohup 指令的用法
Date: 2019-10-18
Category: Linux
Tags: command, linux
Slug: nohup-usage
Author: kuoteng

在機器上執行一些長時間的運算，比如說訓練網路，時常需要等待大量時間，而如果這時候你是用終端機連線，有時候不小心斷線...運算就中斷了

這時候當然可以使用 `screen`, `tmux` 等指令創建不受連線中斷影響的終端機 session
不過我比較常用 `nohup`!

`nohup` 可以讓程式忽略 `SIGHUP` 訊號，如果指定背景執行，那就不用怕中斷啦

以下會直接將輸出印給 nohup.out

```sh
    nohup [要執行的程式] &
```

也可以搭配 nice

```sh
    nohup nice [要執行的程式] &
```

也可以指定輸出

```sh
    nohup nice [要執行的程式] > nohup.out &
```

最後，可以搭配 tail 指令來查看輸出

```sh
    tail -f nohup.out
```
