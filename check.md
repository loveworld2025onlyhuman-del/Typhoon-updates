# 作品批改與自評表

及格：總分 ≥80，且「動畫屬性安全」致命項不得違規。致命項若動態修改 top/left，該項 0 分，整體不通過。

純 SVG 替代圖片時，picture 與 lazy 以不適用／等效方案記分，不捏造圖片壓縮成果。

|維度|標準|配分|暫評|通過|程式碼落證|
|---|---|---:|---:|---|---|
|資源瘦身|純向量、無外部圖檔|8|8|[x]|index.html L11；內嵌 SVG，無衛星圖片或 CDN 依賴。|
|資源瘦身|picture / AVIF / WebP|5|5|[x]|index.html L12；不適用：未載入點陣圖片，按純向量替代方案給分；不能宣稱節省 80%。|
|資源瘦身|loading="lazy"|4|4|[x]|index.html L28；不適用：首屏外只有文字卡片，無可延遲圖片。|
|資源瘦身|viewBox 與中繼資料|8|8|[x]|index.html L11；設定 900 × 520 相對座標；無繪圖軟體冗餘 metadata。|
|動態渲染|動畫屬性安全【致命項】|12|12|[x]|index.html L42；未動態修改 top / left / width / height。SVG transform 屬性與透明度更新仍可能重繪，不能宣稱全部 GPU 合成。|
|動態渲染|16.6ms 預算與量測|10|4|[ ] 待 trace|index.html L48；影格間隔實測已完成；未取得 DevTools 主執行緒 trace，此項暫給 4 / 10。|
|動態渲染|requestAnimationFrame|8|8|[x]|index.html L60；使用時間差推進、限制 dt、隱藏分頁停止推進；減少動態偏好預設暫停。|
|微互動|回彈 cubic-bezier|7|7|[x]|index.html L4；採用 (.34,1.56,.64,1)；反應先超越目標再收斂。|
|微互動|hover / active 手感|6|6|[x]|index.html L4；縮放 1.03 與 .97，提供鍵盤焦點。|
|微互動|描邊與填色彈跳|8|8|[x]|index.html L4；收藏勾選 pathLength=100，dashoffset 100→0；按鈕狀態填色與縮放彈跳。|
|微互動|50–150ms 合成音效|9|9|[x]|index.html L36；120ms 振盪器、100ms 包絡，click / snap / chord；每次點擊建立獨立音源，音檔傳輸 0KB。尚未人工聆聽品質。|
|視覺整合|深色主題與 3D 視角|5|5|[x]|index.html L4；CSS 透視示意，非地理模型或 WebGL 立體地形。|
|視覺整合|時間軸聯動|5|5|[x]|index.html L52；時間軸更新中心、情境、風速與雨量，拖曳時暫停。|
|視覺整合|Console / 響應式|5|5|[x]|index.html L4；Edge headless 測試無 pageerror，390px 無水平溢出。|

總分：94/100（條件式自評），四大維度：25/25、24/30、30/30、15/15。

實測範例：本機 Edge 無頭瀏覽器、1440×1080、180 個 rAF 間隔，平均 6.94ms，P95 7.00ms；此結果為排程間隔，不是動畫函式計算時間，不保證其他裝置零掉幀。未完成 DevTools trace 扣 6 分。

扣分規則：依各項配分，未實作給 0 分；部分完成最高給一半；影格量測項分為已完成間隔量測 4 分、待人工 Performance trace 6 分。Console 任一未處理錯誤該項 0 分。
