# QR Code-based-Field-Games

這是用於戶外大地闖關遊戲的程式，使用 QR Code 當做闖關點，掃描 QR Code 後會出現題目。
![](https://user-images.githubusercontent.com/4427280/32505606-422a3e96-c3a8-11e7-9876-3a90fe72466c.gif)

## 特色
1. 完全使用 Web 技術，用戶不用安裝任何軟體  
2. 除第一次載入外，可在離線環境下進行遊戲  
3. 支援遠端取得密碼與上傳資料

## 使用流程
1. 將檔案上傳至自己的伺服器或使用 [GitHub Page 的版本](https://s3131212.github.io/QRCode-based-Field-Games/)  
註：需要有 HTTPS 才能存取鏡頭，且若有需要上傳題目或許得 token 等，要注意 CORS 問題。
2. 製作初始化 QR Code  
初始化 QR Code 使用下列程式碼：  
```json
[{
	"title": "Test contest",
	"password": "abc",
	"username": "test",
	"request_token": {
		"enable": false,
		"url": ""
	},
	"upload_data": {
		"enable": true,
		"url": "https://requestb.in/blablabla"
	},
	"question": {
		"1": "Question1",
		"2": "Question2",
		"3": "Question3"
	}
}]
```  
`title` 為遊戲標題  
`username` 為用戶名稱。  
`question` 物件包含所有題目與其標題。  
`password` 為解密用的密碼，在啟動 `request_token` 的情況下為請求密碼的 token。
若有啟用 `request_token`，則會夾帶 `password` 送一個 POST 請求給指定網址，並將回傳值視為解密用的密碼。  
若有啟用 `upload_data`，則在「資訊」的地方會有上傳按鈕，按下後會把解題成果以 `data` 欄位，用戶名以 `username` 欄位 POST 給指定網址。  
註：若使用 requestbin 做為上傳網址，會因為 CORS 而無法上傳，但在 requestbin 上仍然可以看到請求。  
最後將此 JSON [轉成 QR Code](https://www.google.com.tw/search?q=qrcode+generator) 即可。  

3. 製作題目的 QR Code  
題目的 QR Code 使用下列程式碼：  
```json
[{
	"id": 1 ,
	"title":"Question1",
	"context":"1+2+3+4+5+6+7+8+9+10=? <br /> (A) 1 <br /> (B) 2 <br /> (C) 45 <br /> (D) 55",
	"answer":"D"
}]
```
註：答案為 A~D 任一個  
將此 JSON 以 AES-256 CBC Mode 用 `password` [加密](https://gotyour.pw/encrypt.html)，並將密文[轉成 QR Code](https://www.google.com.tw/search?q=qrcode+generator) 即可。  

4. 把 QR Code 貼到闖關點
5. 架設伺服器（可選）
若您希望有請求 token 功能，或上傳結果功能，則需要一個後端伺服器接收這些資料。  
就只是單純接 POST request 而已，等有時間時我再補上範例程式碼。
6. Enjoy your game!



