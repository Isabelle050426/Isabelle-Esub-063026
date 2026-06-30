醫療器材智慧化查驗登記與審查系統 (MED-AGENT PRO)
全面技術規格與系統架構設計說明書 (Ultimate System Architecture & Technical Specifications Manual)
1. 系統願景與產品定位 (System Vision & Product Positioning)
1.1 醫療器材查驗登記的合規痛點 (TFDA 法規痛點)
在生技與醫療器材產業中，產品上市程序（Market Authorization）最具挑戰性的關卡即是各國法規主管機關的「查驗登記」審查。以台灣衛生福利部食品藥物管理署（TFDA）為例，所有第二等級與第三等級醫療器材均須依據《醫療器材管理法》及附屬的《醫療器材許可證核發與登錄及年度申報準則》（以下簡稱《申報準則》）進行實質審查。
法規事務（Regulatory Affairs, RA）人員在準備送審文件時，通常面臨以下嚴峻挑戰：
資訊不對稱與零散性：產品技術細節散落於研發部門的原始規格書、測試報告、設計歷程檔案（Design History File, DHF）以及國外原廠的英文上市許可文件（如 US FDA 510(k) 摘要、EU MDR 技術文檔）中。
極高的人工比對成本：RA 必須手動將這些英文非結構化資料轉譯並填入 TFDA 繁瑣的「查驗登記申請書」，過程中極易發生人為登錄錯誤。
高退件補件率（RFI Rate）：由於缺乏系統化的法規預審機制，送審資料常出現邏輯矛盾（例如：說明書宣稱之材料、型號與滅菌確效報告不對等），導致審查委員多次提出「補件要求」，使產品上市時程延宕數月甚至數年，對企業造成重大財務損失。
1.2 MED-AGENT PRO 核心價值主張
MED-AGENT PRO 是專為醫療器材商（藥商）與專業法規顧問設計的「主動式 AI 代理人查驗登記協調工作平台」。本系統秉持「零代碼、高合規、強視覺」的核心宗旨，利用 Google Gemini 1.5 Flash-Lite（作為極速推理、自動填表引擎）與 Gemini 1.5 Pro（作為深度合規預審與多國語義對接引擎），徹底重塑醫療器材查驗登記的工作流。
本系統的核心價值主張在於：
智慧化語義提取（Semantic Ingestion）：用戶只需黏貼或上傳未結構化的原始產品摘要、規格說明書（TXT/MD 格式），系統即會瞬間解析，自動推導並精準填入 TFDA 核心查驗登記表單。
主動式 30 道補件提問（30-Question Elaboration Engine）：針對填寫不完整或存在法規漏洞的區塊，系統代理人會主動生成 30 道深度追問，引導用戶補齊臨床前測試（EMC、生物相容性、滅菌確效）與行政關鍵參數。
紅隊邏輯衝突審查（Logical Contradiction Sentry）：主動交叉掃描表單內所有文字與數據，自動找出「宣稱材質與滅菌溫度衝突」、「分類等級與預期用途矛盾」等致命缺陷，並即時給予修正建議。
TFDA 官方規則自動對標：全面對標《醫療器材許可證核發與登錄及年度申報準則》，一鍵生成「合規預審報告」及「2000-3000 字繁體中文高階送審專用語摘要」。
極致美學的 Sleek 戰略監控面板：提供「太空岩灰（Space Slate）」高對比視覺主題，搭配 6 大互動式 D3/Recharts 數據圖表與「實時 AI 思考軌跡與算力成本 HUD」，賦予專業用戶無與倫比的視覺與操作掌控力。
2. 全域系統架構與資料流 (Global System Architecture & Data Flow)
MED-AGENT PRO 採用現代 Full-Stack（全棧）微服務架構，將複雜的「AI 代理人思維鏈」與「客戶端互動渲染」解耦，確保在 AI Studio 沙箱環境與高吞吐生產環境中皆能流暢、流暢地運作。
code
Code
+-----------------------------------------------------------------------------------------+
|                                    MED-AGENT PRO 前端                                   |
|   +--------------------------+  +--------------------------+  +---------------------+   |
|   |      AI 協調側邊欄       |  |     TFDA 核心申請表      |  |    合規監控 HUD     |   |
|   | (多模態上傳/執行日誌/Chat) |  | (動態流光渲染/全欄位編輯)  |  |  (6大 D3/Recharts)  |   |
|   +--------------------------+  +--------------------------+  +---------------------+   |
+--------------------------------------------+--------------------------------------------+
                                             | 
                                             | 安全 HTTPS / JSON API 交互
                                             v
+-----------------------------------------------------------------------------------------+
|                                  Express.js 後端伺服器                                   |
|   +---------------------------------------------------------------------------------+   |
|   | 核心路由控制器:                                                                  |   |
|   |  - POST /api/ingest (Gemini 1.5 Flash-Lite 自動填表與衝突偵測)                   |   |
|   |  - POST /api/generate-questions (基於語義 Gap 產生 30 道引導追問)               |   |
|   |  - POST /api/pre-audit (對標 TFDA 申報準則執行終極合規審查)                         |   |
|   |  - POST /api/translate-summary (2000-3000 字台灣官方用語繁體轉譯)                |   |
|   |  - POST /api/export (保真導出 JSON/MD/HTML/PDF 渲染緩衝)                        |   |
|   +----------------------------------------+----------------------------------------+   |
|                                            |
|                                            | 惰性加載 / 伺服器端限定金鑰 @google/genai SDK
|                                            v
|   +---------------------------------------------------------------------------------+   |
|   |                                  Google Gemini API                              |   |
|   |  - gemini-1.5-flash-lite : 負責低延遲自動填表、實時對話、日誌生成 (預設)        |   |
|   |  - gemini-1.5-pro        : 負責深層交叉合規預審、多國法規轉譯、複雜邏輯衝突審批 |   |
|   +---------------------------------------------------------------------------------+   |
+-----------------------------------------------------------------------------------------+
2.1 後端代理人協調器設計 (Server-Side Agent Orchestrator)
本系統將所有的大型語言模型（LLM）調用邏輯與金鑰管理，100% 封裝於伺服器端（Express.js 框架），客戶端不直接暴露任何 Google Gemini API。
零客戶端金鑰外洩：所有請求皆透過 Express API 轉發，process.env.GEMINI_API_KEY 僅在後端被讀取。
惰性加載機制（Lazy Initialization）：為避免在部署或啟動容器時，因臨時缺少 API Key 而導致整個進程崩潰，系統會在每次觸發後端 API 時，動態檢查並加載 SDK：
code
TypeScript
// server/services/geminiService.ts
import { GoogleGenAI } from "@google/genai";

let aiClient: GoogleGenAI | null = null;

export function getGeminiClient(): GoogleGenAI {
  if (!aiClient) {
    const apiKey = process.env.GEMINI_API_KEY;
    if (!apiKey) {
      throw new Error("CRITICAL_ERROR: GEMINI_API_KEY is not defined in the environment variables.");
    }
    aiClient = new GoogleGenAI({ apiKey });
  }
  return aiClient;
}
2.2 去識別化與隱私安全閘道 (De-identification & Privacy Gateway)
為符合醫療器材研發之高規格專利防護與病患隱私安全，後端在將資料傳送至 Gemini API 之前，設有一個高防護過濾器：
PII / PHI 自動偵測遮蔽（Data Masking）：利用高精準度正規表達式（Regular Expression）與關鍵字比對，自動掃描上傳之文本，將可能涉及的「患者姓名、身份證字號、特定醫院名稱、研究參與者個資、敏感製程參數專利公式」等進行自動標記置換（例如：[PATIENT_ID_REDACTED]、[PATIENT_NAME_REDACTED]）。
沙箱隔離運行（Sandbox Execution）：每次 API 呼叫均在獨立的上下文緩衝區中進行，不對外部系統進行資料持久化留存，100% 保證智慧財產權。
3. 智慧代理人管線與思維鏈設計 (Agentic Pipeline & Chain-of-Thought System)
MED-AGENT PRO 的最大特色在於其**「自主思維與推理管線」**，而非單純的關鍵字匹配工具。當用戶黏貼一份粗糙的「Submission Summary」後，系統會自動啟動以下四階段的代理人管道：
code
Code
+------------------+     +------------------+     +------------------+     +------------------+
|   階段 1：解析填表  | --> |   階段 2：引導追問  | --> |   階段 3：邏輯衝突  | --> |   階段 4：終極預審  |
| 語義抽離與流光渲染 |     | 30道關鍵技術提問  |     | 深度交叉矛盾掃描 |     | TFDA申報準則對標 |
+------------------+     +------------------+     +------------------+     +------------------+
3.1 階段一：文本提取與多模態自動填表 (Semantic Parsing & Auto-Population)
當用戶黏貼未結構化文本時，系統會調用 gemini-1.5-flash-lite 進行極速語義提取。
語義欄位映射演算法：模型會解析非正式詞彙，將其映射至台灣 TFDA 查驗登記基本資料表的標準格式。例如：將「This tool uses ultrasonic waves to scan peripheral vessels」映射至「效能/用途」欄位，並將產品分類推導為「P.1560 超音波回音影像系統」。
流光填充動畫（Fluid Motion Feel）：系統會在前端對應的輸入框（Input/TextArea）中觸發一次持續 0.8 秒的 bg-blue-500/20 到透明的漸層掃描動畫，向用戶展示 AI 正在「注入 (Inject)」資料，提供強烈的互動反饋。
3.2 階段二：30 道主動式引導補件問答引擎 (30-Question Elaboration Engine)
為徹底補全查驗登記所需的臨床前測試與行政資料，系統會自動執行**「Gap Analysis（缺口分析）」**。若表單中的核心欄位為空，或者缺乏關鍵安全佐證，問答引擎即會啟動。
缺口分數（Gap Score）計算公式：
系統針對 TFDA 應備齊的資料維度設定權重，計算當前表單填寫度：
其中 
 為第 
 個維度的法規權重（例如：生物相容性 
，電氣安全 
），
 為該維度的填寫完整度分數（
 至 
）。
30 道精準問卷生成機制：系統會自動合成 30 個高度上下文相關、非死板模板的專業提問，分成三大板塊：
第 1-10 題 (行政與基本分類)：例如「本產品英文品名為 OxyFlow Pulse Monitor，請問是否有計畫申請台灣商標註冊？是否具備與現有類似品 B 的實質等同性對比表？」
第 11-20 題 (技術與臨床前測試)：例如「摘要中提到具有藍牙傳輸模組，請問是否已取得 NCC 射頻器材認證？是否能提供符合 IEC 60601-1-2 (EMC) 標準的完整測試報告？」
第 21-30 題 (滅菌與生物安全性確效)：例如「本器材接觸患者皮膚。請問與人體接觸時間是否累計超過 24 小時？請補全其符合 ISO 10993-5 (細胞毒性) 與 ISO 10993-10 (刺激與致敏) 測試的實驗室代碼。」
動態對話式欄位更新 UI：這 30 道問題以優雅的「互動式對話卡片流」呈現。用戶可以逐題在對話框中回答，每答一題，Agent 就會解析該回答，即時更新主表單的對應欄位，並將圖表進度條向前推進。
3.3 階段三：邏輯衝突與前後矛盾主動偵測演算法 (Contradiction Check & Inconsistency Sentry)
本功能為系統核心的紅隊安全偵測機制，旨在送審前過濾出所有可能引發審查委員質疑的「前後矛盾點」。
交叉核對規則庫（Cross-Validation Rules）：
材質與滅菌衝突：如果表單「材料說明」填寫為 Polycarbonate (PC) 塑料，但「滅菌方式」填寫為 134°C 高壓蒸氣滅菌 (Steam Autoclave)。Agent 會立即觸發警示：因為普通 PC 塑料在高於 121°C 時會發生結構脆化，此處存在嚴重的技術矛盾。
風險等級與分類代碼衝突：如果分類品項代碼填寫為 P.2010 (醫學影像儲存裝置，通常為第一等級或第二等級)，但「產品等級」卻被勾選為 第三等級 (Class III)。系統會跳出警告。
效能宣稱與規格配對衝突：若用途中宣稱具有「心律不整自動除顫防護（Defibrillation Protection）」，但主要規格或測試標準中未標記「符合 IEC 60601-1 具防除顫功能之 F 型應用部分 (Defibrillation-proof type F applied part)」，系統即會將該欄位標記為琥珀色警示。
衝突警示卡片 UI 規格：
當偵測到衝突時，右側合規面板會優雅地以 motion 動畫滑出琥珀色高對比卡片，指出：
衝突位置：[主要成分] ↔ [臨床前滅菌報告]
衝突原因：材質耐熱極限低於設定的蒸氣滅菌溫度。
修正建議：請將滅菌方式修正為 EO 氣體滅菌，或將材質提升為 聚碸 (Polysulfone, PSU)。
3.4 階段四：基於《醫療器材許可證核發與登錄及年度申報準則》的法規預審機制 (MOHW/TFDA Pre-Audit)
在表單填寫完畢、30 道問題解答完成後，系統會啟動「終極法規審查代理人」，完全對標中華民國衛生福利部《醫療器材許可證核發與登錄及年度申報準則》附表二「第二、三等級醫療器材查驗登記應檢附資料表」：
應檢附資料八大支柱查核：
第一項、第二項：查驗登記申請書、醫療器材商許可執照影本。
第三項：出產國許可製售證明及國外原廠授權書（若屬輸入產品，審查是否符合免附替代條款）。
第十項：結構、材料、規格、性能、用途、圖樣等技術性資料。
第十一項：安全、效能及品質之研發歷程與品質管理系統文件（QSD 登錄函）。
第十二項、第十三項：臨床前測試報告（電氣安全、電磁相容、生物相容性、無菌確效等）。
第十四項：臨床證據資料（若適用）。
合規性量化分數（Compliance Quality Index, CQI）：
結合上述查核結果，系統會輸出一個 0% ~ 100% 的量化綜合合規指標。若低於 85%，系統會將缺少的法規條款（例如：違反《申報準則》第十二條第三款，未檢附原產國製售證明）以醒目的紅字提示，防止用戶盲目送審。
4. 核心查驗登記表單資料庫結構 (Data Dictionary & TFDA Form Metadata Schemas)
為確保資料在前後端傳輸及未來與官方送件系統對接時具備高度的一致性，MED-AGENT PRO 嚴格定義了以下符合 TFDA 實務查驗登記表的數據字典結構：
欄位名稱 (JSON Key)	數據類型 (Type)	格式限制 (Constraint/Regex)	欄位說明及 TFDA 對應規範
docReferenceId	String	^MDE\d{3}\d{2}\d{5}$	電子文號：例如 MDE1150600009，前三碼為民國年，後續為月份與流水號
applicationDate	String	^民國 \d{3} 年 \d{2} 月 \d{2} 日$	申請日期：民國格式雙向映射
caseType	Enum	GENERAL | SAME_PROD_DIFF_NAME | EXPORT_ONLY | RE_APPLY_6M	案件類型：一般申請、同一產品不同品名、專供外銷、重新申請等
deviceType	Enum	GENERAL_DEVICE | IVD	醫療器材種類：一般醫療器材或體外診斷醫療器材
isNewCase	Boolean	true | false	是否為全新申請案 (若是則需輸入新案案號)
originType	Enum	DOMESTIC | IMPORTED | MAINLAND_CHINA	產品產地：國產、輸入、陸輸
riskClass	Enum	CLASS_I | CLASS_II | CLASS_III	產品等級：第一、二、三等級（本系統重點防禦二三等）
hasPredicate	Enum	HAS_PREDICATE | NO_PREDICATE | WORLD_FIRST	類似品狀態：有類似品、無類似品、全球首創
substitutionApplied	Boolean	true | false	替代條款：是否適用《申報準則》第六條免附第十七項資料
chineseName	String	Max 255 chars	中文名稱：不可含有簡體字，須符合 TFDA 品名命名原則
englishName	String	Max 255 chars	英文名稱：須與國外原廠製售證明（FSC）完全一致
intendedUse	String	Max 4000 chars	效能、用途或適應症：詳述其診斷、治療或監測之醫學範疇
modelsAndSpecs	String	Max 8000 chars	型號、規格、主要成分、材料說明與包裝規格
tfdaMainCategory	String	^[A-P]$	分類大類：A(臨床化學) 至 P(放射學) 等 16 大類
tfdaSubCategory	String	^[A-P]\.\d{4}$	分類子類代碼：例如 P.2010 (醫學影像儲存裝置)
applicantTaxId	String	^\d{8}$	申請商（醫療器材商）統一編號：嚴格 8 碼數字
applicantName	String	Max 255 chars	申請商名稱：須與藥商許可執照完全一致
manufacturerName	String	Max 255 chars	製造廠名稱：外國製造廠須有完整英文譯名與國別
manufacturerCountry	String	ISO 3166-1 Alpha-2	製造廠國別：如 US (美國)、DE (德國)、JP (日本)
qsdRegNumber	String	^QSD\d{4}$	醫療器材品質管理系統（QSD/GMP）登錄編號
5. 三大額外 Wow 級 AI 功能設計 (Three Additional Wow AI Features Detailed)
除了基礎的自動填表與問答引導，MED-AGENT PRO 額外搭載了以下三個極具技術深度與商業價值的「Wow 級 AI 功能」：
code
Code
+-----------------------------------------------------------------------------------------+
|                                  三大 WOW 級 AI 功能艙                                   |
+-----------------------------------------------------------------------------------------+
| [Wow Feature 1: 國際標準語義對標]   [Wow Feature 2: 補件風險與熱力圖]   [Wow Feature 3: 台式法規轉譯引擎] |
| 跨國標準(IEC/ISO)語義檢索，自動   利用 TFDA 歷史審查數據預測退件    將 FDA/MDR 技術文檔一鍵轉譯成 |
| 生成標準符合性聲明 (DoC) 範本。     機率，並輸出五維威脅熱力雷達。    台灣食藥署專用繁體中文與格式。  |
+-----------------------------------------------------------------------------------------+
5.1 Wow Feature 1：全球國際與國家標準語義交叉對標與符合性聲明自動生成 (Global Standards Semantic Cross-Referencing)
痛點：法規事務人員在填寫主要規格或測試項目時，必須自行翻閱龐大的標準庫（如 IEC 60601-1 系列電氣安全、ISO 10993 系列生物相容性、ISO 14971 風險管理），手動核對適用條款，耗時且容易遺漏最新版本。
AI 實現機制：系統在後端建置了一個「全球醫療器材標準語義知識庫元數據對照表」。當用戶在表單中輸入技術效能或主要材質時（例如：在型號規格中填入 Wireless bluetooth module with lithium battery），gemini-1.5-pro 會在右側 HUD 面板主動觸發**「法規標準交叉索引卡（Standards Cross-Ref Widget）」**：
自動列出適用於該產品的所有最新版國際標準與台灣 CNS 國家標準（例如：CNS 15015、IEC 60601-1-2 (2020版 EMC)、IEC 60601-1-11 (居家照護安全)、UN38.3 (鋰電池運輸安全)）。
提供**「一鍵生成標準符合性聲明（Declaration of Conformity, DoC）」**按鈕。點擊後，AI 會自動帶入表單中的產品名稱、型號、製造商，並自動輸出格式完美的標準符合性聲明書 PDF 預備草稿，省去 RA 手動編寫的數小時時間。
5.2 Wow Feature 2：查驗登記核准概率預測與 RFI 威脅熱力圖 (TFDA Approval Predictive Analytics & Risk Heatmap)
痛點：送審常因「賭一把」的心理，不知道資料完備度是否能通過 TFDA 嚴苛的審查委員會，導致盲目送審被退件，拉長產品上市週期。
AI 實現機制：系統採用 gemini-1.5-pro 的強大推理能力，分析當前表單填寫度、30 道問題解答之語義完備性，並對比歷史公開的 TFDA 補件退件案例資料庫。
功能展現：
核准成功率評分 (Approval Probability Score)：在 HUD 面板最上方實時顯示百分比分數（例如：78.5%）。
補件威脅雷達熱力圖 (RFI Threat Heatmap)：將送審資料劃分為五大威脅象限（「行政文件完整度」、「技術規格等同性」、「安全測試完整性 (IEC 60601)」、「生物安全性 (ISO 10993)」、「滅菌與效期壽命測試」），並以 Recharts 繪製成高質感的紅色/黃色威脅熱力區塊。
防禦性撰寫建議 (Defensive Drafting Guide)：當用戶滑鼠懸停在熱力圖的高風險區塊時，AI 會主動跳出修改指南（例如：「高補件風險提示：您未檢附藍牙射頻晶片在台灣的 NCC 審驗合格證明。歷史統計中，此漏洞引發 94% 的退件率，建議立即在表單 [主要規格] 補上 NCC ID: CCXXxxYYyyZZzz」）。
5.3 Wow Feature 3：跨國法規極速轉換與台灣本土法規體系語義轉譯器 (Cross-Border Multi-Reg Synthesizer & Taiwan Regulatory Translator)
痛點：進口醫療器材的原始技術文檔多為英文的 US FDA 510(k) Summary 或歐盟 MDR 技術文檔（Technical File）。若直接使用常規翻譯工具轉為繁體中文，會出現大量非台灣本土法定用語的「翻譯腔」（如將 "Device" 翻成 "設備" 而非台灣法定的 "器材"；將 "Indications for Use" 翻成 "使用指引" 而非 "預期用途/適應症"），直接導致 TFDA 審查官退件。
AI 實現機制：系統內建「台灣衛福部食藥署（TFDA）本土法用語詞彙庫（Data Dictionary Mapping）」。當用戶上傳英文的 510(k) 或 MDR 摘要時，系統會調用 gemini-1.5-pro 執行專屬台灣法規體系的雙向語義重組。
功能展現：
2000-3000 字繁體中文高階法規摘要生成：AI 會在 3 秒內自動將英文原始文檔轉譯、重塑並生成完全符合台灣《申報準則》法定格式的「查驗登記申請書摘要文本」（包含：中文品名、英文品名、主要結構與材料、預期用途、作用原理與型號對照）。
強制定用語校正：自動確保所有專業詞彙 100% 採用台灣衛生福利部官方公告用語：
Validation 
 確效（絕非 "驗證"）
Specification 
 規格（絕非 "規範"）
Instructions for Use (IFU) 
 使用說明書（絕非 "用戶說明書"）
Biocompatibility 
 生物相容性（絕非 "生物兼容性"）
Substantial Equivalence (SE) 
 實質等同性（絕非 "實質性等同"）
6. 視覺與互動體驗設計 (Immersive UI/UX & Visualization Specifications)
MED-AGENT PRO 的設計理念在於「科學、效率與無比的高質感」。本系統完全屏棄死板、簡陋的後台管理風格，而是採用專為高強度專業分析設計的 Immersive Command Center（沉浸式控制中心） 視覺風格。
6.1 「太空岩灰 (Space Slate)」與「臨床醫療藍 (Clinical Blue)」主題規範
本系統的配色方案經過精密的色彩對比度計算（符合 WCAG 2.1 AA 無障礙標準），既能減緩法規人員長時間直視螢幕的眼部疲勞，又能提供清晰的視覺層級：
畫布底色 (Canvas Background)：bg-slate-950（極深岩藍灰，用於外圍大背景，呈現深邃穩定的基調）。
HUD 面板填充 (HUD Container)：bg-slate-900 搭配 border-slate-800（高對比卡片網格，提供優秀的內容結構感）。
品牌高亮 / 主功能按鈕 (Focal Highlight)：bg-blue-600 搭配 text-white，滑鼠懸停觸發 hover:bg-blue-500（代表醫療臨床與科技之專業深藍）。
高合規 / 安全狀態色 (Clearance Green)：text-emerald-500 與 bg-emerald-500/10（代表通過審查、合規無虞）。
警告 / 矛盾不一致色 (Warning Amber)：text-amber-500 與 bg-amber-500/10（代表偵測到矛盾，提醒人工確認）。
嚴重缺失 / 法規不符合色 (Failure Rose)：text-rose-500 與 bg-rose-500/10（代表關鍵送審資料缺失，強制補件）。
6.2 6 大互動式 D3/Recharts 數據圖表規格
在系統右側的 Compliance Command Dashboard（合規性戰略控制台） 中，內建了 6 個完全動態更新、支持滑鼠懸停與交互的高端數據圖表：
code
Code
+-----------------------------------------------------------------------------------------+
|                               合規性戰略控制台 (6 大圖表配置)                             |
+-----------------------------------------------------------------------------------------+
| [圖表 1: 八維合規度雷達圖]          [圖表 2: RFI 退件威脅熱力圖]          [圖表 3: 類似品等同拓撲圖]  |
| 反映產品在八大申報維度上的合規    動態網格顯示各送審章節的被退件      基於 D3 Force-Directed 繪製 |
| 百分比，即時響應對話補件進度。    概率，點擊高危格輸出防禦建議。      本品與上市類似品的技術相似度。|
|                                                                                         |
| [圖表 4: 填報進度與信心折線]        [圖表 5: ISO 10993 接觸矩陣圖]       [圖表 6: AI 算力成本遙測儀] |
| 雙軸折線圖，實時繪製 30 題完成    泡泡圖。橫軸接觸時間，縱軸接觸      實時監控 Gemini API 的延遲、  |
| 曲線與 LLM 語義置信度變化。       性質，自動鎖定必須檢附的生物測試。    Token 消耗與算力微觀美元成本。|
+-----------------------------------------------------------------------------------------+
圖表 1：TFDA 申報資料「八維合規度雷達圖」(Eight-Axis Pre-Audit Radar Chart)
技術實現：Recharts <RadarChart> 元件，外網格線設為 stroke="#334155" (slate-700)。
圖表數據指標：
行政法規 (Administrative)
產品等級與分類 (Risk & Class)
用途效能一致性 (Intended Use)
材料主要成分 (Materials & Comp)
電氣安全測試 (IEC 60601)
滅菌壽命確效 (Sterilization)
生物相容性測試 (ISO 10993)
類似品等同性 (SE Comparability)
動態交互：當用戶在左側對話框回答了第 15 題（例如補全了滅菌確效報告編號），雷達圖中的「滅菌壽命確效」頂點會觸發平滑的彈簧過渡動畫（motion spring config: stiffness: 120, damping: 15），從 
 動態拉伸至 
。
圖表 2：RFI 退件機率「威脅等級熱力圖」(RFI Probability Risk Heatmap)
技術實現：D3.js 自定義網格 / Recharts 變體，利用高對比顏色矩陣。
熱力維度：
橫軸 (X-Axis)：送審章節（行政文件、結構規格、安全報告、生物試驗、類似品對照）。
縱軸 (Y-Axis)：審查委員歷史敏感度（高、中、低）。
動態交互：區塊顏色從淺綠（#10b981，代表無風險）到深紅（#ef4444，代表極高退件機率）。滑鼠懸停在紅色區塊時，會彈出 Tooltip 指出：「歷史數據表明，此類別醫療器材因 [生物相容性細胞毒性對比不足] 被要求補件的概率高達 89.2%，請點擊以查看防禦性修補方案。」
圖表 3：類似品等同性「關聯網路拓撲圖」(Substantial Equivalence Predicate Network)
技術實現：基於 D3 力導向圖（Force-Directed Graph）引擎。
節點設計：
中心節點：本申請案 (Your Device)，呈現亮藍色呼吸光源效果。
外圍子節點：市面上已核准之類似品 A、類似品 B、類似品 C（呈現灰色/深藍色節點）。
連線（Edges）：粗細代表「實質等同性相似百分比 (SE Similarity Index)」。
動態交互：點擊類似品 B 節點，拓撲圖會優雅展開，並在 HUD 面板下方彈出 Side-by-Side（雙向規格比對）網格，自動將兩者「主要成分、尺寸、作用機制」進行標色比對（相同標綠，不一致標黃）。
圖表 4：30 道問題填報進度與信心指數「動態雙軸折線圖」(Progress & Confidence Dual-Axis Line Chart)
技術實現：Recharts <AreaChart> 與 <LineChart> 雙軸混合。
雙軸數據：
橫軸 (X-Axis)：30 道追問解答進度（問題 1 到問題 30）。
左縱軸 (Y-Axis Left)：填報完成率（0% 至 100%），以平滑漸變藍色面積圖展示。
右縱軸 (Y-Axis Right)：AI 模型語義信心指數（0.0 至 1.0），以一條亮橘色折線（stroke="#f59e0b"）即時起伏波動，反映用戶給予的答案資料品質。
圖表 5：ISO 10993 人體接觸風險「接觸性質與時間矩陣泡泡圖」(ISO 10993 Contact-Risk Bubble Chart)
技術實現：Recharts <ScatterChart> 散點圖變體。
矩陣維度：
橫軸 (X-Axis)：接觸時間（A：暫時接觸 
、B：中期接觸 
、C：長期接觸 
）。
縱軸 (Y-Axis)：接觸人體性質（皮膚、黏膜、組織/骨、血液循環系統）。
動態交互：當表單自動填寫完畢，系統會在對應象限生成一個帶有動態脈動（Pulsing）特效的藍色泡泡。泡泡的直徑（Size）代表該象限在 ISO 10993-1 標準下，強制要求檢附的生物相容性測試項目總量（如：細胞毒性、致敏、刺激、亞慢性毒性、植入試驗等）。點擊泡泡即會跳出該測試項目的合規查驗清單。
圖表 6：Wow AI 執行效能與 Token 成本「即時遙測 HUD」(Model Telemetry & Cost Analyzer)
技術實現：以 JetBrains Mono 的純代碼風格設計，搭配 30 秒滾動更新的折線圖。
數據指標：
Active Model：gemini-1.5-flash-lite（對話） / gemini-1.5-pro（預審）自動秒級切換。
API Latency：動態毫秒計時（例如：142ms）。
Token Counter：已使用 Input/Output Token 統計。
Real-time Dollar Cost：每次點擊時，即時算出該次請求的算力成本（例如：$0.000185 USD），將「微觀透明化」與未來感拉到極致。
6.3 實時 AI 思考軌跡 (Thoughts Board)
為了將 LLM 調用過程完全「玻璃盒化（Glass-box Architecture）」，側邊欄底部設有專屬的 Thoughts Board 面板，以 font-mono text-xs text-slate-400 bg-slate-950 p-3 rounded 格式，實時打印 Agent 在處理任務時的底層思考鏈（CoT）：
code
Yaml
[SYSTEM] Agentic CoT Initiated. Trigger: 'submission_summary_pasted'
[THOUGHT] Raw summary size: 2408 characters. Running language check.
[THOUGHT] Primary language: Traditional Chinese (Taiwanese dialect variant).
[THOUGHT] Analyzing medical entity. Found key terms: "Pulse Oximeter", "PC shell", "Autoclave".
[THOUGHT] Executing mapping to TFDA classification database.
[THOUGHT] Match found: Main Category [P] Radiology; Sub Category [P.2700] Oximeter. Risk Level: Class II.
[WARNING] Auto-Audit detect: PC shell is sensitive to high temperature (Autoclave > 121C).
[THOUGHT] Flagging structural conflict in DB. Generating warning code MDE-ERR-042.
[THOUGHT] Activating 30-Question Elaboration generator... Focusing on biocompatibility gap.
7. 異常處理、狀態恢復與安全防禦 (Error Recovery & Robust Sentry Mechanisms)
作為一套面向高門檻醫療法規行業的系統，MED-AGENT PRO 設有鋼鐵般的防白屏、防崩潰、防資料丟失機制。
7.1 React 全局渲染防爆盾與 Error Boundary 設計
在 React 18 應用中，若 LLM 返回了損壞、非標準、或意外中斷的 JSON 數據，前端組件在嘗試讀取 data.nested.field 且未作保護時，會導致 React 樹渲染中斷，引發白屏。
防禦手段 A：可選鏈 (Optional Chaining) 與默認 Fallback：
系統前端所有讀取 AI 數據的區塊，強制實施嚴格的防禦性代碼編寫：
const chineseName = apiResponse?.metadata?.chineseName || "（待 AI 提取或手動輸入）";
防禦手段 B：局部 Error Boundary 嵌套：
為 6 大圖表與表單工作區設置獨立的 Error Boundary。當「類似品拓撲圖」因為 D3 力學計算異常發生崩潰時，系統會局部展示一個優雅的「圖表算力校準中 (Telemetry Calibrating...)」琥珀色提示，並提供「手動重設圖表」按鈕，而此時「核心表單」與「AI 協作對話框」完全不受影響，確保業務 100% 連續運作。
防禦手段 C：堅固的後端 JSON 解析器（Defensive JSON Parser Shield）：
在 Express 後端接收到 Gemini 的輸出時，絕不直接調用 JSON.parse，而是通過防爆過濾：
code
TypeScript
export function safeExtractAndParseJSON(rawText: string): Record<string, any> {
  try {
    let cleanText = rawText.trim();
    // 1. 強制移除 Markdown 包裝字元
    if (cleanText.startsWith("```json")) {
      cleanText = cleanText.substring(7);
    } else if (cleanText.startsWith("```")) {
      cleanText = cleanText.substring(3);
    }
    if (cleanText.endsWith("```")) {
      cleanText = cleanText.substring(0, cleanText.length - 3);
    }
    cleanText = cleanText.trim();
    
    // 2. 進行安全解析
    return JSON.parse(cleanText);
  } catch (error) {
    console.error("Gemini JSON formatting anomaly detected. Executing regex extraction fallback.", error);
    // 3. 備用正則提取器：逐個欄位強制檢索，確保前端不白屏
    return executeRegexFallbackParser(rawText);
  }
}
7.2 離線防丟失緩存哨兵 (LocalStorage Sentry)
5 秒自動存檔機制：系統在前端背景運行一個節流（Throttling）計時器。當用戶輸入表單、填寫 30 道問題的任何欄位發生變化時，系統會在 5 秒內將當前應用的完整狀態（Redux / State tree）進行序列化，寫入瀏覽器的 localStorage 中。
災難恢復提示：當發生突然斷電、誤觸瀏覽器重新整理（F5）或瀏覽器崩潰時，用戶再次打開網頁，系統會流暢地從快取中恢復，並在頂部彈出一個精美的小型 Motion 通知：「已為您恢復至民國 115/06/30 20:09:52 的未完工送審現場（表單資料已自動備份）」。
8. 保真多格式導出管線 (High-Fidelity Export Pipeline)
系統提供完美的「一次輸入，多端保真導出」引擎，所有導出格式皆不損害資料的完整性，並針對打印排版進行了極度優化。
JSON 結構化格式：導出標準、精簡且符合 TFDA 資料字典規範的結構化 JSON。該 JSON 可直接用於與外部藥商 ERP 系統對接，或作為未來政府電子化送件平台（E-submission Portal）的導入格式。
Markdown (2000-3000 字繁體中文高階法規報告)：導出一份語義嚴謹、格式漂亮的產品送審精華摘要。包含：
產品概覽與規格對照表
依據《申報準則》條款判定之合規性分析
臨床前測試（ISO 10993/IEC 60601）符合性矩陣
HTML 單檔案離線範本：導出包含內聯 CSS 樣式的單網頁 HTML，完整還原 TFDA 官方紙本「查驗登記申請書（第二、三等級醫療器材）」的網格排版，方便用戶離線開啟。
PDF (A4 打印高度保真優化)：
痛點：網頁直接打印時，常因側邊欄、對話框與按鈕未被遮蔽，且 Form 網格斷行截斷，導致輸出的 PDF 極其雜亂。
排版優化機制：系統利用 CSS Paged Media 規範進行專屬打印樣式封裝：
code
CSS
@media print {
  /* 1. 隱藏所有 UI 控制元件、對話側邊欄、HUD 儀表板 */
  .no-print, #ai-sidebar, #dashboard-panel, .btn-group {
    display: none !important;
  }
  /* 2. 重置主表單寬度與背景色，適配 A4 尺寸 */
  body, #main-form-container {
    background: #ffffff !important;
    color: #000000 !important;
    width: 210mm;
    margin: 0;
    padding: 15mm;
  }
  /* 3. 強制表格跨頁斷行優化，防止文字中途被截斷 */
  tr, .form-section {
    page-break-inside: avoid;
  }
}
9. 20 個深度架構與法規追問 (20 Comprehensive Follow-up Questions)
為了使 MED-AGENT PRO 系統設計能夠完美落地並達到真正的商業級別標準，請進一步思考並針對以下 20 個關乎「技術、美學、法規、安全與邊界」的核心追問進行深度的技術與業務探討：
9.1 台灣 TFDA 法規與表單邏輯維度 (Taiwan TFDA & Regulatory Logic)
問題 1（替代條款深度校核）：在處理《申報準則》第六條「免附第十七項資料（替代條款）」時，若用戶點選「適用」，AI 是否應主動提示並審查其前提條件（例如：是否確屬進口醫材、是否符合原出產國醫療器材主管機關出具之證明文件），並強制用戶上傳第十六項資料（出產國許可製售證明）以利交叉確效？
問題 2（說明書與包裝審查）：TFDA 在實務審查時，對「中文說明書、包裝標籤、說明書擬稿」的要求極高。我們是否應將系統擴充，允許用戶上傳說明書拟稿（PDF 格式），由 gemini-1.5-pro 進行標籤合規性掃描（如：字體大小、警告用語、警示符號），並比對其與主表單「中文品名、英文品名、預期用途」是否一致？
問題 3（定時法規庫更新）：TFDA 的醫療器材分類分級代碼與《醫療器材管理法》子法時有修訂。後端是否需要設計一組 Cron Job 定期與台灣衛生福利部食品藥物管理署（TFDA）的「開放資料平台 (MOHW Open Data API)」進行增量同步，以動態更新主、次類別代碼庫？
問題 4（陸輸醫材加嚴規範）：對於產地為「陸輸（中國大陸製造）」的輸入醫療器材，台灣法規通常設有極其特殊的輸入專案許可限制。系統在偵測到產地為陸輸時，是否應在預審雷達中自動啟動「兩岸醫療器材管制加嚴警示」，提醒用戶備妥額外的兩岸核准比對文件？
問題 5（製造廠類型與 QSD）：製造廠資訊包含「委託製造、受託製造、分裝、包裝」等多種形式。當用戶勾選「委託製造」時，系統表單是否應動態遞歸展開多個子製造商欄位，並引導用戶填寫對應的「國外製造廠 QSD 登錄編號」與有效期限，實時計算其 QSD 效期是否已屆滿？
9.2 Agent 智能管道與 Q&A 互動維度 (Agent Cognitive & Q&A Mechanics)
問題 6（30 道提問的生成節奏）：那 30 道引導追問是應該在用戶剛上傳 Summary 後**「一次性全部生成（List 視圖）」，以便用戶統籌分配給研發或法規同仁回答；還是採用「漸進式交互生成（Dynamic Step-by-Step）」**，根據用戶前一個問題的回答，動態調整甚至削減後續不相關的問題？
問題 7（問卷協同分發機制）：當 RA 人員面對 30 道問題中某些高度技術性的問題（例如電磁相容 EMC 的頻譜數據、無線電發射功率）回答「不清楚，需研發團隊解答」時，系統是否應提供「一鍵生成子問卷外鏈」或「導出 Markdown 技術確認單」功能，以便進行跨部門協作？
問題 8（AI 幻覺控制與信心閾值）：為了杜絕 gemini-1.5-flash-lite 在自動填表時可能發生的語義幻覺，我們是否應在前端每個 input 框右側設置一個微型的「AI 信心度指示燈」？當信心度（Confidence Score）低於 75% 時，指示燈呈現黃色，並強制用戶雙擊該欄位進行人工審核確認，方可解鎖後續的導出按鈕。
問題 9（邏輯衝突的人工覆寫）：當系統的紅隊審查偵測到材料與滅菌方式衝突（如：PC 塑料不耐蒸氣高壓滅菌）並發出警告，但用戶在研發上確實使用了某種改性 PC（耐高溫達 140°C）時，系統是否應允許用戶點擊「人工覆寫（Override & White-list）」，並強制用戶輸入佐證理由，隨後自動將此理由寫入導出材料的「備註技術說明」中？
9.3 視覺主題與 Wow 級互動維度 (Visual Theme & Wow Factor)
問題 10（高飽和度色彩疲勞度）：雖然「太空岩灰（Space Slate）」的暗色調 HUD 具備極強的現代科技感，但考慮到 RA 人員常需要在白色背景的官方文件與灰色系統介面間來回切換，我們是否應增設一個「高對比紙張白（Clinical Paper White）」明亮主題開關，並確保兩種主題皆具備 AAA 級的色彩對比防疲勞設計？
問題 11（拓撲圖節念剪枝算法）：在「類似品關聯拓撲圖」中，如果市面上類似品數量極為龐大（如超過 50 個類似品許可證），D3 力導向圖會因節點過多而產生視覺雜亂與渲染卡頓。我們是否應在前端設計一個「相似度 K-Core 剪枝過濾器」，默認僅展示相似度高於 80% 的前 5 到 7 個核心類似品節點？
問題 12（思考軌跡面板的折疊哲學）：Thoughts Board 的實時內部思維鏈展示雖然能極大地提高系統的 Wow 特色，但對於非技術背景的普通 RA 主管來說，龐大的 Token 消耗日誌與 API 響應時間可能造成視覺干擾。我們是否應默認將 Thoughts Board 設為側邊欄底部半折疊狀態，僅保留代表 AI 正在思考的「思維脈衝呼吸波（Pulsing wave）」？
問題 13（合規突破驚喜動畫）：當用戶解答完所有問題，八維雷達圖的合規分數首次突破 90% 大關時，介面是否應引入一個 Wow 特效，如表單外框閃爍一圈象徵高品質的「金色流光粒子（Golden Aura Effect）」，以提供用戶極致的成就感與完工心理反饋？
9.4 系統安全、沙箱限制與下載維度 (Security & Iframe Sandboxing)
問題 14（Iframe 沙箱環境下載防禦）：由於本系統預設在 AI Studio 的 Iframe 沙箱環境中運行，瀏覽器對於 iframe 內觸發 window.open 或直接觸發 File Blob 下載常設有嚴格的安全性限制。我們應如何精準配置 download 屬性或使用「Base64 數據鏈轉移技術」，確保用戶點擊「導出 PDF/JSON/MD」時 100% 順暢下載而不被沙箱攔截？
問題 15（多模態圖片規格提取）：考慮到很多醫療器材的型號規格散落在產品外觀圖、專利結構圖等設計圖紙中。我們是否應該利用 Gemini 1.5 Pro 的多模態圖像理解能力，允許用戶上傳技術設計草圖（JPEG/PNG 格式），由 Agent 自動提取並在「型號、規格或主要成分」欄位中填寫自動描述？
問題 16（Esbuild 單檔案冷啟動性能）：後端使用 esbuild 編譯 TypeScript 成為單一 CJS 檔案時，針對 @google/genai 這個涉及原生網路協議與潛在外部依賴的 SDK，我們應如何配置 external 排它性依賴？在 Docker 容器冷啟動時，如何確保 Server-Side Node 的啟動延遲控制在毫秒級別？
問題 17（Wasm 本地備用模式）：為了在極度敏感的保密環境下（例如軍工醫療器材研發）運作，若用戶切換至「完全本機模式（Offline Sentry Mode）」，我們是否應在瀏覽器端載入一個輕量化的 WebAssembly 法規比對引擎，確保核心產品文字不流出本機，依然能完成基本的表單校對？
9.5 格式導出與官方系統對接維度 (Export Pipeline & official alignment)
問題 18（A4 PDF 跨頁斷行完美適配）：在導出 PDF 時，不同瀏覽器的 PDF 渲染引擎（如 Chrome 的 Print-to-PDF、Firefox 打印引擎）對 CSS @page 和 break-inside: avoid 的支持存在細微差異。我們是否應在前端加入一個「PDF 排版擬真預覽抽屜（Print Preview Drawer）」，讓用戶能在打印前直觀調整分頁符號（Page Break）的位置？
問題 19（國際法規對照雙向生成）：在 Wow Feature C 導出 2000-3000 字繁體中文高階摘要時，是否能同時在摘要末尾附加一張「台灣 TFDA ↔ 美國 FDA 510(k) ↔ 歐盟 MDR」三方核心法規申報項目對照字典？這對計畫進行全球多中心上市申報的跨國醫療器材商而言，將是無比巨大的 Wow 賣點。
問題 20（工研院/食藥署電子格式 1:1 對標）：台灣工研院及食藥署目前的「醫療器材電子送件系統（e-Submission Portal）」對導入的 XML/JSON 設有極其嚴苛的 DTD（文件類型定義）校驗。系統導出的 JSON 結構，是否需要與官方公開的 XSD/JSON Schema 字典進行 1:1 的精準鍵值映射對標，以達到真正的「官方免修改、直裝直入」？
10. 總結 (Summary)
MED-AGENT PRO 的設計藍圖是一場醫療器材合規申報流程的效率革命。本技術規格說明書透過：
極致美學的太空岩灰 UI 與 6 大互動式 Recharts/D3 圖表，打造高掌控力的法規戰略控制台。
Gemini 雙引擎驅動 的主動式 AI 代理人，實現 100% 語義提取與零摩擦自動填表。
主動式 30 道提問與紅隊邏輯衝突審查，在送審前精準過濾掉所有技術與行政漏洞，顯著降低退件補件率。
三大 Wow 特色功能：國際標準交叉索引、退件率預測熱力圖、台灣食藥署專用語義翻譯引擎，完美對標台灣《醫療器材許可證核發與登錄及年度申報準則》。
鋼鐵般的安全防護與異常處理：React 局部防爆盾、備用正則解析器與 LocalStorage 存檔哨兵，100% 杜絕執行期白屏崩潰。
本規格書已為系統的實施奠定了極其深厚、專業且高工業強度的架構基礎。MED-AGENT PRO 的誕生，將完美實踐法規工程與人工智能的優雅交織。
