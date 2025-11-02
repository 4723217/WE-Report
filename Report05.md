# 第5回 Webエンジニアリング演習 レポート
## 学籍番号（名前は書かなくてよい）
4723217
## 各エンドポイントのSwagger UIでのテスト結果

### GET /
Request:
(curlコマンドでのリクエスト内容)
```
curl -X 'GET' \
  'http://127.0.0.1:8000/' \
  -H 'accept: application/json'
```
Response:
(レスポンス内容)
```
{
  "Hello": "World"
}
```


### GET / health:
Request:
(curlコマンドでのリクエスト内容)
```
curl -X 'GET' \
  'http://127.0.0.1:8000/health' \
  -H 'accept: application/json'
```
Response:
(レスポンス内容)
```
{
  "status": "healthy"
}
```

### GET / items/{item_id}:
Request:
(curlコマンドでのリクエスト内容)
```
curl -X 'GET' \
  'http://127.0.0.1:8000/items/-2' \
  -H 'accept: application/json'
```
Response:
(レスポンス内容)
```
{
  "detail": "Item ID must be positive"
}
```

