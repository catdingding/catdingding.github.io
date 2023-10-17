---
title: 登入功能常見漏洞筆記
date: 2023-10-17 20:19:24
tags:
  - 資安
---

以下大多爲親身通報過的漏洞，~~通報了對方還不見得願意修~~

## 弱密碼

### 預設密碼

- 共通的預設帳號密碼（如某些 ipcam 的預設帳密是 admin/admin）
- 用戶間有相同的預設密碼（都是 1234 ，登入以後再自己改）
- 以身份證字號、學號等資訊做爲預設密碼
- 首次登入後，未強制更改預設密碼

### 未進行密碼強度檢查/錯誤的密碼設定指引

- 帳號與密碼相同
  - 如果帳號都是流水號就更好了
- 123456 之類的垃圾密碼
- 「爲了避免各位同學忘記，請大家統一用身份證末四碼當做密碼」

## sql injection

「你的 form 就是我的 sql client」\
輕則繞過登入，重則資料庫到手\
請唯一支持參數化查詢

範例

```php
$username = "admin' OR 1=1 --";
$password = "1111";
$sql = "SELECT * FROM user WHERE username='$username' AND password='$password'";
```

output:

```sql
SELECT * FROM user WHERE username='admin' OR 1=1 --' AND password='1111'
```

## 重設密碼漏洞

### 易猜測的重設密碼連結/驗證碼

- /reset-password/{username}
- /reset-password/{hash(username)}
- /reset-password/{流水號}
- 易暴力猜測
  - 無 rate limit
  - 以 IP 等方式進行 rate limit
- 答案放在 cookie 或 hidden input 裏面

## 驗證碼（CAPTCHA ）失效

### 過於簡單的驗證碼

都 2023 了，這種驗證碼有跟沒有一樣

{% asset_img image_code.png %}

### 錯誤的檢查順序

例如先檢查帳密，再檢查驗證碼正確性，並且可於錯誤訊息中分辨是何種錯誤。\
如此一來，爆破帳密時可隨意填寫驗證碼，只要發現錯誤訊息從「帳號密碼錯誤」，變成「驗證碼錯誤」，即可知已找到正確組合。

### 驗證碼答案放在網頁中

- 放 cookie
- 放某個 hidden input

### 允許以空驗證碼通過檢查

例如系統在用戶請求驗證碼圖片時，會將正確答案寫入 session ，並在送出登入請求時進行檢查。\
但在用戶從未請求驗證碼圖片時，將答案的預設值視作空字串。\
如此一來，用戶可在驗證碼留空的情況下通過驗證碼檢查。

## 密碼泄漏

### 明文/可還原的密碼儲存

- 明文儲存（DB 拿到手就行）
- 加密儲存（DB + private key 拿到手就行）
- 僅進行簡單的 hash（rainbow table 囉）

### 錯誤的請求方式或 Log 設定

Log 外泄就要完

- 用 GET 傳帳密
- 用 POST 傳帳密，但 Log 會把整包 body 存下來

## 其他

### 未確認用戶是否已登入

例如正常需於 /admin/login.php 登入後，再跳轉到 /admin/dashboard.php \
但實際上直接訪問 /admin/dashboard.php 也可正常操作\
~~再搭上被搜尋引擎收錄就更好了~~

### 未移除錯誤的登入程式

/admin/login.php 運作正常，但有漏洞的 /admin/login_old.php 僅改名未移除

### 未檢查密碼

這個很謎，但真的遇過……\
帳號打對，密碼隨便輸入就行

### 錯誤的 IP 檢查方式

例如相信用戶傳過來的 X-Forwarded-For 或 Client-IP 等 Header
