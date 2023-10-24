---
title: 使用 wayback cdx 取得頂級域名下的 domain 清單
date: 2023-10-24 00:03:07
tags: domain
  爬蟲
---

https://github.com/internetarchive/wayback/tree/master/wayback-cdx-server

透過查詢 wayback 的索引，取得 domain 清單
範例如下：

```
https://web.archive.org/cdx/search/cdx?url=*.tw/&output=txt&fl=original&collapse=urlkey&limit=10&filter=!original:https?://[^/]%2B/.%2B
```

當然這樣抓一定不全，但是很快~~而且不用錢~~\
真要更全，可能再把抓到的 domain 送到 shodan 查，或是一個一個 [knock](https://github.com/guelfoweb/knock) 吧

## 參數說明

- url：要搜尋的 url 格式，此處爲任意 .tw 與其 subhost
- output：輸出格式
- fl：輸出的欄位與其順序
- collapse：將欄位值相同的記錄合併，因此處目的爲取得 domain 清單，故只要 urlkey 相同就合併
- limit：輸出的筆數上限，硬性上限爲 150000，可透過分頁或是 resumeKey 取得更多結果
- filter：可以欄位值過濾資料，加上!是排除，支援 regex，注意做 url encoding

## 抓取數量

2023-10-24 共抓到了 11 萬筆左右（未排除已停止運作的網站或已過期 domain）

## 坑

url 參數用 \*.tw 時，無法取得 .com.tw, .edu.tw 等二級域名底下的 domain，需用 \*.com.tw, \*.edu.tw 才能取得。

目前使用 [TWNIC 保留字](https://www.twnic.tw/dnservice_announce_restore.php) 中的清單來一個一個跑。
