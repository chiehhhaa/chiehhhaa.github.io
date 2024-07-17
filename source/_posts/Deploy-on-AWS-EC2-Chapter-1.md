---
title: 📝 Deploy on AWS EC2 (AWS EC2部署之旅) 上篇
date: 2024-07-17 12:36:38
code_block_shrink:  false
categories: [Deploy]
tags: [AWS, EC2, Ubuntu, Nginx]
---

嗨嗨～ 記錄一下我的AWS部署之旅 💻

一開始接觸可能有些繁瑣，熟悉之後是另一個世界～🤩✨
這次部署的專案是用 Python Django 開發，並且有先 push 到 GitHub 上。

對了！開始部署之前別忘記先註冊一個 AWS 帳號喲(這邊就不特別說明了)

然後～直接進入主題！

### Step 1 進到 AWS EC2 的控制台

![AWS-1](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-1.3k7wf52yuu.webp)

常用的話也可以把 EC2 加到最愛中，就不用每次都要搜尋一次😎

---

### Step 2 新增實例 (instance)

![AWS-2](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-2.4n7lq0ysqq.webp)

選擇啟動**執行個體(Launch Instance)**，右上角可以選擇地區，我是選擇亞太地區(大阪)。

![AWS-3](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-3.5tqwymnpc1.webp)

進到啟動的頁面先給自己的 instance 一個名稱。

![AWS-4](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-4.1759xxp5oh.webp)

再來選擇一個自己習慣使用的操作系統，這邊我是選擇 Ubuntu。
👽💬 Ubuntu 是一個基於 Linux 的操作系統，目前蠻多人選擇使用的喲～

![AWS-5](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-5.8z6exki48v.webp)

網路設定要記得確認以下兩個有沒有勾選到喲！
✅ 建立安全群組 (Create security group)
✅ 允許SSH流量 (Allow SSH traffic from)
👽💬 要選擇SSH流量的原因
1. 管理需要：在設置和管理 EC2 實例時，特別是首次配置和安裝軟件時，需要使用 SSH 來遠程訪問伺服器。
2. 安全性：SSH 流量是加密的，適合安全地進行系統管理操作。


都確定 OK 後，就能點選右下角橘色框框的啟動**執行個體(Launch Instance)**

![AWS-6](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-6.9rjafaypz1.webp)

這時候會跳出一個框框，要你選擇是否要建立新的金鑰對(key pair)，這邊我選擇不使用金鑰對而繼續(Proceed without key pair)，會直接透過 SSH 連接伺服器～

![AWS-7](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-7.67xcphw070.webp)

成功建立實例後回到執行個體(Instances)頁面，就能看到剛剛新增的實例！

一開始的執`行個體狀態(Instance State)` 會是`等待中(Pending)`而`狀態檢查(Status check)`會是 `" - "`，這樣是正常的喔，給他一點時間跑再重新刷新頁面，就會變成`執行中(Running)`以及`2/2項檢查通過(2/2 checks pass)`囉！

![AWS-8](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-8.10222i308r.webp)

好啦，再來就是勾選你要連線的實例，按下上方的**連線(Connect)**。

![AWS-9](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-9.ic0dx1mnv.webp)

順利連線後會看到的畫面長上面這樣👆🏻

👽💬 可以把它想成遠端電腦的終端機，我們可以通過 SSH 或者其他遠端連接工具來進入這台虛擬機器的作業系統，進行檔案管理、程式安裝、設定修改等操作，就像你在本地電腦上操作一樣喲！

---

### Step 3 導入專案
因為是遠端電腦的終端機，所以接下來都會用指令來完成各個操作喲～
首先我們先下指令確定 Ubuntu 的系統使用的是最新的軟體資源列表。
```
$ sudo apt-get update
```
再來把需要部署的專案導入到這台遠端電腦中，我們到 GitHub 複製專案的 https 網址然後：
```
$ git clone https://github.com/chiehhhaa/BanksInfo.git
```

![AWS-10](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-10.lvmbmupdm.webp)

你可以下 ls 指令確認專案是否有導入成功～ 我是有成功喲🤩🥳
再來的操作都跟在本地電腦一樣，專案套件的 install 跟資料庫的連接～

我先建立一個虛擬環境，避免套件的相互衝突和版本問題。

1. 安裝 Python 3 內建的 venv
```
$ sudo apt install python3-venv
```
2. 建立虛擬環境
```
$ python3 -m venv myenv
```
3. 啟動虛擬環境
```
$ source myenv/bin/activate
```
這次部署的專案是使用 Python Django 架設的，所以要下載 python3-pip，方便後續 install 其他的套件～
```
$ sudo apt install python3-pip -
```
再來安裝自己專案需要使用到的各種套件 ok 囉！

🛸 p.s.關於 MySQL 資料庫在 AWS 上的串接，我會再另外寫一篇分享喲 ✍🏻

---

### Step 4 端點 (Port) 設定
好啦～在專案導入成功後，我們就可以來 run server 試試看囉。

先把Port設定讓其他電腦也可以造訪，我們可以下這個指令：
```
$ python3 manage.py runserver 0.0.0.0:8000
```
`0.0.0.0` : 伺服器會在所有可用的網路介面上監聽連線，不僅是本地主機。
`8000` 是指定的端口號，這是預設的 Django 開發伺服器端口。

再來要回到**執行個體(Instances)頁面**

![AWS-11](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-11.361go9unzt.webp)

![AWS-12](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-12.9dcuofqf3t.webp)

![AWS-13](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-13.8ojl4f2w3c.webp)

![AWS-14](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS-14.1hs3r34dtj.webp)

選擇安全性來新增傳入規則(Inbound rules)，這邊新增 HTTP 的傳入規則，這樣就能順利造訪專案的頁面囉～

---

恭喜恭喜✨✨ 我們已經完成部署啦！

下篇是記錄關於反向代理的設定方式，目前的狀況是終端機關掉網站就無法運行，所以需要設定反向代理來確保網站能夠持續運行喲。

感謝大大們看到這邊🥹 
如果有內容錯誤需要更正的也麻煩大神們提點🫡

繼續觀看🔜
