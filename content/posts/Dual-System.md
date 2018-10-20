Title: ubuntu 雙系統建置
Date: 2016
Category: Linux
Tags: ubuntu
Slug: Ubuntu-Dual-System
Author: kuoteng

新學期到了, 又到了該重灌ubuntu雙系統的時間了XDDD（等debian改版後就改裝debian）

既然要重灌, 那就順便紀錄下來吧＠＠

下載並燒錄ubuntu
到ubuntu官網(https://www.ubuntu.com/)下載你想安裝的ubuntu版本iso檔

並且將此iso檔燒錄在光碟片中(我直接使用筆電預設的power2GO來安裝, 不過也可以使用nero等燒錄軟體)

取消快速啟動
快速啟動會讓gnu啟動器失效…所以我們進入電源選項中關閉這個功能





取消安全啟動
開機進入windows開機畫面前按鍵盤「del」（delete鍵）進入bios, 在security中取消secure boot

partition
利用windows磁碟管理工具來給予多的空間給ubuntu

(這部份我以前就做過了XD 所以就不截圖, 不過一般試用可以先試試看20~40GB的空間就好)

使用光碟片安裝
將光碟片放入光碟機中, 關機再開機後再次進入bios, 調整開機順序(startup priority), 將光碟機順位調至最高(如果不是使用光碟片而是隨身碟也是如此), 最後save and exit

然後就會進入ubuntu環境了, 選擇圖形界面中的安裝

ubuntu partition
這邊要注意一塊硬碟最多容納4個主分區(You obviously have a msdos-partitioned disk, so you may only create 4 primary partitions on it.)

/: file system root , ext4, 建議10GB ~ 15GB
swap:虛擬記憶體，通常為實體記憶體的1~2倍, 類型為linux-swap
/boot:啟動文件，給個200M就好
/home: 其他的全給, 類型為ext4
語言支援
安裝完成後, 點選右上角的語言支援(language support), 會跳出安裝的語言尚未完備, 點選安裝並完成安裝後, 登出再登入就可以右上角找到小企鵝輸入法的設定頁面了

參考資料
https://askubuntu.com/questions/666631/how-can-i-dual-boot-windows-10-and-ubuntu-on-a-uefi-hp-notebook

https://read01.com/jND7m.html

http://askubuntu.com/questions/662214/ubuntu-file-system-partition-dual-boot

http://askubuntu.com/questions/343268/how-to-use-manual-partitioning-during-installation

http://www.coctec.com/docs/linux/show-post-140951.html
