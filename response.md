# 風嶼：工程架構與學習說明

這是颱風原理的教學模擬，不是即時預報。地圖以幾何近似台灣，風速與雨量由時間函數生成，未使用真實觀測資料；CSS 透視用來提供空間感，沒有精確高程模型。

## 1. 為什麼使用 SVG
圖形只需輪廓、路徑與標示，SVG 可隨畫面縮放，無需下載大型衛星圖片。單一 HTML 離線可開啟；沒有外部圖片、字型、音訊或程式庫。因沒有同內容 JPEG 基準，不宣稱節省 70% 或 80%。picture / srcset / lazy 在此不適用；若日後加入照片，才應提供 AVIF→WebP→img、適當 sizes 與首屏外 lazy。

## 2. 動畫與瀏覽器渲染
瀏覽器經過樣式計算、Layout、Paint、Composite。修改 top/left 可能觸發重新配置，transform/opacity 通常有利於合成。60Hz 顯示器每格約 16.67ms，但還要共享給腳本、繪圖與其他工作。本作不逐格修改幾何尺寸；SVG 群組 transform、描邊、文字與透明度仍可能帶來 Paint，因此不能宣稱零重繪或保證零掉幀。固定的小型 SVG 場景限制了工作量，rAF 按時間差推進。更高更新率螢幕可能有更短間隔。

## 3. 回彈與感知
cubic-bezier(.34,1.56,.64,1) 用於視角及按鈕變化，讓回饋帶有超越與收斂的感覺；它是視覺模擬而非精確阻尼模型。颱風旋轉按時間差持續推進，並不套用按鈕回彈。hover 1.03、active .97 讓操作有明確狀態。尊重 prefers-reduced-motion，初始暫停並停用 CSS 過場。

## 4. SVG 路徑描邊
勾選路徑以 pathLength=100 正規化，dasharray=100 與 dashoffset 100→0 呈現逐步畫出的效果。這是規準要求的描邊動畫例外，可能重繪，不等於純合成動畫。收藏按鈕有填色與彈跳回饋。

## 5. 合成音效
Web Audio 使用獨立 OscillatorNode 與 GainNode，click 620Hz、snap 880Hz、成功和弦 523.25/659.25/783.99Hz。8ms 淡入、100ms 包絡、120ms 停止；每次使用獨立音源，播放完主動斷開連接。無音檔下載，所以音訊資源傳輸 0KB；不代表程式碼大小為零或裝置延遲為零。音效預設關閉，須使用者點擊開啟；無頭瀏覽器只驗證 API，不代替聽感檢查。

## 6. 客觀驗證方法
Network：開啟 Disable cache 重新載入，確認無遠端圖片、音訊或 CDN 請求，記錄傳輸大小。Performance：在一般可見瀏覽器錄製 10 秒播放、切視角與拖曳，檢查 Frames、Main、Layout、Paint；必要時加 CPU slowdown，記錄超過 16.67ms 比例及最慢影格。Rendering：開啟 Paint flashing 與 FPS meter，檢查重繪範圍。Audio：開音效後連點，確認不刺耳、不截斷且可關閉。請勿把 rAF 間隔直接寫成 CPU 計算時間。

## 學習方向
理解 viewBox、向量與點陣格式的適用場景；區分 Layout / Paint / Composite；使用時間差、rAF、背景分頁與減少動態偏好；掌握 SVG dash 動畫與回彈曲線；理解合成音訊生命週期、包絡與使用者啟動限制；以真實效能證據取代保證語句；理解路徑、風向與迎風坡會隨颱風位置改變。

## 科學內容參考
- 中央氣象署〈臺灣大風吹〉：https://pweb.cwa.gov.tw/PopularScience/wt/wt_30.html
- 中央氣象署〈颱風共伴效應〉：https://climate.cwa.gov.tw/ClimatePedia/detail_page/20

## 製作揭露
本作品由 AI 協助生成與測試。繳交前請自行操作、理解程式與說明，並依課程規定標示 AI 使用方式。
