---
title: 📝Deploy on AWS EC2 (AWS EC2部署之旅) 下篇
date: 2024-07-17 14:38:41
code_block_shrink:  false
categories: [Deploy]
tags: [AWS, EC2, Ubuntu, Nginx]
---
嗨嗨～ 接續 📝Deploy on AWS EC2 (AWS EC2部署之旅) 上篇 。
這篇是記錄如何設定反向代理。
<!-- more -->

首先，先來說說什麼是反向代理？

![AWS2-01](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS2-01.73tu53fon5.webp)

可以把反向代理想像成大樓的管理員，他會接受用戶的請求，並找到相對應的伺服器並返回給用戶。用戶不用直接跟後端伺服器接觸，不僅簡化了流程也增強了安全性和效率！

這次會使用 `Nginx` 搭配 `Gunicorn`。

> 👽💬 Nginx 就是一個反向代理伺服器喲～ 而 Gunicorn ( Green Unicorn) 則是一個運行 Python Web 應用的 WSGI HTTP 伺服器！

那就開始囉！

### Step 1 設定 Gunicorn

```
$ pip install gunicorn
```

在專案中安裝 Gnuicorn 後，再來需要與專案進行連接
```
$ /home/ubuntu/your_project_name/venv/bin/gunicorn --bind unix:/home/ubuntu/your_project_name/core.sock core.wsgi:application
```
這個指令是：確保 Gunicorn 能夠正確地與你的 Django 專案連接，並通過 Unix socket ``(core.sock)`` 與 Nginx 進行通信，以提供 Web 服務。

> 👽💬 可以在指令最後加上 `-- daemon`，讓 Gunicorn 在後台運行，這樣就不會佔據你的終端機囉～

![AWS2-02](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS2-02.5j435mih6s.webp)


指令完成後，你會在你的專案中看到 `core.sock` 的檔案，這時候我們可以檢查一下 Gunicorn 是否有在後台運行。

```
$ ps aux | grep gunicorn
```

![AWS2-03](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS2-03.7egny8uwsj.webp)

當底看到這段就代表你已經順利設定好 Gunicorn 囉 🎉

---

### Step 2 設定 Nginx

```
$ sudo apt install nginx
```
再來我們需要在 /etc/nginx/sites-available/ 目錄下創建一個新的 Nginx 配置文件，例如 banksinfo：
```
$ sudo nano /etc/nginx/sites-available/banksinfo
```
會進到下面的 nano 編輯器畫面中：

![AWS2-04](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS2-04.lypha936.webp)

這邊的內容我們可以先以 HTTP 來設定，後面改設定 HTTPS 時，在執行 Certbot 命令獲取 SSL 憑證後，會自動修改你的 Nginx 配置文件，以使其包含 SSL/TLS 相關的設置。
```
server {
    listen 80;
    server_name your_domain_name;

    location / {
        proxy_pass http://unix:/path/to/your/project/core.sock;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /path/to/your/project/static/;
    }

    location /media/ {
        alias /path/to/your/project/media/;
    }
}
```
需要修改的地方為

1. `server_name your+domain_name` (你的網域)
2. `proxy_pass http://unix:/path/to/your/project/core.sock` (你的 .sock 檔案路徑)
修改完成後，我們可以確認一下檔案內容是否設定正確
```
$ sudo nginx -t
```

![AWS2-05](https://github.com/chiehhhaa/picx-images-hosting/raw/master/AWS2-05.92q0vfl6yq.webp)

這樣代表我們的 Nginx file 內容並沒有錯誤。
好啦～再來重新啟用 Niginx ，並設置 Nginx 服務自動啟動。
```
$ sudo systemctl start nginx
$ sudo systemctl enable nginx
```

---

### Step 3 設定 Certbot 來獲取 SSL/TLS 憑證
這邊使用 Let's Encrypt 以及 Certbot 來設定 SSL 憑證。

>👽💬 Let's Encrypt 是免費的、自動化的、開放的憑證頒發機構，簡單來說這個機構會提供 SSL/TLS ，讓你的網站從 HTTP變成 HTTPS。
>而 Certbot 是一個開源的工具，可以自動化 Let's Encrypt 憑證的獲取和安裝。


先來安裝 Cerbot :
```
$ sudo apt install certbot python3-certbot-nginx
```
再來要幫你的 Domain 取得 SSL 憑證：
```
$ sudo certbot --nginx -d yourdomain.com
```

登愣～ Cerbot 會自動配置 Nginx 以在 HTTP 到 HTTPS 的重定向。這將確保所有用戶訪問你的網站時都是安全的 HTTPS 連接。

最後不要忘記到 AWS 去設定 HTTPS 的安全組 ( security group )。

---

這邊要提醒一件事，假設你在完成上面的設定後，造訪你的網站時發現是502 gateway 時，不要慌張！這邊可能只是你的網頁權限沒有設定好！
```
$ sudo chmod 755 /home/ubuntu
```
將用戶權限設定成能讀取、寫入和執行的權限。


>👽💬 數字權限所代表的分別為：
4：允許讀取（Read）/ 2：允許寫入（Write）/ 1：允許執行（Execute）
7 -> 讀取＋寫入＋執行 的權限
6 -> 讀取＋寫入 的權限
5 -> 讀取＋執行 的權限

---

好啦～到這邊我們就完成了專案的部署啦 🎉🥳
希望大家都能成功部署！

老話一句，感謝大大們看到這邊🥹 
如果有內容錯誤需要更正的也麻煩大神們提點🫡
