# 第6回 Webエンジニアリング演習 レポート
## 学籍番号（名前は書かなくてよい）
4723217
## 動作確認
- Swagger UIでのテスト結果を記載
/todos/
```
[
  {
    "id": 1,
    "title": "牛乳とパンを買う",
    "description": "牛乳は低温殺菌じゃないとだめ",
    "completed": false
  },
  {
    "id": 2,
    "title": "Pythonの勉強",
    "description": "30分勉強する",
    "completed": true
  },
  {
    "id": 3,
    "title": "30分のジョギング",
    "description": "",
    "completed": false
  },
  {
    "id": 4,
    "title": "技術書を読む",
    "description": "",
    "completed": false
  },
  {
    "id": 5,
    "title": "夕食の準備",
    "description": "カレーを作る",
    "completed": true
  }
]
```

/todos/?query=牛乳
```
[
  {
    "id": 1,
    "title": "牛乳とパンを買う",
    "description": "牛乳は低温殺菌じゃないとだめ",
    "completed": false
  }
]
```

/todos/?query=存在しない文字列
```
[]
```