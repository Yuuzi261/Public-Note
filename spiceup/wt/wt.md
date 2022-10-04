# 美化 Windows Terminal

## STEP1. 安裝PowerShell

1. 打開Microsoft Store </br></br>
2. 搜尋"PowerShell" </br></br>
3. 找到PowerShell，點選"取得" </br></br>
![](https://i.imgur.com/NMYgb7U.png)

:::warning
:warning: **注意** </br>
這個PowerShell不是Windows內建的"Windows PowerShell"
:::

## STEP2. 將PowerShell設為預設設定檔(選)

1. 打開Windows Terminal </br></br>
2. 開啟設定 </br></br>
3. 將"預設設定檔"欄位改成"PowerShell"
![](https://i.imgur.com/H9O3czH.png) </br></br>
4. 點選"開啟JSON檔案"，找到"profiles"欄位 </br></br>
5. 將PowerShell移到最上面 </br></br>
6. 將Windows PowerShell的"hidden"屬性改成"true"，如圖
![](https://i.imgur.com/sImPQuI.png) </br></br>
7. 重啟Windows Terminal </br></br>
8. 可以看到一打開就是PowerShell，並且點選工作欄上的小箭頭之後，PowerShell在第一個
![](https://i.imgur.com/Q0PYTwz.png)

## STEP3. 安裝Oh My Posh

1. 搜尋"oh my posh"，進入官網
![](https://i.imgur.com/v4VTfSh.png)
2. 點選"Get Started"，右方欄位找到"Installation"，這裡以Windows為例，所以點選"Windows"
![](https://i.imgur.com/CvnoLuy.png)
3. 往下找到這行指令直接複製貼上到Windows Terminal裡

    ```bash
    winget install JanDeDobbeleer.OhMyPosh -s winget
    ```

4. 安裝好之後大概會像這樣，中途如果有停下來直接打"Y"同意就好
![](https://i.imgur.com/XWccLVu.png)
5. 重啟Windows Terminal
6. 輸入`oh-my-posh.exe`測試安裝有沒有成功，如果出現警告可以重新開機再輸入看看，通常會跑出一堆東西，如圖:
![](https://i.imgur.com/bJo0RXy.png)

## STEP4. 安裝建議的字體

:::info
:information_source: 這步是必要的，否則Windows Terminal可能會無法正常顯示
:::

1. 搜尋"nerd font"或是[點選這裡](https://www.nerdfonts.com/) </br></br>
2. 點選"Downloads"進入下載介面
![](https://i.imgur.com/MMBfaMg.png) </br></br>
3. 找到喜歡的字體並下載，會得到一個壓縮檔 </br></br>
4. 打開Windows的設定，搜尋"字型設定" </br></br>
5. 將解壓縮後的檔案全選並拖拉至字型設定中安裝
![](https://i.imgur.com/Xhp2BxS.png) </br></br>
6. 打開Windows Terminal設定介面 </br></br>
7. 點選"預設值" $\rightarrow$ "外觀" $\rightarrow$ "字體"，將字體更改為剛剛安裝好的字體
![](https://i.imgur.com/tmk9JOv.png)

## STEP5. 更改PROFILE檔案

1. 官網右方欄位找到"Prompt"，接下來照著官方教學，輸入`code $PROFILE`，這樣就會以vscode開啟這個檔案，如果沒有檔案就新建一個。沒有安裝vscode的話也可以先輸入`echo $PROFILE`之後到指定的路徑新增設定檔
![](https://i.imgur.com/pBdsu6J.png) </br></br>
![](https://i.imgur.com/v3x3mNK.png)
2. 官網右方欄位找到"Customize"，複製以下指令到設定檔並存檔，接著輸入`. $PROFILE`重新載入設定檔

    ```bash
    oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH/jandedobbeleer.omp.json" | Invoke-Expression
    ```

3. 如果成功的話重啟後會看到這樣的畫面:
![](https://i.imgur.com/E2T7hga.png)

## STEP6. 更換喜歡的主題

1. 官網右方欄位找到"Themes"，開始物色喜歡的主題
![](https://i.imgur.com/XoQYkuL.png) </br></br>
2. 將反白的這個部分換成想要的主題的名字
![](https://i.imgur.com/3kzqJwn.png) </br></br>
3. 輸入`. $PROFILE`重新載入設定檔之後，可以發現變成自己設定的主題了
![](https://i.imgur.com/yLlGi8f.png)

## STEP7. 更換Windows Terminal的背景 設定透明度．．．

1. 這部份很簡單，甚至不用操作JSON檔，所以就不詳細寫了，只要在Windows Terminal設定介面依照自己的喜好進行設定就可以了
![](https://i.imgur.com/8FktBkY.png)

:::info
ℹ️延伸閱讀: [提升 PowerShell 的使用體驗](https://hackmd.io/@Yuuzi/ps)
:::
