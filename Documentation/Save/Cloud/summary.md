# 雲存檔模型 `summary`
請注意，此模型不存於存檔zip裏面，而是請求`/1.1/classes/_GameSave`回復的json路徑`results[<index>].summary`，並經過base64解碼而得到。

注意：不要采信`summary`的數據，如果需要，請從存檔zip裏面的存檔文件獲取/進行計算。
## 版本1
`summary`由以下結構組成（僞代碼，非實際游戲代表）：
1. `uint8`：存檔版本。
2. `int16`：挑戰模式的等級，見[gameProgress - 挑戰模式的等級](gameProgress.md)。
3. `float32`：玩家RKS。
4. `LEB128`：游戲版本。
5. `LEB128`：玩家頭貼id的字節長度。
6. `uint8[]`：玩家頭貼id的字串。UTF8編碼，無BOM。
7. `PlayCountSummary`：所有`EZ`難度的譜面游玩概要。
8. `PlayCountSummary`：所有`HD`難度的譜面游玩概要。
9. `PlayCountSummary`：所有`IN`難度的譜面游玩概要。
10. `PlayCountSummary`：所有`AT`難度的譜面游玩概要。

而`PlayCountSummary`類型則是由以下結構組成：
1. `int16`：玩家游玩過的數量。
2. `int16`：玩家達成全連的數量。
3. `int16`：玩家達成全部完美的數量。

## 實例代碼
1. C#：[PhigrosLibraryCSharp](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/f588dfea20253fd8bec5395a2cdcb5efc20afaeb/PhigrosLibraryCSharp/CloudSave/Summary.cs#L135)
2. Python：[Phi-CloudAction-python](https://github.com/wms26/Phi-CloudAction-python/blob/ae4a5d951cb8a7f1f53474b71b5d9fc6576c0a90/PhiCloudAction/Structure/summary.py#L4)
