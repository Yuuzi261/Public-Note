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
