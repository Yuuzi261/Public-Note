# Lecture 3 卷積神經網路 (Convolutional Neural Networks, CNN)

## Image Classification

我們先假設每張輸入進模型的圖片大小是固定的，實際上現今普遍的影像辨識系統在丟入圖片之前也是會先 Rescale 成大小一樣。我們模型的目標是分類，所以將類別表示成 One-Hot Vector 的形式，即為下圖的 $\hat{y}$，假設目標是貓，貓所對應的 dimension 就會是1，而其他 dimension 為0，這個 dimension 的長度決定了一個模型可以辨識的種類多寡

![](https://i.imgur.com/UKjO0oY.png)

模型的輸出經過 $softmax$ 之後表示為 $y'$，我們會希望 $y'$ 和 $\hat{y}$ 的 cross entropy 越小越好。接下來會遇到的問題是，怎麼把一張圖片當成模型的輸入呢？我們必須先了解對電腦來說，一張影像是什麼樣的東西

![](https://i.imgur.com/hkmYscn.png)

其實對電腦來說，圖片就是一個3維的 Tensor（超過2維的矩陣我們就會稱呼他為 Tensor），一維代表圖片的長，一維代表圖片的寬，還有一維代表圖片的 channel 數目，一張彩色的圖片是由 RGB 三色所組成，所以上圖3個 channel 就是代表 RGB 這三個顏色。接下來將這個三維的 Tensor 拉直成一個巨大的向量，就可以放進 Network 去做訓練了

![](https://i.imgur.com/KBgrB5f.png)

接下來我們把這個 vector 輸入到 Fully Connected Network 裡，假設第一層有1000個 Neuron，那麼第一層就會有 $100\times100\times3\times1000=3\times10^7$ 個 weight，如此多的參數雖然可以讓模型更有彈性，但也增加了 overfitting 的風險，所以考慮到影像辨識的特性，其實並不需要全連接，接下來會講述我們對影像辨識這個問題的一些觀察
