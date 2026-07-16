# DiuLa! 失物比對（前端）

失物招領比對的網頁介面，發布在 GitHub Pages。
填分類、遺失日期、地點 → 打後端 `/match` → 回排序後的候選拾獲物。

**網址：** https://saamiin.github.io/diula-web/

## 這個 repo 只有前端

後端（Flask + Firestore 比對引擎）跟 Firebase 金鑰都在另一個 **private** repo
`saamiin/Diula`，部署在 Render。金鑰不會出現在這裡，也不會進到瀏覽器。

```
瀏覽器 (GitHub Pages, public)
   └─ fetch → Render 上的 Flask API (private code, 金鑰在 Secret File)
                └─ Firestore
```

## 本機開發

`index.html` 會自己判斷要打哪個後端，**不用改網址**：

| 從哪裡開 | 打的 API |
|---|---|
| `file://` 或 `localhost` | `http://localhost:5001`（本機 Flask） |
| 其他（GitHub Pages） | Render 雲端 |

所以本機測試只要在 `saamiin/Diula` 那邊跑起 Flask，再直接用瀏覽器打開這支 `index.html` 就好：

```bash
python flask_app.py       # 在 Diula repo，起在 5001
```

後端網址換了的話，改 `index.html` 裡的 `CLOUD_API` 那一行。

## 注意：Firestore 免費配額

比對走 Firebase Spark 免費方案，**一天 5 萬次讀取**，一次比對最多燒 ~900 次
→ 後端設了 `MATCH_DAILY_CAP=120` 的每日上限，超過會回 429。

因此這頁**不會自動比對**，只有按下「比對」才送出。（早期版本一打開網頁就自動查一次，
等於每個人開頁／每次重新整理都燒掉一次額度。）
