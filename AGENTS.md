# AGENTS.md — 專案開發與 AI 協作指南

這份文件是我為這個專案（AI Knowledge Hub）寫的開發規範與 AI 協作指南。當使用 Claude、ChatGPT 等 AI 工具幫忙新增頁面或修改程式碼時，AI 必須參考並遵守這裡訂好的結構與視覺規則。

---

## 1. 專案簡介與設計構想 (Project Scope)

- **專案簡介**：這是一個用來記錄個人 AI 學習筆記、Prompt 技巧與 Vibe Coding 實務經驗的靜態知識庫網站。
- **整體風格與想法**：
  - **拒絕 AI 罐頭感**：網頁避免使用 Emoji 或過度的螢光發光特效，整體希望能呈現像現代線上科技雜誌一樣乾淨、質感的排版。
  - **免建置環境、好部署**：採用純 HTML5 配合 Tailwind CSS CDN，這樣不需要安裝複雜的 Node.js 環境，寫完可以直接推上 GitHub Pages 預覽。
  - **注重細節與動態體驗**：卡片要有平滑的浮動與圖片微放大效果，讓畫面不會死板。

---

## 2. 專案目錄結構 (Directory Structure)

```text
/
├── index.html           # 網站首頁（放 Hero 介紹區塊與文章卡片列表）
├── AGENTS.md            # 本專案的 AI 開發規範文件
├── pages/               # 所有筆記與文章頁面
│   ├── vibe-coding.html # 文章 1：Vibe Coding 實踐指南
│   ├── agents-guide.html# 文章 2：如何設計高效率 AGENTS.md
│   └── prompt-guide.html# 文章 3：提示詞工程指南
└── assets/              # 靜態圖片與資源
    └── images/
```

---

## 3. 視覺設計與 UI 規範 (Design System)

AI 在幫忙生成新頁面或調整樣式時，請直接套用下面這套視覺設定：

### 3.1 色彩搭配 (Color Palette)

- **主背景色**：`bg-[#fcfcfd]`
- **深色區塊 (頂部選單 / Hero 區 / 頁腳)**：`bg-slate-950` (`#020617`) 搭配 `backdrop-blur-md` 玻璃擬態與 `border-white/10` 邊框
- **文字顏色**：內文用 `text-slate-800`，主要標題用 `text-slate-900`，次要說明用 `text-slate-600`
- **點綴色 (Primary Accent)**：`emerald-400` / `emerald-500`，用於 Logo 點綴與重點文字

### 3.2 字型排版 (Typography)

- **內文與數字字型**：`Plus Jakarta Sans` (`font-sans`)
- **標題襯線字型**：`Playfair Display` (`font-serif italic`)，用於 Hero 標題關鍵字增添人文感
- **程式碼與標籤字型**：`font-mono`

### 3.3 品牌 Logo 格式 (Branding)

- **Logo Icon**：統一採用深底白框的 `KH` Monogram 方塊標記（**請勿使用 🤖 機器人圖示**）：
  ```html
  <span
    class="w-8 h-8 rounded-lg bg-white/10 border border-white/20 flex items-center justify-center font-mono font-bold text-sm tracking-tighter"
    >KH</span
  >
  <span class="text-base font-semibold tracking-wide text-slate-100"
    >Knowledge Hub<span class="text-emerald-400">.</span></span
  >
  ```

### 3.4 分類 Badges 樣式

文章卡片與頁面頂部的分類標籤，統一採用深色半透明背景搭 `font-mono` 字體：

- **Vibe Coding 分類**：`bg-slate-900/80 text-emerald-400 text-[11px] font-mono px-2.5 py-1 rounded-md border border-white/10`
- **Agent 規範分類**：`bg-slate-900/80 text-blue-400 text-[11px] font-mono px-2.5 py-1 rounded-md border border-white/10`
- **Prompt 工程分類**：`bg-slate-900/80 text-purple-400 text-[11px] font-mono px-2.5 py-1 rounded-md border border-white/10`

---

## 4. 新文章 HTML 標準模板 (Subpage Architecture)

新增任何 `pages/*.html` 頁面時，必須套用以下與首頁 `index.html` 視覺完全一致的模板：

```html
<!DOCTYPE html>
<html lang="zh-TW" class="scroll-smooth">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{{文章標題}} — Knowledge Hub</title>
    <!-- Google Fonts -->
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,600;1,400&family=Plus+Jakarta+Sans:wght@400;500;600;700&display=swap"
      rel="stylesheet"
    />
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
      tailwind.config = {
        theme: {
          extend: {
            fontFamily: {
              sans: ['"Plus Jakarta Sans"', "system-ui", "sans-serif"],
              serif: ['"Playfair Display"', "Georgia", "serif"],
            },
          },
        },
      };
    </script>
  </head>
  <body
    class="bg-[#fcfcfd] text-slate-800 font-sans flex flex-col min-h-screen antialiased selection:bg-slate-900 selection:text-white"
  >
    <!-- 頂部導覽列 -->
    <header
      class="sticky top-0 z-50 bg-slate-950/85 backdrop-blur-md border-b border-white/10 text-white"
    >
      <div
        class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 h-16 flex items-center justify-between"
      >
        <a href="../index.html" class="flex items-center gap-3 group">
          <span
            class="w-8 h-8 rounded-lg bg-white/10 border border-white/20 flex items-center justify-center font-mono font-bold text-sm tracking-tighter group-hover:bg-white group-hover:text-slate-950 transition duration-300"
            >KH</span
          >
          <span class="text-base font-semibold tracking-wide text-slate-100"
            >Knowledge Hub<span class="text-emerald-400">.</span></span
          >
        </a>
        <nav class="flex items-center gap-6 text-sm font-medium">
          <a
            href="../index.html"
            class="text-slate-400 hover:text-white transition flex items-center gap-1"
          >
            ← 返回文章列表
          </a>
        </nav>
      </div>
    </header>

    <!-- 主要內容區 -->
    <main class="flex-grow max-w-4xl mx-auto w-full px-4 sm:px-6 py-12">
      <!-- 文章標頭區塊 -->
      <div class="mb-10 border-b border-slate-200 pb-8">
        <div class="flex items-center gap-3 mb-4">
          <span
            class="bg-slate-900 text-emerald-400 text-xs font-mono px-3 py-1 rounded-md border border-slate-800"
            >{{分類標籤}}</span
          >
          <span class="text-xs font-mono text-slate-400"
            >{{更新日期}} • {{預估閱讀時間}}</span
          >
        </div>
        <h1
          class="text-3xl sm:text-5xl font-bold text-slate-900 tracking-tight leading-tight mb-4"
        >
          {{文章標題}}
        </h1>
        <p class="text-slate-600 text-lg leading-relaxed">{{文章摘要簡述}}</p>
      </div>

      <!-- 文章內文區塊 -->
      <article
        class="prose max-w-none text-slate-700 leading-relaxed space-y-6"
      >
        <!-- 內文放置於此 -->
      </article>
    </main>

    <!-- 頁腳 -->
    <footer
      class="bg-slate-950 text-slate-400 py-10 border-t border-slate-800 mt-auto"
    >
      <div
        class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col sm:flex-row items-center justify-between gap-4 text-xs"
      >
        <div class="flex items-center gap-3">
          <span
            class="w-5 h-5 rounded bg-emerald-500/20 text-emerald-400 flex items-center justify-center font-mono font-bold text-[10px]"
            >KH</span
          >
          <span>Designed under AGENTS.md framework</span>
        </div>
        <p>&copy; 2026 Knowledge Hub. All rights reserved.</p>
      </div>
    </footer>
  </body>
</html>
```

---

## 5. AI 新增內容與修改時的注意事項 (Rules for AI)

當指示 AI 幫忙新增頁面或修網頁時，請提醒 AI 遵守以下規則：

1. **檔案命名要乾淨**：
   - 新頁面一律放在 `pages/` 資料夾內。
   - 檔名統一用英文小寫跟連字號（Kebab-case），例如 `pages/prompt-engineering.html`。

2. **記得自動更新首頁 (index.html)**：
   - 每當在 `pages/` 裡面新增了一篇新文章，**AI 必須同步更新 `index.html`** 的文章卡片網格。
   - 新卡片要包含符合主題的 Unsplash 縮圖、正確顏色的分類 Badge、日期、摘要與點擊連結。

3. **路徑一定要用相對路徑**：
   - `pages/` 資料夾裡的 HTML 檔，要連回首頁或引用資源時，請務必寫相對路徑 `../index.html`。
   - 這樣丟上 GitHub Pages 部署時，連結才不會失效出錯。

4. **確保響應式體驗 (RWD)**：
   - 無論手機版或電腦版都要經過測試，確保文字大小、邊距與欄位數量（1 欄 / 2 欄 / 3 欄）能順暢切換。
