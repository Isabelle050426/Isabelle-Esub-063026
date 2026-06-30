# 醫療器材智慧化查驗登記與審查系統 (MED-AGENT PRO)
## 系統架構、代理人認知管道、合規預審與 Wow 級互動儀表板之全面技術規格書
### Technical Specification for Intelligent Medical Device Registration and Review System (MED-AGENT PRO)

---

## 1. 執行摘要與行業痛點分析 (Executive Summary & Industry Pain Points)

### 1.1 項目背景
在當今高度受規管的醫療器材 (Medical Device) 產業中，產品進入市場的關鍵限速步驟 (Rate-Limiting Step) 在於各國法規主管機關的「查驗登記 (Registration and Approval)」程序。以台灣衛生福利部食品藥物管理署 (TFDA) 為例，醫療器材的查驗登記與上市後管理必須嚴格遵循《醫療器材管理法》及附屬的《醫療器材許可證核發與登錄及年度申報準則》（以下簡稱《申報準則》）。

查驗登記流程高度繁瑣，申請商（醫療器材商）需要準備龐大的行政文件與技術性資料。一項中高風險（第二、三等級）的醫療器材，其送審文件動輒數百至數千頁，涵蓋：
1. **行政文件**：查驗登記申請書、原廠授權書、醫療器材商許可執照、出產國許可製售證明 (FSC) 等。
2. **技術性資料**：結構、材料、規格、製造流程圖、滅菌確效報告、軟體確效報告等。
3. **臨床前測試報告**：生物相容性 (ISO 10993 系列)、電氣安全與電磁相容性 (IEC 60601 系列) 等。
4. **臨床證據資料**（若適用）。

### 1.2 核心行業痛點 (Industry Pain Points)
1. **高昂的人工撰寫與建檔成本**：法規事務 (Regulatory Affairs, RA) 人員需要手動從數百頁的英文研發規格書 (Design History File, DHF) 中，提取資料並填報繁瑣的 TFDA 申請表單。
2. **送審資料的前後矛盾 (Inconsistencies)**：由於多個技術報告由不同部門或外包實驗室撰寫，極易出現規格、材質或滅菌參數不一致的情況（例如：中文說明書聲稱產品耐受 134°C 高壓蒸氣滅菌，但生物相容性報告中註明塑料材質在 120°C 以上會釋放有害物質）。
3. **極高的退件補件率 (High RFI Rate)**：審查委員常因「資料不齊全」或「未對標最新國家/國際標準」而發出補件通知 (Request for Information, RFI)，每次補件往返少則一個月，多則半年，嚴重拖累產品上市時程 (Time-to-Market)，造成數百萬至數千萬元的市場機會成本損失。
4. **國際申報資料轉換困難**：大批進口醫材的原始申報資料皆為美版 FDA 510(k) 或歐盟 MDR 格式，若直接使用通用翻譯工具，會因缺乏「台灣官方標準法規用語」而導致審查官難以理解，甚至被直接退件。

### 1.3 系統願景與核心價值
**MED-AGENT PRO** 是一套專為醫療器材法規事務 (RA) 人員與審查人員設計的「主動式 AI 代理人 (Agentic AI) 查驗登記輔助系統」。本系統整合先進的大型語言模型 (LLM)，以 Gemini 1.5 Flash-Lite 為預設極速引擎，配合 Gemini 1.5 Pro 進行深層交叉比對與法規推理，打造出兼具極致美學、高吞吐量與深度合規性的工作平台。

本系統的核心價值在於：
* **零人工建檔 (Zero-Friction Ingestion)**：用戶僅需上傳或粘貼原始的醫療器材產品摘要（如 txt、md 等格式之規格書、研發總結或國外上市申報草稿），系統即能精準提取 TFDA 申報表單所需之結構化欄位。
* **主動式代理人問答 (Agentic Multi-Turn Co-Pilot)**：針對資料不足處，代理人會主動生成 30 個精準的補件提問，以引導式對話補全關鍵技術參數。
* **極致合規比對 (Strict Regulatory Alignment)**：基於《醫療器材許可證核發與登錄及年度申報準則》，主動掃描、交叉比對整套申報資料與技術報告，抓出矛盾點（如分類品項代碼與預期用途不符、規格與類似品不對等），最大化降低退件率。
* **Wow 級視覺與監控 (Immersive Analytics HUD)**：提供高質感的「Sleek Interface」太空岩灰（Space Slate）與高對比靛藍視覺主題，內建「LLM 思考軌跡與執行遙測儀（Live Execution Telemetry）」、即時運作日誌（Live Logs）以及 6 大互動式圖表的「Wow 級可視化儀表板」，讓複雜的法規審查數據一目了然。

---

## 2. 系統架構與設計哲學 (System Architecture & Design Philosophy)

MED-AGENT PRO 採用現代 Full-Stack（全棧）SPA 與微服務架構，結合「邊緣端渲染 (Client-Side Rendering, CSR)」與「伺服器端代理人協調器 (Server-Side Agent Orchestrator)」模式，確保資料隱私（符合醫療產業合規性）與流暢的用戶體驗。

### 2.1 系統架構圖 (Architecture Diagram)

```
                               +----------------------------------+
                               |        MED-AGENT PRO UI          |
                               | (Space Slate & clinical theme)   |
                               +-----------------+----------------+
                                                 |
                                                 | Secure HTTPS (JSON API)
                                                 v
                       +-------------------------+-------------------------+
                       |              Express.js App Server                |
                       |       (Node.js + TSX Runtime / Port 3000)         |
                       +-------+-----------------+-----------------+-------+
                               |                 |                 |
         +---------------------+                 |                 +---------------------+
         |                                       |                                       |
         v                                       v                                       v
+--------+--------------+              +--------+--------------+              +--------+--------------+
|   Gemini AI Gateway   |              |  Local Rule Sentry    |              | Export & Translation  |
| (google/genai Server) |              | (MOHW SQLite / Files) |              |  (Markdown/PDF/HTML)  |
+--------+--------------+              +-----------------------+              +-----------------------+
         |
         +--> Default: gemini-1.5-flash-lite (Latency <150ms)
         +--> High-Reasoning: gemini-1.5-pro (For Deep Cross-Ref Audit)
```

### 2.2 技術棧組成 (Technology Stack)
* **前端框架 (Frontend)**：React 18+，使用 Vite 作為建置工具，確保零 HMR 延遲與最優化 bundle 大小。
* **樣式與主題 (Styling)**：Tailwind CSS v4+，導入自訂色彩系統，嚴格遵循「Sleek Interface」設計指導。
* **動畫與互動 (Animation)**：使用 `motion/react`（原 `framer-motion`）實現多步驟表單切換、Agent 思考脈衝波、即時日誌滾動之微動畫。
* **資料視覺化 (Visualization)**：D3.js 與 Recharts，用於繪製法規熱力圖、類似品關聯網路圖、合規缺口雷達圖等 6 大視覺化組件。
* **後端引擎 (Backend)**：Node.js + Express + TSX (TypeScript Execute)，將所有 LLM 調用封裝於後端 `/api/*` 路由中。
* **LLM SDK**：官方最新 `@google/genai` (v2.4.0+)，堅決不用舊版 `@google/generative-ai`。

### 2.3 LLM 託管與安全性哲學 (API Key Security & Lazy Initialization)
根據本系統環境之安全規範，醫療器材專案規格（包括專利結構設計、未公開測試數據）屬於高度敏感的企業機密。
1. **零客戶端金鑰洩露**：系統中所有與 Gemini API 互動的邏輯，皆透過後端 Express 伺服器代理。`process.env.GEMINI_API_KEY` 絕不暴露於瀏覽器環境。
2. **惰性初始化 (Lazy Initialization)**：為避免開發與伺服器啟動階段因缺少 API 金鑰而導致 Container 崩潰，系統採用惰性載入模式：
```typescript
// server/gemini.ts
import { GoogleGenAI } from "@google/genai";

let aiClient: GoogleGenAI | null = null;

export function getGenAIClient(): GoogleGenAI {
  if (!aiClient) {
    const apiKey = process.env.GEMINI_API_KEY;
    if (!apiKey) {
      throw new Error("CRITICAL: GEMINI_API_KEY environment variable is missing. Configure secrets in AI Studio.");
    }
    aiClient = new GoogleGenAI({ apiKey });
  }
  return aiClient;
}
```
3. **輸入隔離演算法**：在資料發送至 LLM 之前，後端設有敏感資訊去識別化過濾器，確保病患個資 (PHI) 或極度敏感之軍工級加密結構參數不隨意上傳至外部伺服器。

---

## 3. 「Sleek Interface」極致美學視覺設計 (Visual Design & Theme Specifications)

本系統嚴格背離市面上常見的平庸、千篇一律的「AI 科技風」（如刺眼的純黑背景搭配亮綠霓虹漸層，或慘白的多維表格），而是深入淬鍊出**「Sleek Interface」之美學精髓，完美融合「高對比太空岩灰 (Space Slate)」、「冷冽醫學藍 (Clinical Blue)」與「靜謐墨黑 (Deep Charcoal)」**。

### 3.1 視覺與排版規格 (Typography & Spacing)
* **主體字型 (Primary Font)**：採用 Inter 作為全系統通用字，並將追蹤間距 (Tracking) 設為 -0.015em 以呈現現代、緊湊的數位質感。
* **技術數據字型 (Data & Telemetry Font)**：採用 JetBrains Mono，用於顯示「模型思維日誌 (Thoughts Board)」、「API 反應延遲計時器」與「查驗登記表單之系統自動填入代碼（如 P.2010 等分類碼）」。
* **布局網格 (Adaptive Workspace Layout)**：
  * 主框架固定為三欄式布局（由左至右：AI 代理人協調面板 (340px) -> TFDA 核心查驗表單工作區 (Flex-1) -> 合規視覺化監控艙 (420px, 可收折)）。
  * 邊距與內襯 (Margins & Paddings) 採非對稱節奏（例如，非關鍵數據卡片使用 p-4，核心表單主面板則使用 p-8 搭配寬廣的負空間，打破死板的機械化網格）。

### 3.2 自訂 Tailwind 色彩系統 (Color Palette)
```css
@theme {
  --color-slate-950: #020617; /* 全局畫布底色 */
  --color-slate-900: #0f172a; /* 核心卡片與表單區底色 */
  --color-slate-800: #1e293b; /* 輸入框與元件邊框色 */
  --color-slate-400: #94a3b8; /* 副文本與描述字體色 */
  --color-blue-600: #2563eb;  /* 核心焦點色彩，代表臨床與專業度 */
  --color-blue-500: #3b82f6;  /* 亮藍按鈕與活躍狀態 */
  --color-emerald-500: #10b981;/* 合規通過、高信度標籤 */
  --color-amber-500: #f59e0b;  /* 潛在矛盾警示色 */
  --color-rose-500: #ef4444;   /* 嚴重的合規缺失、漏項紅牌 */
}
```

### 3.3 高階微動畫與互動反饋 (Interactive Motion Design)
為了賦予 LLM 枯燥的文字生成過程強烈的「生命感」與「精確度」，系統引入以下 Wow 動畫特效：
1. **思維脈衝波 (Reasoning Ripple Effect)**：當後端 Agent 正在比對資料或執行 LLM 推理時，AI 側邊欄頂部的「Gemini 1.5 Flash-Lite Active」指示燈會擴散出三重透明度漸變的靛藍色漣漪圈（頻率為 1.8 秒/次），動態呈現 AI 正在深思熟慮。
2. **表單欄位流光補全 (Form Field Fluid Flow)**：透過 AI 提取摘要並自動填寫表單時，被更新的 input / textarea 欄位將觸發一次 0.6 秒的流光渲染（從左至右掃過 `bg-blue-500/20` 漸層），並逐漸沉澱為淺藍色 background 表示「AI 預填入狀態 (AI-Populated)」。
3. **日誌骨架流 (Dynamic Log Stream)**：即時執行日誌採用打字機延遲與微滑動 (Slide-up in 200ms) 混合特效，讓用戶清晰感知到 Agent 正在進行的「查驗、分析、生成問卷」等分子級別步驟。

---

## 4. 主表單數據欄位設計 (Taiwan TFDA Form Specifications)

根據台灣食品藥物管理署 (TFDA) 官方規格，本系統將「基本資料」查驗登記表單進行了完美的數位化重構。以下為系統底層資料結構 (Data Schema) 與對應的 UI 元件映射，確保 100% 貼合《申報準則》的申報實務。

### 4.1 電子案號與核心控制資訊 (System Reference Headers)
* **公文文號 (Official Dispatch No)**：`string`，輸入框。
* **電子流水號 (System Serial ID)**：`string`，系統自動生成之防篡改條碼。格式為 `MDE + 民國年份 + 月份 + 5位流水號` (例如：`MDE11506300009`)。
* **申請日期 (Application Date)**：`string`，日期選擇器，自動默認當前系統時間（民國與西元雙制轉換）。
* **案件類型 (Case Type)**：`enum`，單選按鈕群組 (Radio Group)。
  * `GENERAL`：一般申請案
  * `SAME_PROD_DIFF_NAME`：同一產品不同品名
  * `EXPORT_ONLY`：專供外銷
  * `RE_APPLY_6M`：許可證有效期限屆至後六個月內重新申請

### 4.2 醫療器材屬性分類 (Device Classification Schema)
* **醫療器材種類 (Device Category)**：`enum`。
  * `GENERAL_DEVICE`：一般醫療器材
  * `IVD`：體外診斷試劑
* **案件種類 (Case Category)**：`enum`，`NEW_CASE` (新案)、`RENEWAL` (展延)、`ALTERATION` (變更)。
* **新案案號 (New Case Reference No)**：`string`，若為關聯案件，提供手動關聯輸入框。
* **產品產地 (Origin)**：`enum`，`DOMESTIC` (國產)、`IMPORTED` (輸入)、`MAINLAND_CHINA` (陸輸)。
* **產品等級 (Risk Class)**：`enum`，`CLASS_I` (第一等級)、`CLASS_II` (第二等級)、`CLASS_III` (第三等級)。
* **類似品關聯性 (Similar Product Status)**：`enum`。
  * `SIMILAR_EXISTS`：有類似品在台灣上市
  * `NO_SIMILAR_EXISTS`：無類似品
  * `WORLD_FIRST`：全球首創
* **替代條款 (Substitution Clause)**：`boolean`，是否適用「醫療器材許可證核發與登錄及年度申報準則」第六條附表二說明第七點第二款或第三款（免附第十七項資料）。

### 4.3 醫療器材名稱與效能說明 (Device Metadata & Indications)
* **中文名稱 (Chinese Name)**：`string`，文字輸入框。
* **英文名稱 (English Name)**：`string`，文字輸入框。
* **效能、用途或適應症 (Intended Use & Indications)**：`string`，大型文字域 (Text Area)，具備「詳如核定之中文說明書」快捷核取按鈕。
* **型號、規格或主要成分 (Models & Specifications / Active Ingredients)**：`string`，大型文字域 (Text Area)，具備「詳如核定之中文說明書」快捷核取按鈕。

### 4.4 TFDA 分類分級品項代碼 (TFDA Class/Grading Item Code)
系統內建完整 TFDA 醫療器材分類資料庫：
* **主類別 (Main Category)**：下拉選單 (Select)，從 A 到 P，例如：
  * `A：臨床化學及臨床毒理學`
  * `B：血液學及病理學`
  * `E：心臟血管醫學`
  * `I：一般及整形外科手術`
  * `P：放射學科學`
* **次類別 (Sub-category)**：下拉選單 (Select)，動態響應主類別選擇。例如主類別選 `P` 時，次類別可供選擇：
  * `P.1000：磁振診斷裝置`
  * `P.2010：醫學影像儲存裝置`
  * `P.2050：醫學影像管理和處理系統`
  * `P.2060：放射醫學電腦輔助診斷疑似癌症病灶軟體`

### 4.5 申請商與製造廠完整資料 (Applicant & Manufacturer Specs)
* **申請商資料（與醫療器材商許可證同步）**：
  * **統一編號 (Unified Business No)**：`string`，固定長度 8 碼。
  * **醫療器材商名稱 (Applicant Name)**：唯讀（需透過母帳號修改）。
  * **地址、負責人姓名、聯絡人姓名、電話、傳真、電子郵件**。
* **製造廠資訊**：
  * **製造廠類型**：單一製造廠 / 全部製程委託製造 / (O)/(P) 製造。
  * **製造廠名稱、製造廠國別（ISO 3166-1 標準國家代碼下拉選單，預設 TAIWAN, ROC）、製造廠地址**。

---

## 5. 代理人核心行為與智能管道 (Agentic Core & Cognitive Pipeline)

MED-AGENT PRO 的精髓在於其**「代理人思維鏈 (Agentic Chain-of-Thought)」**。當用戶粘貼一段散亂的「Submission Summary」時，系統並非執行簡單的正則匹配，而是啟動一系列結構化語義分析管道。

```
+---------------------------------------------------------------------------------------------------------+
| COGNITIVE PIPELINE STAGES                                                                               |
|                                                                                                         |
| [1. Parse & Ingest] ---------> [2. Deduce & Populate] -------> [3. Inconsistency & Contradiction Check] |
| User uploads summary           Agent populates Form Fields     Agent runs cross-field logical audit     |
| (Raw markdown/txt)             using structural schemas        "Does Class align with ISO 13485 test?"  |
|                                                                                                         |
| [6. TFDA Guide Pre-audit] <---- [5. Multi-turn Chat] <--------- [4. Elaboration Question Engine]         |
| Rules compliance assessment    User answers questions          Generate exactly 30 context-aware items  |
| "醫療器材申報準則附表二"                                                                                 |
+---------------------------------------------------------------------------------------------------------+
```

### 5.1 階段一：文本解析與表單自動填寫 (Ingestion & Auto-Population)
當用戶粘貼長篇產品研發規格書時，後端 Agent 會調用 Gemini 1.5 Flash-Lite。此模型針對高速資訊提取進行了極度優化：
1. **System Prompt 設計**：
   ```markdown
   你是一個專業的 TFDA 醫療器材法規專家。你的任務是從用戶提供的未結構化非正式產品描述 (Submission Summary) 中，提取符合《醫療器材分類分級管理辦法》與查驗登記基本資料表欄位所需的結構化 JSON 數據。
   必須精確推導出：中文名稱、英文名稱、主類別、次類別代碼、產品風險等級、預期用途、型號規格，以及製造廠國別。
   如果原始文本中提及 "Continuous spot-checking of oxygen saturation"，你應自動填寫至適應症，並將分類碼關聯至 "P.1560 超音波回音影像系統" 或其他正確的放射/診斷子類別。
   請嚴格輸出符合格式的 JSON，切勿包含額外解釋。
   ```
2. **多模態提取**：若用戶黏貼的 Summary 中含有規格對照表或 Markdown 嵌套表格，Agent 能完全理解並保留其表格結構，精準抽離型號。

### 5.2 階段二：30道主動追問引擎 (The 30-Question Elaboration Engine)
查驗登記極少能靠一份原始大綱就 100% 填報完成。為了避免讓用戶面對空白表單冥思苦想，Agent 會啟動一個**「引導式補件問卷生成器」**。
* **工作機制**：比對已提取的表單欄位與「完美送審模型 (Perfect Submission Template)」，找出技術、臨床前或行政資料上的空缺 (Gap Analysis)。
* **生成機制**：Agent 會生成整整 30 道精心設計、上下文高度相關的提問。
* **提問結構與示例 (Traditional Chinese)**：
  * **第 1-10 題 (行政與基本分類)**：「貴公司申報之『OxyFlow Pulse Monitor』，經初步判斷屬於第二等級醫材。請問是否已有類似品在台灣上市？若有，請提供其許可證字號以利進行『有無類似品』之實質等同性 (Substantial Equivalence) 比對。」
  * **第 11-20 題 (技術與臨床前測試)**：「本產品包含無線傳輸模組，但摘要中未提及無線電安全測試。請問本裝置是否通過 IEC 60601-1-2 (EMC) 電磁相容性測試？若有，請註明其測試實驗室是否具備 TAF 認證標章。」
  * **第 21-30 題 (滅菌與生物相容性確效)**：「文件提及外殼接觸患者皮膚。請問本裝置與人體接觸時間是否超過 24 小時？請提供符合 ISO 10993-1 之生物相容性測試（如細胞毒性、致敏性及刺激性測試）之摘要數據。」
* **互動 UI 設計**：
  這 30 道問題絕非死板地一次性鋪在畫面上，而是以**「互動式氣泡卡片流」**展現。用戶可以逐一回答，或由 Agent 輔助引導。每當用戶在對話框中輸入答案，Agent 就會即時更新主表單對應的欄位，並動態將該問題標記為「已補全 (Resolved)」。

### 5.3 階段三：邏輯衝突主動偵測與警示 (Inconsistency & Contradiction Detection)
此功能為本系統的 Wow 特色之一。Agent 不僅能填寫資料，還能擔任「法規紅隊 (Regulatory Red Teaming)」，主動審查申報資料中隱藏的矛盾與陷阱。
* **比對演算法**：
  * **風險等級與分類代碼衝突**：若表單中填寫的分類碼為 `P.2010 (醫學影像儲存裝置)`，此類別一般為第一或第二等級。但如果產品等級被勾選為 `第三等級`，Agent 立即偵測到不對等，彈出琥珀色 (Amber) 警示窗。
  * **適應症與測試報告矛盾**：若 summary 中宣稱產品具有「高壓除顫防護 (Defibrillation-Proof)」功能，但 Agent 掃描技術規格資料時發現並未標註「符合 IEC 60601-1 防除顫電擊安全標準」，即會標註該欄位為「待確認衝突點 (Inconsistent)」。
* **警示介面 (Wow Indicator Card)**：
  ```
  ▲ 偵測到邏輯不一致 (Logical Inconsistency Found)
  -------------------------------------------------------------
  * 衝突欄位：[型號規格/主要成分] <-> [臨床前滅菌確效報告]
  * 說明：產品摘要宣稱本器材材質為 'Polycarbonate (PC)' 且
    採用 '134°C 高壓蒸氣滅菌 (Steam Autoclave)'。但技術規格
    指出該 PC 塑料級別在高於 121°C 時會發生結構脆化。
  * 建議修正：請確認是否應修正滅菌方式為 'EO Gas'，
    或將外殼材質提升為醫療級聚碸 (Polysulfone, PSU)。
  ```

### 5.4 階段四：TFDA 法規合規預審器 (Review Guidance Check Agent)
在用戶填寫完整表單後，Agent 會調用**台灣《醫療器材許可證核發與登錄及年度申報準則》附表二（第二、三等級醫療器材查驗登記應檢附資料表）**進行終極虛擬審查。
* **合規審核清單 (Pre-Audit Checklist)**：
  1. 是否檢附醫療器材商許可執照？
  2. 是否檢附製造廠符合醫療器材品質管理系統準則 (QSD) 之證明文件？
  3. 技術性資料是否包含結構、材料、規格及圖樣？
  4. 是否具有完整符合 ISO 13485 體系之產品研發歷史檔案 (Design History File, DHF) 說明？
* **評估報告輸出**：
  Agent 會生成一份優雅的合規性量化雷達圖。如果發現漏缺（例如：宣稱是進口醫材，卻未檢附「出產國許可製售證明 (FSC)」，亦不符合免除替代條款），系統會給予強烈的紅字警告，並在「Wow AI 執行日誌」中詳細標出漏缺法條（如：違反《申報準則》第十二條第三款）。

---

## 6. 三大額外 Wow 級 AI 特色功能 (Three Additional Wow AI Features)

為了讓本系統在競品中脫穎而出，展現無與倫比的智能化實力，本系統特別量身打造了以下三大 Wow AI Feature，分別針對「法規對標」、「市場智庫」與「國際化申報」三大高價值痛點：

### 6.1 Wow Feature A：全球法規語義對標與標準交叉索引 (Global Standards Semantic Cross-Referencing)
* **痛點**：RA 人員在撰寫 TFDA 申報書時，往往需要手動去翻閱國際標準（如 ISO 13485、IEC 60601-1、ISO 14971、ISO 10993）來比對條文，極度耗時。
* **AI 實現**：系統內建「全球醫療器材標準語義知識庫」。當用戶在表單中輸入某一項技術效能描述（例如：`RF wireless transmission for physical monitoring`），Agent 會主動調用 Gemini 1.5 Pro 的超長上下文與深層推理能力，自動在畫面右側生成一個**「法規標準交叉索引卡 (Standards Cross-Ref Widget)」**。
* **功能展現**：
  1. 自動列出適用於該產品的所有最新版國際標準與台灣 CNS 國家標準（如 CNS 15015、IEC 60601-1-11）。
  2. 一鍵生成標準符合性聲明 (Declaration of Conformity, DoC) 範本，自動帶入表單中的產品名稱與型號，省去法規人員手動檢索的時間。

### 6.2 Wow Feature B：查驗登記核准成功率預測與風險熱力圖 (TFDA Approval Predictive Analytics & Risk Heatmap)
* **痛點**：送審是一場賭博。申請商無法預知此案在 TFDA 的審查難度，以及被要求補件的概率。
* **AI 實現**：基於歷史公開之 TFDA 補件退件案例、類似品查驗登記審查重點 (Review Guidelines)，以及產品當前表單填寫的完整度，運行一個「預測機率分類器」。
* **功能展現**：
  1. **核准率評分 (Approval Probability Score)**：即時計算當前申報資料的獲准成功率（例如：84.2%）。
  2. **補件風險熱力圖 (RFI Risk Heatmap)**：將表單劃分為五大風險維度：「行政文件完整性」、「預期用途等同性」、「電磁相容性風險」、「無菌與生物安全性確效」及「型號規格描述一致性」。以高質感的雷達圖與紅色/黃色熱力區塊顯示哪一部分是「高危區（最有可能被審查委員要求補件的段落）」，並給出具體的「防禦性撰寫建議」。

### 6.3 Wow Feature C：多國法規極速轉換與繁體中文（台灣法規體系）語義翻譯器 (Cross-Border Multi-Reg Synthesizer & Taiwan Regulatory Translator)
* **痛點**：許多進口醫材的原始資料均為英文、日文或歐洲 MDR 格式，若直接用 Google 翻譯成中文，會出現大量「非台灣官方標準法規用語」（例如將 "Device" 翻譯成 "設備" 而非台灣法定用語 "器材"，將 "Indications" 翻譯成 "適應症候" 而非 "適應症"）。
* **AI 實現**：內建「台灣 TFDA 專用語義對照引擎」，採用專門定義的 System Persona 來調用 Gemini：
  * **System Persona 指令**：「你是一名在台灣從事醫療器材查驗登記超過 15 年的資深 RA 顧問。請將以下非台灣規格的醫材申報大綱，轉化為 2000 到 3000 字、文筆流暢精確、完全符合 TFDA 官方習慣的繁體中文申報精華摘要。」
* **功能展現**：
  1. **極速翻譯**：自動將上傳的英文 FDA 510(k) 摘要或 MDR 技術文檔，翻譯並重組為完全符合《醫療器材許可證核發與登錄及年度申報準則》法定格式的繁體中文查驗登記申報精華摘要。
  2. **專有名詞校正**：自動確保所有專業詞彙 100% 採用台灣衛生福利部官方公告用語（例如：Validation -> 確效，Specification -> 規格，Instruction for Use (IFU) -> 使用說明書，Biocompatibility -> 生物相容性）。這不僅是語言翻譯，更是底層「法規體系」的語義對接。

---

## 7. Wow 級互動式儀表板與 6 大圖表規格 (The Wow Interactive Dashboard - 6 Graph Specs)

為了讓審查與填報進度一目了然，MED-AGENT PRO 右側工作區配備了一個全動態、即時更新的法規戰略儀表板 (Compliance Command Dashboard)。儀表板內含 6 大精心設計的互動式圖表（基於 Recharts / D3.js 設計規格）：

### 7.1 圖表 1：TFDA 申報基本資料「八維合規度雷達圖」(Eight-Axis Pre-Audit Radar Chart)
* **設計規格**：Recharts `<RadarChart>` 封裝。以高飽和度冷藍流線（`stroke="#3b82f6" fill="#3b82f6" fillOpacity={0.2}`）繪製。
* **維度指標**：
  1. 行政資格 (Administrative)
  2. 分類分級精確度 (Class Alignment)
  3. 效能用途等同性 (Intended Use)
  4. 結構材料完整性 (Materials Spec)
  5. 電氣與 EMC 確效 (IEC 60601)
  6. 滅菌與壽命測試 (Sterilization)
  7. 生物安全性防護 (ISO 10993)
  8. 臨床試驗證據 (Clinical Trial)
* **動態互動**：當用戶回答 30 道問題中的某一題（例如，補全了 ISO 10993 生物相容性測試代號），雷達圖中的「生物安全性防護」頂點會平滑地（使用 motion 彈簧動效，`stiffness: 100`）向外拉伸，合規百分比即時攀升。

### 7.2 圖表 2：補件退件機率「威脅等級熱力圖」(RFI Probability Risk Heatmap)
* **設計規格**：自訂 D3 Grid / Recharts Heatmap 區塊。由淺綠、淡黃、漸變至深紅（`#10b981` -> `#f59e0b` -> `#ef4444`）。
* **監控區塊**：橫軸為「送審章節」，縱軸為「審查委員過往關注敏感度」。
* **動態互動**：滑鼠懸停在熱力圖高風險（深紅）格子時，會彈出浮動提示框 (Tooltip)，提示該處之所以高風險的原因（例如：「*歷史統計：此類高頻電刀之次類別，審查委員 92% 會追問高頻洩漏電流測試報告，請務必補齊。*」）。

### 7.3 圖表 3：類似品等同性「關聯網路拓撲圖」(Substantial Equivalence Predicate Network)
* **設計規格**：基於 D3 力導向圖 (Force-Directed Graph)。
* **節點設計**：中心節點為「本申請案 (Your Device)」，周邊為已被 TFDA 核准上市之「類似品 A、類似品 B、類似品 C」。連線的粗細表示「技術等同性相似度比例 (Similarity Index)」。
* **動態互動**：用戶點擊周邊類似品節點時，拓撲圖會優雅展開，並在下方顯示雙向「技術規格對照表 (Side-by-Side Comparison)」，自動標示出相似點與差異點。

### 7.4 圖表 4：30 道問題填報進度與信心指數「動態雙軸折線圖」(Progress & Confidence Dual-Axis Line Chart)
* **設計規格**：Recharts `<AreaChart>` 與 `<LineChart>` 雙軸混合。
* **資料指標**：橫軸為「問題填寫進度 (1-30)」；左縱軸為「填報完成率 (0-100%)」；右縱軸為「模型語義信心指數 (Semantic Confidence Index, 0.0-1.0)」。
* **動態互動**：隨著用戶逐題填入資料，折線下方的面積填充色（`fill="url(#colorProgress)"`）會動態上漲，且信心折線會隨著輸入品質的高低而起伏，實時展現 AI 對資料質量的評估。

### 7.5 圖表 5：ISO 10993 人體接觸風險「矩陣泡泡圖」(ISO 10993 Contact-Risk Bubble Chart)
* **設計規格**：散點圖 (Scatter Chart) 變體。
* **矩陣維度**：橫軸為「接觸時間 (A: 暫時 <=24h, B: 中期 <30d, C: 長期 >=30d)」；縱軸為「接觸性質 (皮膚、黏膜、組織/骨、血液)」。
* **動態互動**：系統根據 Form 自動填寫的材質與預期用途，在相應的象限生成一個有呼吸動畫的藍色泡泡。泡泡的大小表示該產品在 ISO 10993 標準下必須強制檢附的生物測試項目數量。點擊泡泡即能展開「測試項目檢查表（如細胞毒性、致敏、刺激、亞慢性毒性）」。

### 7.6 圖表 6：Wow AI 執行效能與 Token 成本「動態監控 HUD」(Model Telemetry & Cost Analyzer Line Chart)
* **設計規格**：極具未來科技感的 JetBrains Mono 字體數據面板，搭配 30 秒即時更新的波動折線圖。
* **數據指標**：
  * 當前驅動模型：`gemini-1.5-flash-lite (Active)`
  * 算力延遲：`142ms / API Call`
  * 累積輸入 Token：`4,821 / 1,000,000`
  * 累積輸出 Token：`2,190 / 1,000,000`
  * 單次預估算力成本：`$0.000142 USD` (Wow 級微觀透明化)
* **動態互動**：每一次用戶按下「Parse and Fill Form」或 Agent 生成問題時，折線圖會出現一次高頻波動脈衝，隨後平滑恢復，完美展現後端 AI 引擎的實時呼吸與算力消耗。

---

## 8. 智慧化 LLM 執行軌跡、日誌與提示面板 (Live Execution Telemetry & Prompt Tuning HUD)

在許多低端 AI 應用中，LLM 的調用過程完全是一個「黑盒子」，用戶只能乾等加載動畫，這極大地損害了專業用戶的信任。MED-AGENT PRO 引入了 Wow 級執行軌跡面板，將 Agent 的大腦思維透明化。

### 8.1 模型思考軌跡流 (Thoughts Board)
系統在側邊欄下方設有一個隱藏式/可展開的面板：「Agent 內心思維鏈 (Internal CoT Telemetry)」。此處以 JetBrains Mono 字體、深灰色高對比配色滾動顯示 Agent 正在運行的內部推理步驟：
```yaml
[SYSTEM] Initiating Agentic CoT. Trigger: submission_summary_pasted.
[THOUGHT] Analyzing input text (Length: 1420 chars). Defaulting to gemini-1.5-flash-lite.
[THOUGHT] Primary entity identified: "OxyFlow Pulse Monitor". Primary material: Polycarbonate.
[THOUGHT] Mapping to TFDA classification database. Keywords matched: "oxygen", "pulse", "continuous".
[THOUGHT] Match found: P.1560 (Ultrasonic echography system) is incorrect. Correct match: P.1540 (Non-neonatal ultrasonic monitor) or P.1550? Retrying logic...
[THOUGHT] Classification resolved to Main Class: P (Radiology), Sub Class: P.2010 (Medical Image Storage Device) with 94% confidence.
[THOUGHT] Formatting structured JSON response... Done (Latency: 184ms).
[THOUGHT] Scanning regulatory compliance. Checking against Taiwan '醫療器材許可證核發與登錄及年度申報準則' Appendix 2.
[WARNING] Material is PC but sterilization is 134C Autoclave. Inconsistency flag raised. Adding to audit_logs.
[THOUGHT] Generating 30 interactive questions. Constructing question 14 of 30 targeting sterilization...
```

### 8.2 實時執行日誌 (Live Activity Log)
這是一個系統邊緣的半透明終端，向法規專家展示系統後台正在處理的事務。它採用 WebSocket 模擬流，實時推送：
* `[INFO] Form auto-save complete. Serialized state key: TFDA-DRAFT-2024-X982.`
* `[API] Google GenAI SDK call: GenerateContent success. Model: gemini-1.5-flash-lite.`
* `[AUDIT] Run logical audit on Class II. Found 2 contradictions, 14 empty required fields.`
* `[SUCCESS] Output generated in Traditional Chinese (TW) compliant vocabulary.`

### 8.3 提示詞調校與模型切換面板 (Prompt Tuning & Model Selector HUD)
這是一項極致高階的 Wow 功能，專為具備資深法規開發背景或技術狂熱的 RA 主管設計。
1. **模型切換器 (Model Selector)**：
   提供極致直觀的開關切換：
   * **Gemini 1.5 Flash-Lite (預設)**：極速填表、快速問答，平均響應延遲 < 0.2 秒。
   * **Gemini 1.5 Pro**：用於「終極合規預審器」與「多國法規翻譯對標」，能進行極其深度的多文檔交叉比對，並理解上萬頁的複雜法規文本。
2. **提示詞編輯器 (System Prompt Modifier Drawer)**：
   用戶可展開「進階提示詞抽屜 (Advanced Prompt Tuning)」，直接在代碼編輯器（模擬式 Monaco Editor 質感）中修改 Agent 運作時的底層提示詞。例如，用戶可以修改 System Prompt，要求 Agent 「採用更加嚴苛、近似 TFDA 審查官的批判口吻進行預審，不留任何情面」，或「將所有技術術語對標至歐盟 MDR 體系，而非台灣 TFDA體系」。修改後，點擊「Apply Tuning」立即生效，體驗極致的定制化掌控感。

---

## 9. 導出管道、資料完整性與多語言架構 (Export Pipeline & Localization)

### 9.1 格式保真導出引擎 (Multi-Format Export Pipeline)
系統提供完美的「一次填寫，多格式無損導出」引擎。所有導出的文件均經過了精心的格式與排版優化，保證資料結構 100% 完整。
1. **JSON 導出 (Structured Schema)**：
   導出標準、經過精簡且完全自說明的結構化 JSON 文件。該 JSON 可直接用作政府電子化送件系統（如工研院或衛福部線上電子送件系統）的底層資料導入。
2. **Markdown 導出 (The 3000-Word Report)**：
   導出一份結構優雅、符合學術與官方送審格式的技術說明書草稿。內含：產品基本資訊、分類品項對比、類似品等同性論證 (Substantial Equivalence Section)、以及 ISO 10993/IEC 60601 測試項目矩陣。
3. **HTML 導出 (Printers-Ready Template)**：
   導出包含嵌入式內聯 CSS 樣式的單網頁 HTML 文件，完整重現「第二、三等級醫療器材查驗登記送審表」的官方列印排版樣式。
4. **PDF 導出 (Print to Document)**：
   利用前端 `window.print()` 或後端 PDF 渲染管道，提供完美的紙張排版。移除所有 UI 控制按鈕、HUD 儀表板與側邊欄，只保留符合 A4 規格、帶有頁碼與官方規格表格的**「第二、三等級醫療器材查驗登記送審表紙本格式」**。

### 9.2 繁體中文本地化與國際化 (Traditional Chinese & Internationalization)
為了確保本系統完美服務於台灣及大中華區的醫療器材商，MED-AGENT PRO 採用了全系統繁體中文語境設計：
* **語系開關**：可在右上角極速切換「Traditional Chinese (TW)」與「English (US)」。
* **法規文本語義庫**：所有的表單欄位說明、AI 發出的 30 道提問、合規預審警告，均嚴格使用台灣醫療器材產業的標準行話。例如：
  * 絕不使用「塑料」而用「塑料/聚合物材質」。
  * 絕不使用「用戶手冊」而用「中文說明書」。
  * 絕不使用「一致性」而用「等同性/相符性」。
  * 絕不使用「消毒」而用「滅菌/消毒確效」。

---

## 10. 潛在 Bug 防治、異常處理與白屏修復策略 (Bug Prevention & Error Recovery)

作為一款具備高度 Wow 級動效與複雜 React 狀態交互的系統，MED-AGENT PRO 在底層架構上制定了鋼鐵般的「防崩潰、防白屏」機制。

### 10.1 白屏 Bug 的根源與主動防護 (The "Blank Screen" Prevention)
在 React 18 應用中，白屏通常源自「React 渲染週期中未捕獲的執行期錯誤 (Runtime Exceptions)」，特別是當 LLM 返回了不完整、損壞或非預期的 JSON 格式時，前端組件若直接讀取 `data.nested.field` 且未作 Optional Chaining (`?.`)，會直接觸發 React 崩潰。

#### 預防手段 A：全局 Error Boundary 封裝
在全系統的核心組件（如 6 大圖表、主表單、AI 側邊欄）外層，全部嵌套 React ErrorBoundary。當某一圖表（例如因 D3 計算異常）崩潰時，僅該圖表區域顯示優雅的「圖表算力重設中 (Telemetry Recalibrating...)」琥珀色提示框，而整套系統、主表單與對話功能完全不受影響，保證 100% 業務連續性。

#### 預防手段 B：強健的 JSON 解析防爆盾 (Robust JSON Parsing Shield)
當 Gemini 1.5 Flash-Lite 返回自動填表數據時，系統絕不直接執行 `JSON.parse(apiResponse)`。而是通過以下防爆防護網：
```typescript
export function safeParseLLMResponse(rawText: string): Record<string, any> {
  try {
    // 1. 清理 LLM 可能返回的 Markdown Codeblock 包裝 (```json ... ```)
    let cleaned = rawText.trim();
    if (cleaned.startsWith("```json")) {
      cleaned = cleaned.substring(7);
    } else if (cleaned.startsWith("```")) {
      cleaned = cleaned.substring(3);
    }
    if (cleaned.endsWith("```")) {
      cleaned = cleaned.substring(0, cleaned.length - 3);
    }
    cleaned = cleaned.trim();
    
    // 2. 執行安全解析
    return JSON.parse(cleaned);
  } catch (err) {
    console.error("LLM Response Formatting Error. Initiating fallback parser.", err);
    // 3. Fallback: 使用正則表達式強制抽取部分欄位，防止白屏
    return extractPartialFieldsViaRegex(rawText);
  }
}

function extractPartialFieldsViaRegex(rawText: string): Record<string, any> {
  const result: Record<string, any> = {};
  const chineseNameMatch = rawText.match(/"ChineseName"\s*:\s*"([^"]+)"/);
  if (chineseNameMatch) result.ChineseName = chineseNameMatch[1];
  
  const englishNameMatch = rawText.match(/"EnglishName"\s*:\s*"([^"]+)"/);
  if (englishNameMatch) result.EnglishName = englishNameMatch[1];
  
  // 保底填充其餘欄位，防止 React UI 崩潰
  return result;
}
```

### 10.2 異常處理與離線保存 (Offline Recovery & Auto-Save)
* **API Key 異常**：當後端 `process.env.GEMINI_API_KEY` 未配置或 API 請求配額耗盡時，前端會顯示精美的「AI 算力離線模式 (Offline Compliance Mode)」HUD 通知，並提示用戶可在「提示詞調校與金鑰配置面板」中手動輸入其備用 Key，同時開啟本地預置的「法規規則庫靜態預審引擎」，確保離線時系統依然保有基本的表單填寫與導出功能。
* **本地防丟失緩存 (Local Storage Sentry)**：系統每隔 5 秒會自動將當前表單的所有狀態、30 道問題的答覆進度、甚至思維日誌的歷史記錄，寫入瀏覽器的 localStorage 中。當發生意外斷電、瀏覽器崩潰或用戶不小心刷新網頁時，再次打開系統會平滑復原至最後一秒的工作現場，並彈出微型提示：「*已為您恢復至民國 115/06/30 20:09:52 的填報現場 (Form state recovered)*」。

---

## 11. 20 個深度架構與法規追問 (20 Comprehensive Follow-up Questions)

為了將本系統的技術架構與合規設計進一步打磨至生產級強度，請對以下 20 個關鍵問題進行探討與反饋：

### 11.1 台灣 TFDA 法規與表單邏輯 (Regulatory & Form Logic)
1. **問題 1**：在處理《醫療器材許可證核發與登錄及年度申報準則》第六條「免附第十七項資料（替代條款）」時，系統是否需要引導用戶上傳「第十六項資料（如出產國製售證明等替代佐證材料）」的掃描檔，並由 AI 進行自動真實性與關聯性審核？
2. **問題 2**：針對 TFDA 的「中文說明書」以及「包裝標籤」草冊，系統是否應新增一個多模態 AI 審查器，讓用戶上傳說明書 PDF 或包裝盒設計圖檔，並由 Agent 比對其宣稱與主表單「效能、用途或適應症」的一致性？
3. **問題 3**：TFDA 的分類分級品項代碼 (如 P.2010、P.2050) 時有修訂。我們是否應在後端設計一個定時任務 (Cron Job) 或是動態 API，定期爬取台灣衛生福利部開放資料平台 (MOHW Open Data)，以自動更新主、次類別數據庫，防止代碼過時？
4. **問題 4**：對於「產地」勾選為「陸輸（中國大陸輸入）」的醫療器材，台灣法規通常設有特殊的輸入限制與加嚴審查。我們是否應在 Agent 邏輯中，針對陸輸產品自動啟動「兩岸醫療器材合作框架與專案輸入規範」的加嚴合規預審檢查？
5. **問題 5**：當申請商在製造廠資訊中勾選了「全部製程委託製造」時，系統是否應主動觸發追問流程，引導用戶填寫「委託製造廠 (Contract Manufacturer)」與「受託製造廠 (Subcontractor)」的 QSD (品質系統文件) 登錄編號，並進行 QSD 有效期限之合規查核？

### 11.2 Agent 智能與問答引擎 (Agent Cognitive & Q&A Engine)
6. **問題 6**：那 30 道主動提問是應該在用戶粘貼 Summary 後**「一次性全部生成」（提供列表方便 RA 統籌回答），還是應該「動態逐次生成」**（根據用戶前一個問題的回答，隨時調整、甚至刪減後續不相關的問題）？
7. **問題 7**：當用戶對 30 道問題中的某些技術提問（例如：電磁相容性測試參數）回答「我不清楚，需詢問研發部門」時，系統是否應提供「導出單一問題卡片為 Markdown/PDF 備忘錄」或「生成一鍵發送郵件範本」之功能，以便 RA 人員與研發團隊跨部門協作？
8. **問題 8**：針對 LLM (Gemini 1.5 Flash-Lite) 在表單欄位自動填充時可能發生的「幻覺 (Hallucination)」問題，我們是否應在每個被自動填寫的欄位旁設計一個「AI 信心度微型進度條」，並在信心度低於 70% 時自動將其標記為黃色、強制用戶進行人工點擊確認？
9. **問題 9**：在執行「邏輯衝突主動偵測」時，若用戶堅稱其申報材質（例如：PC 材質耐 134°C 滅菌）沒有錯誤，系統是否應允許用戶點擊「忽略此警示 (Override & Whitelist)」，並要求用戶輸入書面理由，隨後將此理由自動歸檔至送審材料的「備註說明」中？

### 11.3 視覺設計與「Wow」級體驗 (Visual & Experience Design)
10. **問題 10**：為了讓「Sleek Interface」的冷藍與太空岩灰主題達到極致的視覺舒適度，我們是否應加入「無障礙 AAA 級對比度切換」功能？或者提供「紙質白（Paper Light）主題」，以方便 RA 人員在長時間審閱白色 PDF 報告時進行眼部放鬆？
11. **問題 11**：儀表板中的「圖表 3：類似品等同性關聯網路拓撲圖」，當類似品數量極多（例如 >50 個）時，D3 力導向圖可能會出現視覺雜亂與卡頓。我們是否應實施「K-Core 節點剪枝過濾器」，僅保留相似度最高的 5-7 個核心類似品節點？
12. **問題 12**：在「LLM 思考軌跡 (Thoughts Board)」的顯示上，是否需要加入「簡化視圖」與「極客/原始日誌視圖」的切換？以防止非技術背景的普通 RA 人員被龐大的內部 Prompt 和 Token 遙測數據所淹沒。
13. **問題 13**：我們是否應在儀表板中加入一個名為「Wow 互動粒子波 (Calculus Particles)」的特效？當合規預審得分突破 90% 時，整個儀表板邊框會閃爍出代表高質感的金色流光粒子，以提供用戶無與倫比的情感反饋與完工成就感。

### 11.4 系統架構、效能與安全性 (Architecture, Performance & Security)
14. **問題 14**：本系統預設在 iframe 沙箱環境中運行。這限制了直接使用 `window.open`。為了導出並下載 MD、HTML、JSON 與 PDF，我們應該如何精準設計前端「客製化無阻礙下載 Modal (Custom Downloader Portal)」以確保檔案順利下載？
15. **問題 15**：考慮到醫療器材規格書 (Submission Summary) 可能含有高清晰度之專利結構圖。我們是否應該利用 Gemini 1.5 Pro 的多模態圖像理解能力，允許用戶直接上傳圖檔，由 Agent 進行圖樣特徵提取並自動描述其「型號、規格與結構特徵」？
16. **問題 16**：在使用 esbuild 將後端 TypeScript 程式打包編譯為單一 `dist/server.cjs` 時，針對 `@google/genai` 這個含有原生 Node 網路請求與潛在外部依賴的 SDK，我們應如何配置 `packages: 'external'` 以及環境變數，以確保冷啟動速度達到極致的 <50ms？
17. **問題 17**：為了實現完全離線的安全保證，我們是否應設計一個「全瀏覽器端 (Local-Only) 備用模式」？當後端伺服器斷線時，自動加載內置在 Web Assembly (Wasm) 中的微型本地規則比對引擎，確保 100% 的數據絕不流出用戶的本機瀏覽器。

### 11.5 數據轉譯與導出 (Data Translation & Export Pipeline)
18. **問題 18**：在導出 PDF 時，為了確保符合台灣官方「第二、三等級醫療器材查驗登記送審書」的嚴苛雙頁排版。我們是否應該在 CSS 中引入 CSS Paged Media 規範（如 `@page { size: A4; margin: 20mm; }`），並精確計算每一個 Form Section 的高度以防打印時斷行截斷？
19. **問題 19**：在「Wow Feature C」提供的 2000-3000 字繁體中文高階法規摘要導出中，我們是否應提供「歐盟 MDR / 美國 FDA 510(k) 雙向語義映射對照表」？使得一份台灣查驗登記表單能一鍵生成可向歐美申報的國際化草冊。
20. **問題 20**：系統生成的導出 JSON 數據架構，是否需要與工研院諮詢服務或衛生福利部食藥署目前的「電子送件系統 (e-Submission Portal)」之 XML/JSON Schema 進行 1:1 的精準字典映射 (Data Dictionary Mapping)，以實現真正的「官方免修改、直裝直入」？

---

## 12. 總結 (Summary)

**MED-AGENT PRO** 的設計不僅是一項軟體，更是對醫療器材合規申報流程的一次革命性效率重塑。透過：
1. **極致美學的「Sleek Interface」** 與 6 大 Wow 級動態圖表。
2. **Gemini 1.5 Flash-Lite 與 Pro 雙引擎驅動** 的主動式 AI 代理人。
3. **高達 30 道的精準上下文補件提問** 與 《醫療器材許可證核發與登錄及年度申報準則》的法規紅隊預審。
4. **無與倫比的三大 Wow AI 附加功能**（標準交叉對標、核准機率預測、台灣合規專用語義翻譯）。

本系統成功將法規人員從枯燥、重複的文件比對中解放，使其專注於真正的產品創新。本技術規格書已為 MED-AGENT PRO 奠定了穩健、合規、具備極高工業級強度的架構基礎。未來系統的實現，將以最純粹、最優雅、最無懈可擊的代碼工程，完美實踐上述所有設計藍圖。
