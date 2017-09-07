# 什麼是 CORS

因為 2017 HelloJS 結束前很多人反映有遇到 CORS 問題，所以藉此由這個專案跟其他幾個相關專案講解清楚這個到底是什麼。

[錄影](https://www.youtube.com/watch?v=9XRX8uAYgKA)

## 相關資料

[MDN 上的介紹](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/Access_control_CORS)

（我以為 MSDN 上也該有類似資料，如果有看到其他瀏覽器的開發者文件有寫到這個部分，拜託幫我 PR 補充）

## 重點

- 一個 HTTP 的 HEADER 
- CORS 是瀏覽器實作的功能
- 用來防止自己的 API 被別人存取

## 處理方式

既然這是瀏覽器的功能，當然可以強制把瀏覽器這個功能關掉：

Windows :
```
taskkill /F /IM chrome.exe --disable-web-security --user-data-dir
"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --disable-web-security --user-data-dir
```

Linux: 
```
chrome --disable-web-security --user-data-dir
chrome.exe --disable-web-security --user-data-dir
```

(出處：[StackOverflow](https://stackoverflow.com/questions/3102819/disable-same-origin-policy-in-chrome))

不過這頗弱的，開你的網站還要把瀏覽器安全性設定關閉。

或許你可以編譯一個沒有 CORS 的瀏覽器，這樣省下了下參數的功夫，不過還是不太好用，瀏覽網站還要用特定瀏覽器...

## 正經地迴避方式

不過既然 CORS 是瀏覽器的功能，不要用瀏覽器就好啦。

### 直接在 Server 中把資料挖回來。

既然 CORS 是瀏覽器的功能，在 Server 抓資料就沒問題啦！

可以透過 下面的 Sample Project 將示範透過 Python 的 Request 套件把 http 的資料抓回來。

Sample Project: [cors-demo-python-site](https://github.com/dd-han/cors-demo-python-site)

### 架設反向代理

不過透過 Python 的 Request 你可以發現速度不是很理想，這裡有個專業的反向代理 nginx

它的速度可以比 Python 中發 Request 更快一點(畢竟是 C 寫的，比直譯式語言快當然合理。)

Sample Config: [cors-demo-nginx-config-basic](https://github.com/dd-han/cors-demo-nginx-config/blob/master/reverse-proxy-basic.site)

而 nginx 除了可以把整台 Server 變得像是別人的 Server 外，也可已設定一些不同的路徑把不同了路徑導向不同主機。

下面的 Sample 示範將 `/` 導向 localhost:5000 這台 Python Server，而 `/api/` 導向 `https://harekaze.moe/`

Sample Config: [cors-demo-nginx-config-advanced](https://github.com/dd-han/cors-demo-nginx-config/blob/master/reverse-proxy-advanced.site)


