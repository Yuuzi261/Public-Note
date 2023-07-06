{%hackmd BJrTq20hE %}

# Requests

## 安裝Requests

### STEP1. 在終端機利用pip安裝

在終端機上打上指令

```shell
pip install requests
```

安裝完成後通常會是這樣的畫面：
![](https://i.imgur.com/54embYz.png)

### STEP2. 檢查是否安裝成功

在終端機上打上指令

```shell
python
```

接下來將requests模組import進來

```py
import requests
```

如果沒有出現任何報錯基本上就是成功了，我們還可以試著打印看看安裝的requests版本

```py
print(requests.__version__)
```

大致會出現的結果：（若安裝時沒有指定版本，打印出來的版本可能會跟我不一樣）
![](https://i.imgur.com/u8pnStZ.png)

## 發送GET請求

我們可以requests模組來向特定網站發送請求，首先先從發送GET請求開始吧！

```py
import requests as req      # requests名字太長了，以req縮寫方便使用

# url可以填上任何你想請求的網址，這裡我以我的網站為例
url = 'https://goldorange261.github.io/Note-web/'
r = req.get(url)            # 向網站發送GET請求，並將回應儲存在r這個變數中
print(r)
```

沒有意外的話執行結果如下（200就是請求成功的回應碼）：
![](https://i.imgur.com/s5rOS6b.png)

如果想知道網站到底回應了我們什麼的話，可以成這樣

```py
print(r.text)
```

可以發現是回應了一個HTML檔案：
![](https://i.imgur.com/3WGdD43.png)

如果我們想知道回應的表頭（Response Header）的話可以這樣寫

```py
print(r.headers)
```

打印出來的結果如下：
![](https://i.imgur.com/YYp7m3y.png)

這邊注意一下**Content-Type**這欄，檔案類型為**text/html**，跟我們剛剛打印text所發現的事一樣

## 下載圖片

將要下載的圖片網址複製下來（這裡以[我網站](https://goldorange261.github.io/Note-web/)上的圖片為例）

```py
import requests as req

url = 'https://goldorange261.github.io/Note-web/index/welcome.png'
r = req.get(url)
print(r.text)
```

如果像之前一樣直接打印出text的話就會出現一堆亂碼，這是因為圖片檔這種二進制檔案以文字的形式去讀取造成的，因此我們要稍微修改一下程式碼

```py
print(r.content)
```

打印出來就不會是亂碼了：
![](https://i.imgur.com/B6D6Oh7.png)

:::info
ℹ️諸如圖片、影片、PDF...這種檔案，我們都可以使用`r.content`
:::

不過這只是打印出來而已，並沒有儲存下來，所以要再補上這2行

```py
# 因為要寫入2進制檔案所以模式要使用wb（write byte）
with open('Nacho.png', mode='wb') as file:
    file.write(r.content)
```

可以看到在當前路徑下確實將圖片給儲存下來了：
![](https://i.imgur.com/ewI8iOJ.png)

## 測試請求

:::info
ℹ️可以用[httbin](https://httpbin.org/)這個網站來測試自己發出去的請求
:::

先來測試GET吧！

```py
url = 'https://httpbin.org/get'
r = req.get(url)
print(r.text)
```

可以看到它回傳給我們的東西就是我們發送出去的請求：
![](https://i.imgur.com/eEVebKv.png)

這樣就可以檢查我們到底發送了什麼東西給伺服器，**args**就是附帶的參數，因為我們剛剛沒有附帶參數，所以就是空的。**header**都是python自動幫我們附上的。**origin**就是我發送請求的這台電腦現在的IP，**url**就是發送到哪個網址

## 網址附帶參數

一般在瀏覽網頁時你可能會看到網址後面有個問號，舉個例子：

```html
https://www.youtube.com/watch?v=GyhLj-YdJW4
```

:::danger

▶️**YouTube**

[![](https://img.youtube.com/vi/GyhLj-YdJW4/maxresdefault.jpg)](https://www.youtube.com/watch?v=GyhLj-YdJW4)

キャットラビング / Nachoneko (cover)

:::

這個YouTube影片的網址後面就有一個參數**v**，而這個參數所儲存的資料就是這個影片的ID

:::info
ℹ️如果有多個參數的話在網址中會以'&'隔開
:::

如果我們要發送這種有附帶參數的網址，可以這樣做（這邊以[httbin](https://httpbin.org/)來做測試）：

```py
url = 'https://httpbin.org/get'
params = {
    'id' : 'NaCHokAwaiI',
    'page' : '1011'
}
r = req.get(url, params=params)
print(r.text)
```

執行結果如下：
![](https://i.imgur.com/Vabz5iy.png)

因為這次我們有傳輸參數所以**args**就不是空白而是剛剛傳入的參數，再觀察一下**url**的部分：`"url": "https://httpbin.org/get?id=NaCHokAwaiI&page=1011"`，問號後面確實有我們附帶的參數

## 發送POST請求

直接將剛剛的程式碼稍加修改並觀察（get全部改成post）

```py
import requests as req

url = 'https://httpbin.org/post'
params = {
    'id' : 'NaCHokAwaiI',
    'page' : '1011'
}
r = req.post(url, params=params)
print(r.text)
```

![](https://i.imgur.com/6vi0mb1.png)

可以發現跟GET比起來，POST多了**data**、**files**、**form**和**json**，這是因為POST主要的功能是要上傳資料到伺服器。接下來我們就試著上傳資料到伺服器吧！

### form欄位

```py
import requests as req

url = 'https://httpbin.org/post'
params = {
    'id' : 'NaCHokAwaiI',
    'page' : '1011'
}
data = {
    'name' : 'Nacho',
    'bd' : '10/11'
}
r = req.post(url, params=params, data=data)
print(r.text)
```

執行結果如下，由此可知我們的資料是被用表單的的形式上傳到伺服器的

![](https://i.imgur.com/DmrExrb.png)

### json/data欄位

如果想改用json的格式上傳到伺服器的話，就將**data**改成**json**即可

```py
r = req.post(url, params=params, json=data)
```

這樣一來我們的data就會出現在**json**那裡了，**data**欄位也不為空了

![](https://i.imgur.com/LPzaaH7.png)

:::info
ℹ️**data和json欄位的區別**

- json欄位是指請求中的JSON格式的數據，它會被自動解析為一個Python字典。
- data欄位是指請求中的原始數據，它是一個字符串，不會被解析。
- 如果請求的Content-Type是application/json，那麼json欄位和data欄位的值應該是一致的，只是格式不同。
- 如果請求的Content-Type不是application/json，那麼json欄位可能會是None，因為無法解析非JSON格式的數據。

:::

### files欄位

**files**跟我們要上傳的檔案有關，這裡以我們先前下載下來的圖片為例

```py
# 以二進制檔案的形式讀取Nacho.png並放入字典中
with open('Nacho.png', mode='rb') as file:
    image = { 'Nacho_image' : file.read() }

r = req.post(url, files=image)

# 因為回應太長了所以寫到log裡面看才不會被洗掉
with open('output.log', mode='w') as file:
    file.write(r.text)
```

圖片確實是在files裡面：

![](https://i.imgur.com/4ZzJ9Uw.png)

### 改變Resquest Header

注意一下以前我們所發送的請求，在**User-Agent**是不是都長這樣：`"User-Agent": "python-requests/x.xx.x"`，基本上這樣就是很明白的在告訴伺服器我是爬蟲請擋掉我，因此我們要偽造這個**User-Agent**，讓它看起來像是由瀏覽器所發送的

首先打開瀏覽器按下**F12**並選到**網路（Network）**這個頁面裡面，然後重新整理網頁，找到**User-Agent**，最後複製下來

![](https://i.imgur.com/DtV4xdR.png)

創建一個字典header，裡面有一個User-Agent的key，將剛剛複製下來的字串作為它的值

```py
header = {
    'User-Agent' : 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.67'
}
r = req.post(url, headers=header)
print(r.text)
```

這樣**User-Agent**就成功偽造了：

![](https://i.imgur.com/P4vGOmk.png)

## 基本驗證

在[httbin](https://httpbin.org/)也可以讓我們測試基本驗證，如下圖：

![](https://i.imgur.com/X1xQESK.png)

用法也非常簡單，可以自由設定使用者名稱和密碼：`/basic-auth/{user}/{passwd}`

我就簡單地設定了一組：

![](https://i.imgur.com/WbDs3EL.png)

可以看到進入這個網頁之後會要我們輸入使用者名稱和密碼，如果輸入的使用者名稱或是密碼不正確，就會要求我們重新輸入直到輸入正確為止，認證成功後可以看到這個畫面

![](https://i.imgur.com/9fZ778D.png)

我們一樣也可以使用Python幫我們做基本認證的登入

```py
url = 'https://httpbin.org/basic-auth/Nacho/1011'
r = req.get(url, auth=('Nacho', '1011'))
print(r.text)
```

這樣就認證成功了：

![](https://i.imgur.com/HIgEHTV.png)

:::warning
❓那如果沒有驗證成功/沒有驗證呢？

直接GET試試看：

```py
r = req.get(url)
print(r)
```

得到了401（Unauthorized）的回應：

![](https://i.imgur.com/PMVL3ma.png)

:::

## 設定Timeout

我們可以將設定的請求設定**Timeout**，如果伺服器處理太久，就會報錯，這裡使用[httbin](https://httpbin.org/)中的**delay來模擬延遲很久的回應**

```py
# delay/{延遲秒數}
url = 'https://httpbin.org/delay/5'

# 如果報錯就輸出TIME OUT!!，否則輸出r 
try:
    r = req.get(url, timeout=3)
    print(r)
except:
    print('TIME OUT!!')
```

因為伺服器過了5秒才回應，但我們只能容忍3秒，於是就輸出了**TIME OUT!!**：

![](https://i.imgur.com/7WQOr63.png)

那如果伺服器只花了2秒就回應的話：

```py
url = 'https://httpbin.org/delay/2'

try:
    r = req.get(url, timeout=3)
    print(r)
except:
    print('TIME OUT!!')
```

這樣就OK了：

![](https://i.imgur.com/cdJMAlA.png)
