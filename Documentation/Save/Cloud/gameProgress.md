# 雲存檔模型 `gameProgress`
## 版本1
`gameProgress`存檔文件解密後由以下結構組成（僞代碼，非實際游戲代表）：
1. `ProgressFlag`：有關於游戲進度的enum。大小為1字節。
    - 比特0：是否爲第一次執行游戲。
    - 比特1：是否完成“過去的章節”。
    - 比特2：是否已展示“收集”的提示。
    - 比特3：是否已展示解鎖`IN`難度的提示。
2. `LEB128`：完成進度的字節長度。
3. `uint8[]`：完成進度的字串。UTF8編碼，無BOM。已知有使用的值跟效果：
    - `3.0`：`AboutUs`場景開始播放時將設爲這個值。
    - <空字串符>：如果*不*為這個值，玩家可以選擇`Legacy`難度的曲目。
4. `LEB128`：歌曲更新的數量<sup>[待確認]</sup>。
5. `int16`：挑戰模式的等級。百位數為等級的顔色類型，個位及十位為等級。
    - 例如：`420`為金（百位為4）20（等級）
    - 例如：`144`為綠44
    - 例如：`15`為白15（不可透過正常途徑獲得）
6. `Currency`：玩家所持有的貨幣（Data）數量。
7. `DifficultyUnlockFlag`：`Spasmodic`的難度解鎖狀態enum。大小為1字節。
    - 比特0：難度`EZ`已解鎖。
    - 比特1：難度`HD`已解鎖。
    - 比特2：難度`IN`已解鎖。
    - 比特3：難度`AT`已解鎖。
8. `DifficultyUnlockFlag`：`Igallta`的難度解鎖狀態enum。定義見上一字段。
9. `DifficultyUnlockFlag`：`Rrhar'il`的難度解鎖狀態enum。定義見上一字段。
10. `SongRecordFlag`：各種曲目的解鎖狀態enum。大小為1字節。
    - 比特0：玩家是否已獲得`You are the Miserable`的`S`等級。本地存檔鍵：`YouaretheMiserableINSGrade`
    - 比特1：玩家是否已獲得`Stasis`的`S`等級。本地存檔鍵：`StasisINSGrade`
    - 比特2：玩家是否已獲得`Shadow`的`S`等級。本地存檔鍵：`ShadowINSGrade`
    - 比特3：玩家是否已獲得`心之所向`的`S`等級。本地存檔鍵：`心之所向INSGrade`
    - 比特4：玩家是否已獲得`Inferior`的`S`等級。本地存檔鍵：`inferiorINSGrade`
    - 比特5：玩家是否已獲得`DESTRUCTION 3,2,1`的`S`等級。本地存檔鍵：`DESTRUCTION321INSGrade`
    - 比特6：玩家是否已獲得`Distorted Fate`的`S`等級。本地存檔鍵：`DistortedFateINSGrade`

而`Currency`類型則是由以下結構組成：
1. `LEB128`：玩家持有的`KB`量。
2. `LEB128`：玩家持有的`MB`量。
3. `LEB128`：玩家持有的`GB`量。
4. `LEB128`：玩家持有的`TB`量。
5. `LEB128`：玩家持有的`PB`量。

## 版本2
在版本1上追加：
1. `RandomVersionFlag`：`Random`曲目的版本解鎖狀態enum。大小為1字節。
    - 比特0：版本`R`已解鎖。
    - 比特1：版本`A`已解鎖。
    - 比特2：版本`N`已解鎖。
    - 比特3：版本`D`已解鎖。
    - 比特4：版本`O`已解鎖。
    - 比特5：版本`M`已解鎖。

## 版本3
在版本2上追加：
1. `Chapter8UnlockFlag`：第八章的解鎖狀態enum。大小為1字節。
    - 比特0：第一階段解鎖。
    - 比特1：第二階段解鎖。
2. `Chapter8UnlockFlag`：第八章的曲目解鎖狀態enum。大小為1字節。定義見版本1。
## 版本4
在版本3上追加：
1. `TakumiUnlockFlag`：`Takumi`章節的曲目解鎖狀態enum。大小為1字節。
    - 比特0：玩家是否已獲得`Cuvism`的`S`等級。本地存檔鍵：`CuvismINSGrade`
    - 比特1：玩家是否已獲得`iL-Artifact`的`S`等級。本地存檔鍵：`iLArtifactINSGrade`
    - 比特2：玩家是否已獲得`a truth seeker -Communication with Utopia will be lost-`的`S`等級。本地存檔鍵：`atruthseekerCommunicationwithUtopiawillbelostINSGrade`

## 實例代碼
1. C#：[PhigrosLibraryCSharp](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/f588dfea20253fd8bec5395a2cdcb5efc20afaeb/PhigrosLibraryCSharp/CloudSave/GameProgress.cs#L90)
2. Python：[Phi-CloudAction-python](https://github.com/wms26/Phi-CloudAction-python/blob/ae4a5d951cb8a7f1f53474b71b5d9fc6576c0a90/PhiCloudAction/Structure/gameProgress.py#L8)
