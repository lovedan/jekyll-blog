---
title: 移除Mac的.DS_Store、._ 檔
tags: [Mac技巧]
categories: 
- 苹果相关
- Mac技巧
date: 2017-02-16 01:28:40
thumbnail: https://lh3.googleusercontent.com/-zE-dLaGRh9s/WKUC65Rm6pI/AAAAAAAABoQ/AYijQp5xALk/s0/2017-02-16_10-39-56.png
---
<!--excerpt-->

## 前言

這教學不單對Mac 的用戶有效，只要你的電腦跟Mac 機的Dropbox 同步過，都會發現電腦里出現大量的小檔案，而且全部檔案的開首都是 “._" (dot underscore)。

雖然對系統的儲存空間影響很少，但同步的過程會因為要處理的檔案非常多而影響效能，尤其對於使用外置記憶卡的我。

據說，這些.DS_Store 檔案是記錄資料夾的文件資料的（如果打開系統資訊 ，會顯示不同類型的檔案佔用的空間），如果剷除了，"Storage" 就會就會把所有檔案顯示成 “Others" 。

## 事前準備

這些檔案其實是隱藏的，所以要讓系統顯示所有檔案：
1.在Terminal 輸入：【``defaults write com.apple.finder appleshowallfiles true``】

這樣就會修改系統的設定，讓系統顯示全部檔案，包括被隱藏的。

## 制止``.DS_Store``繼續產生
1.下載 Death_To_DS_Store (直接下載：http://www.aorensoftware.com/Downloads/Files/DeathToDSStore.zip )
2.直接開啟解壓出來的 ``Death_To_DSStore.app`` （剔選Launch Agent來自動啟動這軟件，那麼一開機就會開始阻止 ``.DS_Store``）

## 剷除所有``._ 檔案``（``dot_clean()``）——不能一勞永逸

這方法會剷除所有存在在儲存空間的 "``._``" 檔，而系統也不會重新產生這些檔案；但對於新的檔案，還是會有 ``._`` 檔案的。

現有的付費方案 (``BlueHarvest``，售價``HKD108``)，也只是把以下的工序自動化，并在每次寫檔案時執行一次而已。
1.打開Terminal，輸入【dot_clean /path_to_folder/】
```
(e.g.) dot_clean /Users/alan/Documents
```
這樣系統就會自動把 ._ 的檔案信息融合到原本的檔案。如果原本的檔案消失了，都會被剷除。

建議一個星期執行一次就可以了，一次過把所有這類型的檔案鏟掉。

## 在Windows 上剷掉 ``._`` 檔

如果大家同時再Mac 跟 Windows 系統上用Dropbox 同步文件的話，相信大家都會看過，即使在Windows 上也發現這些煩人的檔案；就算沒有用 Dropbox, 用 USB Thumbdrive 把檔案傳來傳去時，Mac 系統也會“順便”把這些檔案“附送”給Windows。

要剷除的話，其實很簡單，只需要一句 CMD Command 【Run: “cmd"】
```
dir /S ._*
```
這個可以在剷除檔案前，確保沒有亂剷
```
del /S ._*
```
好了，真的要動手咯～

解釋一下
* ``/S``: 就是 Recursive 的意思，就是徹底搜出 ._ 的檔案
* ``*``: 是asterisk, wildcard 的意思啦～

## 後話

其實如果不使用【Dropbox】 和 【外置儲存裝置】的話，系統這個預設的行為是沒有影響的；但這系統的行為影響最大的是【外置儲存裝置】。

由於SSD 對我最主要的用途是快速啟動軟件，跟暫時處理檔案用的。如果用作同步，這些經常讀寫檔案的行為，還是用【外置儲存裝置】比較划算（這算不算）
