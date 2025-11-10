# 第7回 Webエンジニアリング演習 レポート
## 学籍番号（名前は書かなくてよい）
（4723217）
## PUTリクエスト検証用のモデルとAPI部分のコード
```
{
  "title": "牛乳はあまり好きではない",
  "description": "string",
  "completed": true
}
```
```
@app.put("/todos/{todo_id}", response_model=TodoItem)
def update_todo(todo_id: int, req: TodoItemUpdateScheme):
    for i, todo in enumerate(todos):
        if todo.id == todo_id:
            todos[i] = TodoItem(
                id=todo_id,
                title=req.title if req.title is not None else todo.title,
                description=req.description if req.description is not None else todo.description,
                completed=req.completed if req.completed is not None else todo.completed
            )
            return todos[i]

    raise HTTPException(status_code=404, detail=f"ID {todo_id} のTODOが見つかりません")
```

## 動作確認結果
- Swagger UIでのPUTエンドポイントテスト結果
- 正常系：存在するタスクの更新
```
{
  "id": 1,
  "title": "牛乳はあまり好きではない",
  "description": "string",
  "completed": true
}
```
- 異常系：存在しないタスクIDでのエラー確認
```
{
  "detail": "ID 999 のTODOが見つかりません"
}
```