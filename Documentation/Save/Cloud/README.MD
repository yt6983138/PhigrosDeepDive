# 雲存檔
Phigros會讀取或寫入的雲存檔主要由以下部分組成：
1. [summary](summary.md)：儲存了本次存檔的大致記錄，例如RKS，游玩數量，存檔和游戲版本等等。
    - 注意：`summary`中的數據可信度不高，像RKS這類建議從存檔zip中讀取和計算，因爲這些數據會隨著定數更新而變動。
2. 存檔zip：儲存了玩家的全部游戲記錄，例如成績，解鎖進度，收集解鎖程度等等。
    - 模型：[gameKey](gameKey.md)，[gameProgress](gameProgress.md)，[gameRecord](gameRecord.md)，[settings](settings.md)，[user](user.md)
    - 注意：請求下來的zip本身并不加密，但是裏面的檔案會加密。結構：
        - 首字節：存檔文件版本。
        - 其餘字節：加密的存檔文件内容。加密方法：`AES-CBC-PKCS7`，密鑰：`<redacted>`，IV：`<redacted>`

## API 終點
服務器網域：
1. 國際服：`https://kviehlel.cloud.ap-sg.tapapis.com`
2. 國服：`https://rak3ffdi.cloud.tds1.tapapis.cn`

Http客戶端需要的頭：
1. `X-LC-Id`：國際服：`<redacted>` 國服：`<redacted>`
1. `X-LC-Key`：國際服：`<redacted>` 國服：`<redacted>`
1. `User-Agent`：`LeanCloud-CSharp-SDK/1.0.3`
1. `Accept`： `application/json`
1. `X-LC-Session`：使用者的Phigros session token。

### GET /1.1/users/me
請求使用者的基本資訊。Phigros内顯示的昵稱在這裏獲得。

請求額外需要的頭與主體：無。

範例回復：
```jsonc
{
	"ACL": { "*": { "write": true, "read": true } },
	"authData": {
		"taptap": {
			"access_token": "<redacted>",
			"avatar": "https://<redacted>",
			"kid": "<redacted>",
			"mac_algorithm": "hmac-sha-1",
			"mac_key": "<redacted>",
			"name": "EXAMPLE TAPTAP ACCOUNT NAME", // TapTap 賬號名稱
			"openid": "<redacted>",
			"token_type": "mac",
			"unionid": "<redacted>"
		}
	},
	"avatar": "https://<redacted>",
	"createdAt": "2026-01-01T00:00:00.000Z",
	"emailVerified": false,
	"mobilePhoneVerified": false,
	"nickname": "EXAMPLE INGAME NICKNAME", // Phigros内顯示的玩家昵稱
	"objectId": "<redacted>",
	"sessionToken": "<redacted>", // Phigros session token
	"shortId": "mfswkj",
	"updatedAt": "2026-01-01T00:00:00.000Z",
	"username": "<redacted>"
}
```

### GET /1.1/classes/_GameSave
請求存檔資訊。

請求額外需要的頭與主體：無。

範例回復：
```jsonc
{
	"results": [
		{
			"createdAt": "2023-04-29T13:16:47.811Z",
			"gameFile": {
				"__type": "File",
				"bucket": "<redacted>",
				"createdAt": "2025-06-08T03:57:41.963Z",
				"key": "gamesaves/<redacted>/.save",
				"metaData": { "_checksum": "45ea540bcbeb9fe7112261cdb97e3e71", "prefix": "gamesaves", "size": 15072 },
				"mime_type": "application/octet-stream",
				"name": ".save",
				"objectId": "68450a358f79e<redacted>",
				"provider": "qiniu",
				"updatedAt": "2025-06-08T03:57:41.963Z",
				"url": "https://<redacted>/gamesaves/<redacted>/.save" // 存檔zip請求位置，發送GET以下載
			},
			"modifiedAt": { "__type": "Date", "iso": "2025-06-08T03:57:42.137Z" },
			"name": "save",
			"objectId": "644d18bff08b<redacted>",
			"summary": "BrwBByV6QX8N44KC44G644KC44G6MQcAAQAAAGYAGAAAAL0AQAAQABYABAAAAA==",
			"updatedAt": "2025-06-08T03:57:43.107Z",
			"user": { "__type": "Pointer", "className": "_User", "objectId": "<redacted>" }
		},
		{
			"createdAt": "2023-07-24T12:38:07.543Z",
            // ...
			"modifiedAt": { "__type": "Date", "iso": "2023-07-24T12:38:04.708Z" },
            // ...
			"updatedAt": "2023-07-24T12:38:07.543Z"
            // ...
		},
		{
			"createdAt": "2023-10-27T14:31:06.941Z",
			// ...
			"modifiedAt": { "__type": "Date", "iso": "2023-10-27T14:31:05.131Z" },
            // ...
			"updatedAt": "2023-10-27T14:31:06.941Z"
            // ...
		}
        // ...
	]
}
```
可觀察每一項目的`updatedAt`或`modifiedAt`時間戳字段，存檔的時間排序如下：
- [0]：最新。
- [1]：倒數第1舊。
- [2]：倒數第2舊。

以此類推。
