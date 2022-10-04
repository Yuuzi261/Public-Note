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

```powershell
Import-Module -Name Terminal-Icons
```

存檔之後輸入`. $PROFILE`或是重啟PowerShell即可重新載入設定檔</br></br>

**比較一下改造前後的差異:**</br></br>
改造前:
![](https://i.imgur.com/9enUVwk.png)</br></br>
改造後:
![](https://i.imgur.com/YWXEuLP.png)</br></br>
