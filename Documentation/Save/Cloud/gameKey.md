# 雲存檔模型 `gameKey`
## 版本1
`gameKey`存檔文件解密後由以下結構組成（僞代碼，非實際游戲代表）：
1. `LEB128`：游戲特定資源解鎖狀態的數量。
2. `GameKeyPayload[]`：游戲特定資源的解鎖狀態。
3. `uint8`：`Lanota`相關的收集閲讀狀態。比特{n}對應到本地存檔鍵`ReadLanota{n}`。例如`ReadLanota0`。{n}的範圍：`0 <= {n} <= 5`

而`GameKeyPayload`類型則是由以下結構組成：
1. `uint8`：鍵的字節長度。
2. `uint8[]`：鍵字串，已知可能值：
    - 曲目名字
    - 頭貼名稱（有些有數字後綴的會被整合進無數字後綴的鍵）
    - 收集的id
3. `uint8`：有效載荷的字節長度。
4. `uint8[]`：有效載荷。其中：
    - 字節0：代表載荷擁有的類型。
        - 比特0：收集閲讀數量。
        - 比特1：單曲集解鎖狀態。
        - 比特2：收集解鎖數量。
        - 比特3：解鎖曲繪狀態。
        - 比特4：頭貼解鎖狀態。
    - 字節1~5：實際有效載荷的值。大小為字節0比特為1的數量。舉`(LSB)00011001`爲例，會有3個字節代表載荷的值。其中記錄的排序類似於棧，以頭貼解鎖狀態爲例，要存取這個值的索引為3，因爲在載荷類型中未指定單曲集解鎖狀態和收集解鎖數量，并且索引0指向載荷類型，計算為`4(頭貼解鎖狀態的類型比特索引) - 2(0的數量) + 1(不算載荷類型)`。

    - 舉例值：`(LSB, 小端) 字節0：00001100 字節1：00000011 字節2：00000101`
        - 載荷類型有（字節0）：收集解鎖數量（比特2），解鎖曲繪狀態（比特3）
        - 收集解鎖數量（字節1）：3
        - 解鎖曲繪狀態的值（字節2）：9

## 版本2
在版本1上追加：
1. `bool`：是否已讀取`bassareusEgg`收集。本地鍵：`ReadbassareusEgg`。大小1字節。

## 版本3
在版本2上追加：
1. `bool`：是否已讀取`investigatewuxiang`收集。本地鍵：`ReadSideStory4Begin`。大小1字節。
1. `bool`：是否已刪除部分游玩記錄。本地鍵：`IsOldScoreClearedV390`。大小1字節。清除的曲目及難度如下：
    - `BonusTime.MegaloPaleWhite.0`: `EZ`, `HD`
    - `EnginexStartmelodymix.CrossingSound.0`: `EZ`, `HD`
    - `FULiAUTOSHOOTER.MYUKKE.0`: `EZ`, `HD`
    - `Glaciaxion.SunsetRay.0`: `HD`
    - `HumaN.SOTUI.0`: `EZ`, `HD`
    - `Orthodox.tokiwa.0`: `EZ`, `HD`
    - `PixelRebelz.Normal1zer.0`: `EZ`, `HD`
    - `PRAW.Bluewind.0`: `EZ`, `HD`
    - `Wintercube.CtymaxfeatNceS.0`: `EZ`, `HD`
    - `混乱Confusion.OnlyMyBlackScore.0`: `EZ`, `HD`

## 實例代碼
1. C#：[PhigrosLibraryCSharp GameKeyPayload](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/f588dfea20253fd8bec5395a2cdcb5efc20afaeb/PhigrosLibraryCSharp/CloudSave/GameKeyFlag.cs#L40), [PhigrosLibraryCSharp GameKey](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/master/PhigrosLibraryCSharp/CloudSave/GameKey.cs)
2. Python：[Phi-CloudAction-python](https://github.com/wms26/Phi-CloudAction-python/blob/ae4a5d951cb8a7f1f53474b71b5d9fc6576c0a90/PhiCloudAction/Structure/gameKey.py#L8)
