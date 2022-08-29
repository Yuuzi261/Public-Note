# 美化 Windows Terminal

## STEP1. 安裝PowerShell

1. 打開Microsoft Store
2. 搜尋"PowerShell"
3. 找到PowerShell，點選"取得"
![](1)

:::warning
:warning: **注意** <br>
這個PowerShell不是Windows內建的"Windows PowerShell"
:::

## STEP2. 將PowerShell設為預設設定檔(選)
1. 打開Windows Terminal
2. 開啟設定
3. 將"預設設定檔"欄位改成"PowerShell"
![](2)
4. 點選"開啟JSON檔案"，找到"profiles"欄位
5. 將PowerShell移到最上面
6. 將Windows PowerShell的"hidden"屬性改成"true"，如圖
![](3)
7. 重啟Windows Terminal
8. 可以看到一打開就是PowerShell，並且點選工作欄上的小箭頭之後，PowerShell在第一個
![](4)

## STEP3. 安裝Oh My Posh

1. 搜尋"oh my posh"，進入官網
![](5)
2. 點選"Get Started"，右方欄位找到"Installation"，這裡以Windows為例，所以點選"Windows"
![](6)
3. 往下找到這行指令直接複製貼上到Windows Terminal裡
```
winget install JanDeDobbeleer.OhMyPosh -s winget
```
4. 安裝好之後大概會像這樣，中途如果有停下來直接打"Y"同意就好
![](7)
5. 重啟Windows Terminal
6. 輸入以下指令測試安裝有沒有成功，如果出現警告可以重新開機再輸入看看，通常會跑出一堆東西，如圖
```
oh-my-posh.exe
```
![](8)

## STEP4. 安裝建議的字體

:::info
:information_source: 這步是必要的，否則Windows Terminal可能會無法正常顯示
:::