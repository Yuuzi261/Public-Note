# 提升PowerShell的使用體驗

## 為資料夾和檔案添加圖標

### STEP1. 進入[PowerShell Gallery](https://www.powershellgallery.com/)搜尋icon

![](https://i.imgur.com/GC5UI7H.png)

### STEP2. 點進去Terminal-Icons

畫面大概會像這樣，接下來就直接按旁邊藍色的按鈕複製指令</br></br>
![](https://i.imgur.com/r0Os0Nv.png)

### STEP3. 打開PowerShell並貼上指令

![](https://i.imgur.com/Zq22l4G.png)</br></br>
這邊會要你確認是否信任這個庫，中間可能會問很多次，怕麻煩的話輸入A就可以了

### STEP4. 修改PowerShell設定檔

安裝好後你會發現完全沒有變化，這時就要編輯一下PowerShell的設定檔，在PowerShell中輸入`code $PEOFILE`即可用vscode編輯設定檔

:::info
ℹ️ 如果沒有改過設定檔的話請看我上一篇文章: [美化 Windows Terminal](https://hackmd.io/@Yuuzi/wt)
:::

接著在設定檔裡面添加一行:

```PowerShell
Import-Module -Name Terminal-Icons
```

存檔之後輸入`. $PROFILE`或是重啟PowerShell即可重新載入設定檔</br></br>

**比較一下改造前後的差異:**</br></br>
改造前:
![](https://i.imgur.com/9enUVwk.png)</br></br>
改造後:
![](https://i.imgur.com/YWXEuLP.png)</br></br>

## 自動完成指令

### STEP1. 進入[PowerShell Gallery](https://www.powershellgallery.com/)搜尋PSReadLine

跟上面一樣按藍色按鈕複製，但先不要直接貼上PowerShell執行</br></br>
![](https://i.imgur.com/wpHX2kG.png)

### STEP2. 在PowerShell中輸入指令

在剛剛複製的指令後面指定版本:

```PowerShell
Install-Module -Name PSReadLine -RequiredVersion <version>
```

比如想要下載2.2.2版本的話就打上`Install-Module -Name PSReadLine -RequiredVersion 2.2.2`

:::success
👍 雖然我不知道直接下載最新版會有什麼後果，但就直接找一個比較多人下載的版本吧！
:::

接著執行就可以了:</br></br>
![](https://i.imgur.com/Z88vHJd.png)

### STEP3. 修改PowerShell設定檔

和上面一樣，輸入`code $PROFILE`修改設定檔</br></br>
首先這行一定要加，不然會和上面一樣什麼事都不會發生:

```PowerShell
Import-Module -Name PSReadLine
```

顯示歷史指令:

```PowerShell
Set-PSReadLineOption -PredictionSource History
```

![](https://i.imgur.com/z0krLME.png)</br></br>

列表顯示:

```PowerShell
Set-PSReadLineOption -PredictionViewStyle ListView
```

![](https://i.imgur.com/HzCbAhD.png)

:::success
👍不過因為常常不小心打錯指令所以我沒有打開這個功能，不想看到這些黑歷史
:::

Tab選單:

```PowerShell
Set-PSReadlineKeyHandler -Chord Tab -Function MenuComplete
```

![](https://i.imgur.com/JM5urYb.png)

## 自動完成Git指令

你可能會跟我一樣，安裝好上面這些新功能興高采烈的想要開始使用了，結果發現git指令怎麼按tab就是無法自動完成，所以我們要再安裝一個Module: **posh-git**

### STEP1. 安裝posh-git

老樣子，按藍色按鈕複製，貼上PowerShell執行</br></br>
![](https://i.imgur.com/zZ2QEAl.png)</br></br>
![](https://i.imgur.com/pegVwBv.png)

### STEP2. 修改PowerShell設定檔

在設定檔中加入這段:

```PowerShell
Import-Module -Name posh-git
```

重啟PowerShell之後就可以用囉:</br></br>
![](https://i.imgur.com/D8dBB5u.png)
