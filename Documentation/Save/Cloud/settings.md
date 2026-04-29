# 雲存檔模型 `settings`
## 版本1
`settings`存檔文件解密後由以下結構組成（僞代碼，非實際游戲代表）：
1. `GamePlayFlag`：游玩譜面時的設定enum。
    - 比特0：多壓指示器是否開啓。
    - 比特1：全連/全部完美指示器是否開啓。
    - 比特2：打擊音效是否開啓。
    - 比特3：低解析度模式是否開啓。
2. `LEB128`：裝置名稱的字節長度。
3. `uint8[]`：裝置名稱字串。UTF8編碼，無BOM。
4. `float32`：游玩譜面時的背景亮度。0~1。
5. `float32`：游玩譜面時的背景音樂大小。0~1。
6. `float32`：音效音量大小。0~1。
7. `float32`：游玩譜面時的打擊音效大小。0~1。
8. `float32`：游玩譜面時的偏移大小，單位為秒。
9. `float32`：游玩譜面時的note大小。

## 實例代碼
1. C#：[PhigrosLibraryCSharp](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/f588dfea20253fd8bec5395a2cdcb5efc20afaeb/PhigrosLibraryCSharp/CloudSave/GameSettings.cs#L114)
2. Python：[Phi-CloudAction-python](https://github.com/wms26/Phi-CloudAction-python/blob/ae4a5d951cb8a7f1f53474b71b5d9fc6576c0a90/PhiCloudAction/Structure/settings.py#L8)
