# 雲存檔模型 `gameRecord`
## 版本1
`gameRecord`存檔文件解密後由以下結構組成（僞代碼，非實際游戲代表）：
1. `LEB128`：分數資料的數量
2. `ScoreGroup[]`：所有分數資料。

而 `ScoreGroup`類型則是由以下結構組成：
1. `LEB128`：曲目id的字節長度。
2. `uint8[]`：曲目id的字串内容。UTF8編碼，無BOM。
3. `uint8`：此`ScoreGroup`的資料長度（字節數量，不包含本長度字段）
4. `DifficultyExistenceFlag`：決定是否有指定難度記錄的enum。大小爲一字節。0~3比特分別代表`EZ`，`HD`，`IN`，`AT`，`Legacy`難度。
5. `FullComboFlag`：決定指定難度是否全連的enum。大小爲一字節。0~4比特分別代表`EZ`，`HD`，`IN`，`AT`，`Legacy`難度。
6. `RawScore[]`：游玩該曲目的記錄，數量為`DifficultyExistenceFlag`其中比特為1的數量。舉`(LSB)00001101`爲例，即玩過`EZ`，`IN`，`AT`，代表會有三個項目。其中記錄的排序類似於棧，如果前一難度不存在，則此記錄會“掉回”前一難度的位置。以上面提及的flag爲例，`AT`記錄的索引就會是`2`而不是`3`，因爲`IN`難度不存在。

`RawScore`的結構包含兩個字段：
1. `int32`：分數，0~1000000。
2. `float32`：準度，0\~100。注意：此字段并非以幾率的0~1表示。

## 實例代碼
1. C#：[PhigrosLibraryCSharp](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/f588dfea20253fd8bec5395a2cdcb5efc20afaeb/PhigrosLibraryCSharp/CloudSave/GameRecord.cs#L111)
2. Python：[Phi-CloudAction-python](https://github.com/wms26/Phi-CloudAction-python/blob/ae4a5d951cb8a7f1f53474b71b5d9fc6576c0a90/PhiCloudAction/Structure/DataType.py#L453)
