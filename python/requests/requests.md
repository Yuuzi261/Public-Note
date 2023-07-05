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
