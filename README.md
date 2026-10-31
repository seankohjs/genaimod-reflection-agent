# ApplyReflect｜AI 實習求職信反思修訂 Agent

生成式 AI 課程的 **Reflection Agent**：以三個獨立步驟產生、審查及修訂英文求職信，Gradio 顯示每個階段。

**[Open in Colab](https://colab.research.google.com/github/seankohjs/genaimod-reflection-agent/blob/4573c05076769592615c14e7c781c51623eb7165/Week_08_ApplyReflect.ipynb)**

1. 儲存副本到 Google Drive。
2. 在 Colab Secrets 新增／啟用 **`Groq`**，使用自己的 API key。
3. Run all；若提示重啟，選 Runtime → Restart session，再 Run all。
4. 到第 5 節點「開始／繼續 Reflection」，再操作三個測試案例。

## 流程

`職缺＋確認背景 → Writer 初稿 → Reviewer 審查 → Writer 修訂`

- **Writer**：只以提供的事實撰寫，不把職缺要求當成候選人能力。
- **Reviewer**：同時接收職缺、原始背景與初稿，給出具體問題及修訂清單；JSON 格式先經 Python 檢查。
- **修訂 Writer**：收到原始資料、初稿與審查，依據事實修改。
- **Gradio**：逐步顯示三欄、字數提示及實際 API 嘗試紀錄；失敗可接續未完成步驟。

正常模式為三次模型呼叫。提供既有初稿的模式為兩次，第一欄明確標示來源；失敗重試次數也會計入紀錄。
同一模型可以擔任不同角色；程式確實把前一步輸出傳給下一步，不依賴單次回覆聲稱已反思。

## 案例與限制

預設職缺是**假想的 AI Infrastructure 實習**，背景範例不含姓名、學號或聯絡資訊。
三個案例為背景完整、資訊不足、刻意含未證實數字的測試初稿。
第三項的 `80%`、`$100,000`、團隊及生產部署主張均為**控制測試輸入，並非候選人真實成果**。

輸入背景、職缺及草稿會傳至 Groq。Key 只從 Colab Secrets／本機環境讀取，不寫入 notebook。
每個 Gradio session 分開保存目前結果；程式不會寄送求職信。模型審查及程式檢查都不能取代事實核對。

## 技術與本機驗證

參考老師 [Demo07a Reflection](https://github.com/yenlung/AI-Demo/blob/master/【Demo07a】AI代理設計模式_Reflection.ipynb)。
使用 AISuite 0.2.0、Groq SDK 0.37.1、Gradio 6.0.1，模型 `groq:openai/gpt-oss-20b`。
Groq 參數與結構化輸出依 [官方文件](https://console.groq.com/docs/structured-outputs) 設定；JSON 格式符合不等於內容正確。
Colab 保留平台內建 NumPy、pandas、Matplotlib；`requirements.txt` 是本機參考版本。

2026-11-01：在 macOS / Python 3.12，**8 個程式儲存格皆執行成功，11 組本機檢查通過**。
檢查包含三階段資料傳遞、原始事實保留、不同 session 隔離、錯誤接續、JSON 檢查與實際 Gradio API。
AISuite／Groq 請求以明確標示的離線 HTTP 替身驗證，沒有假冒真實模型輸出。
**真實 Groq 對話仍需學生使用自己的 key 操作。** 第 6 節保留實測觀察欄位。

程式與說明由 AI 協助製作；請保留實際提示詞與回覆截圖，並自行核對實測結果。
