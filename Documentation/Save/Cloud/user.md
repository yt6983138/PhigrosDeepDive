# 雲存檔模型 `user`
## 版本1
`user`存檔文件解密後由以下結構組成（僞代碼，非實際游戲代表）：
1. `bool`：是否展示使用者昵稱（是否展開）。
2. `LEB128`：使用者自我介紹的字節長度。
3. `uint8[]`：使用者自我介紹字串。UTF8編碼，無BOM。
4. `LEB128`：使用者頭貼id的字節長度。
5. `uint8[]`：使用者頭貼id字串。UTF8編碼，無BOM。
6. `LEB128`：使用者背景id的字節長度。
7. `uint8[]`：使用者背景id字串。UTF8編碼，無BOM。

## 實例代碼
1. C#：[PhigrosLibraryCSharp](https://github.com/yt6983138/PhigrosLibraryCSharp/blob/f588dfea20253fd8bec5395a2cdcb5efc20afaeb/PhigrosLibraryCSharp/CloudSave/GameUserInfo.cs#L53)
2. Python：[Phi-CloudAction-python](https://github.com/wms26/Phi-CloudAction-python/blob/ae4a5d951cb8a7f1f53474b71b5d9fc6576c0a90/PhiCloudAction/Structure/user.py#L8)
