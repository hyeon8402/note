<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>태그 노트 계층형 (클라우드 완결판)</title>
  <style>
    :root {
      --bg:#101010;
      --header:#1b1b1b;
      --card:#171717;
      --card2:#202020;
      --input:#141414;
      --text:#f5f5f5;
      --sub:#b6b6b6;
      --line:#3a3a3a;
      --main:#2563eb;
      --danger:#dc2626;
      --star:#f59e0b;
      --ok:#16a34a;
      --edit:#7c3aed;
    }

    * { box-sizing:border-box; }

    body {
      margin:0;
      background:var(--bg);
      color:var(--text);
      font-family:system-ui,-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif;
      padding-bottom:150px;
      -webkit-tap-highlight-color:transparent;
    }

    header {
      background:var(--header);
      padding:18px 16px;
      border-bottom:1px solid #242424;
      position:sticky;
      top:0;
      z-index:10;
    }

    .titleRow {
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
    }

    h1 {
      font-size:23px;
      margin:0;
      display:flex;
      align-items:center;
      gap:8px;
    }

    h2 {
      margin:0 0 12px;
      font-size:19px;
    }

    .moon {
      font-size:24px;
      opacity:.85;
    }

    main {
      max-width:900px;
      margin:0 auto;
      padding:18px 12px 0;
    }

    .card {
      background:var(--card);
      border:1px solid var(--line);
      border-radius:22px;
      padding:14px;
      margin:12px 0;
    }

    .desc {
      color:var(--sub);
      margin:5px 0 0;
      font-size:14px;
      line-height:1.5;
    }

    input, select, textarea, button, label.fileBtn {
      width:100%;
      border:1px solid var(--line);
      border-radius:14px;
      padding:13px;
      font-size:16px;
    }

    input, select, textarea {
      background:var(--input);
      color:var(--text);
      outline:none;
      margin-bottom:9px;
    }

    textarea {
      min-height:150px;
      resize:vertical;
    }

    input:focus, select:focus, textarea:focus {
      border-color:var(--main);
    }

    button, label.fileBtn {
      background:var(--main);
      color:white;
      border:0;
      font-weight:800;
      margin:4px 0;
      display:block;
      text-align:center;
    }

    .searchBox {
      border:2px solid #5a5a5a;
      border-radius:22px;
      padding:8px 10px;
      position:relative;
      background:var(--input);
      margin-bottom:18px;
    }

    .searchLabel {
      color:var(--sub);
      font-size:14px;
      padding:0 6px 3px;
    }

    .searchBox input {
      border:0;
      background:transparent;
      margin:0;
      padding:6px 44px 8px 6px;
      font-size:20px;
      font-weight:700;
    }

    .clearSearchBtn {
      position:absolute;
      right:12px;
      top:50%;
      transform:translateY(-50%);
      width:34px;
      height:34px;
      padding:0;
      margin:0;
      border-radius:50%;
      background:#333;
      color:#fff;
      display:none;
      align-items:center;
      justify-content:center;
      font-size:22px;
      line-height:1;
      border:1px solid #555;
    }

    .clearSearchBtn.show { display:flex; }

    .selectBlock {
      background:#151515;
      border-radius:20px;
      padding:12px;
      margin-top:12px;
    }

    .selectBlock label {
      display:block;
      color:#d1d1d1;
      font-size:14px;
      margin:0 0 4px 4px;
    }

    .selectBlock select {
      background:#3a3a3a;
      border:0;
      font-weight:800;
      font-size:18px;
      padding:10px 8px;
      margin:0;
    }

    .compactSelectBlock {
      margin-top:0;
      margin-bottom:9px;
    }

    .modalSelectBtn {
      background:#3a3a3a;
      border:1px solid #4a4a4a;
      color:#fff;
      font-weight:900;
      font-size:18px;
      padding:12px 42px 12px 12px;
      margin:0;
      text-align:left;
      position:relative;
    }

    .modalSelectBtn::after {
      content:"⌄";
      position:absolute;
      right:15px;
      top:50%;
      transform:translateY(-50%);
      color:#d0d0d0;
      font-size:20px;
    }

    .inlineDropdown {
      max-height: 0;
      overflow: hidden;
      opacity: 0;
      background: #161616;
      border: 1px solid transparent;
      border-radius: 16px;
      padding: 0 10px;
      box-shadow: inset 0 2px 8px rgba(0,0,0,0.5);
      transition: max-height 0.28s cubic-bezier(0.4, 0, 0.2, 1), 
                  opacity 0.2s ease, 
                  padding 0.28s ease, 
                  margin 0.28s ease;
    }

    .inlineDropdown.show {
      max-height: 320px;
      overflow-y: auto;
      opacity: 1;
      border-color: #3a3a3a;
      padding: 8px 10px;
      margin-top: 6px;
      margin-bottom: 12px;
    }

    .modalOptionBtn {
      display:flex;
      align-items:center;
      justify-content:space-between;
      width:100%;
      background:#232323;
      border:1px solid #3a3a3a;
      border-radius:16px;
      padding:13px 14px;
      margin:7px 0;
      color:#fff;
      font-size:17px;
      font-weight:850;
      text-align:left;
    }

    .modalOptionBtn.active {
      background:#12337e;
      border-color:#4f83ff;
    }

    .modalOptionBtn .checkMark {
      color:#9fc0ff;
      font-weight:900;
      margin-left:10px;
    }

    .selectHelp {
      color:#cfcfcf;
      font-size:14px;
      margin:8px 4px 0;
      line-height:1.45;
    }

    .countBox {
      display:grid;
      grid-template-columns:repeat(3,1fr);
      gap:8px;
      margin-top:12px;
    }

    .countItem {
      background:#0d0d0d;
      border:1px solid var(--line);
      border-radius:16px;
      padding:10px;
      text-align:center;
    }

    .countNum {
      font-weight:900;
      font-size:20px;
    }

    .countLabel {
      font-size:12px;
      color:var(--sub);
      margin-top:2px;
    }

    .tag {
      background:#101010;
      border:1px solid var(--line);
      border-radius:18px;
      padding:14px;
      margin:10px 0;
      transition:.12s;
    }

    .tag.selected {
      background:linear-gradient(180deg, #12337e, #0e1d47);
      border-color:#4f83ff;
      box-shadow:0 0 0 2px rgba(37,99,235,.18) inset;
    }

    .tag.suspect {
      border-color:#f59e0b;
      box-shadow:0 0 0 2px rgba(245,158,11,.12) inset;
    }

    .suspectBox {
      margin-top:8px;
      background:#3b2605;
      border:1px solid #9a6a12;
      color:#ffe7aa;
      border-radius:14px;
      padding:9px 10px;
      font-size:13px;
      line-height:1.45;
      font-weight:800;
    }

    .tagName {
      font-size:19px;
      font-weight:900;
      word-break:break-word;
    }

    .tagPath {
      margin-top:8px;
      color:#bcbcbc;
      font-size:14px;
      line-height:1.45;
      font-weight:700;
    }

    .tagDesc {
      margin-top:8px;
      color:#e3e3e3;
      font-size:15px;
      line-height:1.55;
    }

    .tagKeywords {
      margin-top:8px;
      color:#aaa;
      font-size:14px;
      line-height:1.45;
    }

    .tagBtns {
      display:grid;
      grid-template-columns:1fr 54px 54px 54px;
      gap:8px;
      margin-top:12px;
    }

    .tagBtns button {
      padding:10px 6px;
      margin:0;
      font-size:14px;
    }

    .copyBtn { background:var(--main); }
    .edit { background:var(--edit); }
    .selectBtn { background:var(--star); color:#111; }
    .del { background:var(--danger); }

    .editBox {
      max-height: 0;
      overflow: hidden;
      opacity: 0;
      background: #171717;
      border: 1px solid transparent;
      border-radius: 16px;
      padding: 0 12px;
      margin-top: 0;
      transition: max-height 0.32s cubic-bezier(0.4, 0, 0.2, 1),
                  opacity 0.25s ease,
                  padding 0.32s ease,
                  margin 0.32s ease;
    }

    .editBox.active {
      max-height: 620px;
      opacity: 1;
      border-color: var(--line);
      padding: 12px;
      margin-top: 12px;
    }

    .editActions {
      display:grid;
      grid-template-columns:1fr 1fr;
      gap:8px;
    }

    .ok { background:var(--ok); }
    .danger { background:var(--danger); }
    .ghost {
      background:#262626;
      border:1px solid var(--line);
      color:#fff;
    }

    .quickHelp {
      display:grid;
      gap:6px;
      margin-top:10px;
      color:#bdbdbd;
      font-size:14px;
      line-height:1.45;
    }

    nav {
      position:fixed;
      left:0;
      right:0;
      bottom:0;
      z-index:20;
      background:#111;
      border-top:1px solid var(--line);
      display:grid;
      grid-template-columns:repeat(5,1fr);
      gap:5px;
      padding:8px;
    }

    nav button {
      margin:0;
      background:#303030;
      border:1px solid #444;
      border-radius:14px;
      padding:11px 2px;
      font-size:13px;
    }

    nav button.active { background:var(--main); }

    .selectionBar {
      position:fixed;
      left:8px;
      right:8px;
      bottom:69px;
      z-index:25;
      background:#0a0a0a;
      border:1px solid #4a4a4a;
      border-radius:18px;
      padding:8px;
      display:none;
      grid-template-columns:1fr 1fr 1fr;
      gap:7px;
      box-shadow:0 10px 28px rgba(0,0,0,.5);
    }

    .selectionBar.show { display:grid; }

    .selectionBar button {
      margin:0;
      padding:10px 6px;
      border-radius:12px;
      font-size:14px;
    }

    .selectionCount {
      grid-column:1 / -1;
      color:#fff;
      text-align:center;
      font-weight:900;
      padding:3px 0 5px;
    }

    .page { display:none; }
    .page.active { display:block; }

    .toast {
      position:fixed;
      left:50%;
      bottom:138px;
      transform:translateX(-50%);
      background:#f5f5f5;
      color:#050505;
      padding:10px 14px;
      border-radius:999px;
      opacity:0;
      transition:.2s;
      z-index:30;
      white-space:nowrap;
      font-weight:900;
      max-width:92vw;
      overflow:hidden;
      text-overflow:ellipsis;
    }

    .toast.show { opacity:1; }
    .hideFile { display:none; }

    .miniHelp {
      background:#0c0c0c;
      border:1px solid var(--line);
      border-radius:16px;
      padding:11px;
      margin-top:10px;
      color:var(--sub);
      line-height:1.5;
      font-size:14px;
    }

    @media (min-width:760px) {
      #tagList {
        display:grid;
        grid-template-columns:1fr 1fr;
        gap:10px;
      }
      .tag { margin:0; }
    }
  </style>
</head>
<body>
<header>
  <div class="titleRow">
    <h1>🛠️ 태그 노트</h1>
    <div class="moon">🌙</div>
  </div>
</header>

<main>
  <section id="pageSearch" class="page active">
    <div class="card">
      <div class="searchBox">
        <div class="searchLabel">태그 검색</div>
        <input id="searchInput" placeholder="검색어를 입력하세요..." />
        <button id="clearSearchBtn" class="clearSearchBtn" type="button">×</button>
      </div>

      <div class="selectBlock">
        <label>대분류</label>
        <button id="mainCategorySelectBtn" class="modalSelectBtn" type="button">전체</button>
        <select id="mainCategorySelect" style="display:none"></select>
      </div>
      <div id="mainHelp" class="selectHelp"></div>

      <div class="selectBlock">
        <label>소분류</label>
        <button id="subCategorySelectBtn" class="modalSelectBtn" type="button">전체</button>
        <select id="subCategorySelect" style="display:none"></select>
      </div>

      <div class="countBox">
        <div class="countItem"><div id="totalCount" class="countNum">0</div><div class="countLabel">전체</div></div>
        <div class="countItem"><div id="shownCount" class="countNum">0</div><div class="countLabel">표시</div></div>
        <div class="countItem"><div class="countNum">DB</div><div class="countLabel">IndexedDB</div></div>
      </div>

      <div class="quickHelp">
        <div>• 복사 버튼: 태그 하나 복사</div>
        <div>• 수정 버튼: 태그 내용 수정</div>
        <div>• 선택 버튼: 여러 태그 모아서 복사</div>
      </div>
    </div>

    <div class="card">
      <div id="tagList"></div>
    </div>
  </section>

  <section id="pageAdd" class="page">
    <div class="card">
      <h2>태그 추가</h2>
      <input id="tagInput" placeholder="태그 예: sitting" />
      <input id="krInput" placeholder="한글 이름 예: 앉아있음" />
      <div class="selectBlock compactSelectBlock">
        <label>대분류</label>
        <button id="addMainCategoryBtn" class="modalSelectBtn" type="button">이미지 구성</button>
        <select id="addMainCategory" style="display:none"></select>
      </div>
      <div class="selectBlock compactSelectBlock">
        <label>소분류</label>
        <button id="addSubCategoryBtn" class="modalSelectBtn" type="button">품질 및 해상도</button>
        <select id="addSubCategory" style="display:none"></select>
      </div>
      <textarea id="descInput" placeholder="설명"></textarea>
      <input id="keywordsInput" placeholder="키워드 예: 앉은 자세, 앉기, 착석" />
      <button id="saveTagBtn">저장</button>
    </div>
  </section>

  <section id="pageBulk" class="page">
    <div class="card">
      <h2>대량 추가</h2>
      <p class="desc">선택한 대분류/소분류에 여러 태그를 한 번에 저장합니다.</p>
      <div class="selectBlock compactSelectBlock">
        <label>대분류</label>
        <button id="bulkMainCategoryBtn" class="modalSelectBtn" type="button">이미지 구성</button>
        <select id="bulkMainCategory" style="display:none"></select>
      </div>
      <div class="selectBlock compactSelectBlock">
        <label>소분류</label>
        <button id="bulkSubCategoryBtn" class="modalSelectBtn" type="button">품질 및 해상도</button>
        <select id="bulkSubCategory" style="display:none"></select>
      </div>
      <textarea id="bulkText" placeholder="sitting : (앉아있음) 의자나 표면에 앉아 있는 자세&#10;standing : (서있음) 서 있는 자세"></textarea>
      <button id="bulkAddBtn" class="ok">대량 저장</button>
    </div>
  </section>

  <section id="pageBackup" class="page">
    <div class="card">
      <h2>로컬 백업 / 복원</h2>
      <p class="desc">오프라인용 JSON 백업 수동 관리 메뉴입니다.</p>
      <button id="exportBtn">JSON 백업 저장</button>
      <label class="fileBtn">JSON 불러오기<input id="importFile" class="hideFile" type="file" accept=".json"></label>
      <button id="resetBtn" class="danger">전체 삭제</button>
    </div>

    <div class="card">
      <h2>☁️ 실시간 클라우드 동기화 (GitHub Gist)</h2>
      <p class="desc">인터넷 창고 보안을 유지하기 위해, 아래에 열쇠 정보를 입력하고 각인해 주세요. (내 폰 브라우저에만 기억됩니다)</p>
      <input id="cloudToken" type="password" placeholder="GitHub Personal Access Token (ghp_...) 입력" style="margin-top:10px;" />
      <input id="cloudGistId" placeholder="본인의 Gist ID 입력" />
      <button id="saveCfgBtn" class="ok" style="background:var(--ok); font-weight:bold; margin-bottom:12px;">🔑 주소 및 열쇠 내 폰에 기억시키기</button>
      
      <div class="editActions" style="border-top:1px solid #333; padding-top:12px;">
        <button id="cloudUploadBtn" class="ok">클라우드로 업로드</button>
        <button id="cloudDownloadBtn" class="copyBtn" style="margin-top:6px;">클라우드에서 불러오기</button>
      </div>
    </div>
  </section>

  <section id="pageHelp" class="page">
    <div class="card">
      <h2>사용법</h2>
      <p class="desc">복사 버튼을 누르면 태그 하나가 바로 복사됩니다.</p>
      <p class="desc">수정 버튼을 누르면 수정창이 열립니다.</p>
      <p class="desc">선택 버튼을 누르면 여러 태그를 모아서 아래 선택바에서 한 번에 복사할 수 있습니다.</p>
    </div>
  </section>
</main>

<div id="selectionBar" class="selectionBar">
  <div id="selectionCount" class="selectionCount">선택 0개</div>
  <button id="copySelectedBtn">선택 복사</button>
  <button id="copySelectedLineBtn">줄복사</button>
  <button id="clearSelectedBtn" class="ghost">해제</button>
</div>

<nav>
  <button class="nav active" data-page="pageSearch">검색</button>
  <button class="nav" data-page="pageAdd">추가</button>
  <button class="nav" data-page="pageBulk">대량</button>
  <button class="nav" data-page="pageBackup">백업</button>
  <button class="nav" data-page="pageHelp">도움말</button>
</nav>

<div id="toast" class="toast"></div>

<script>
const DB_NAME = "tag_note_hierarchy_indexeddb_v2";
const STORE_NAME = "tags";
const DB_VERSION = 1;
const DISPLAY_LIMIT = 400;

const CATEGORY_TREE = {
  "전체": { "help": "전체 태그를 표시합니다.", "subs": ["전체"] },
  "이미지 구성": {
    "help": "품질, 구도, 아트 스타일 등 이미지 구성 레이아웃 태그 목록입니다.",
    "subs": ["전체", "품질 및 해상도", "출처 표시", "이미지 형식", "아트 스타일", "색상 및 톤", "구도 및 앵글", "기타"]
  },
  "인물": {
    "help": "신체적 특징, 헤어스타일, 눈 종류 관련 태그입니다.",
    "subs": ["전체", "인원수 및 관계", "신체적 특징", "헤어스타일 및 색상", "눈 특징", "기타"]
  },
  "복장 및 악세서리": {
    "help": "착용 의복, 장비 장식 소품 태그 목록입니다.",
    "subs": ["전체", "상체 의류", "하체 의류", "드레스 코스튬", "액세서리", "기타"]
  },
  "기타": { "help": "분류가 명확하지 않은 태그 모음입니다.", "subs": ["전체", "기타"] }
};

const starterTags = [];

let db;
let allTags = [];
let activeMain = "전체";
let activeSub = "전체";
let selectedTags = new Set();
let modalPicker = null;
let editModalContext = null;

const $ = id => document.getElementById(id);

window.addEventListener("DOMContentLoaded", async () => {
  db = await openDB();
  await seedIfEmpty();
  await refreshAll();
  initCategorySelects();

  // 폰 로컬 스토리지 브라우저에서 안전하게 영구 로드
  if (localStorage.getItem("cloud_token")) $("cloudToken").value = localStorage.getItem("cloud_token");
  if (localStorage.getItem("cloud_gist_id")) $("cloudGistId").value = localStorage.getItem("cloud_gist_id");

  $("saveCfgBtn").onclick = () => {
    localStorage.setItem("cloud_token", $("cloudToken").value.trim());
    localStorage.setItem("cloud_gist_id", $("cloudGistId").value.trim());
    toast("🔑 토큰 열쇠와 주소값을 폰 안전 기억공간에 저장했습니다!");
  };

  $("mainCategorySelect").onchange = () => {
    activeMain = $("mainCategorySelect").value;
    activeSub = "전체";
    updateSubSelect();
    updateModalSelectButtons();
    render();
  };

  $("subCategorySelect").onchange = () => {
    activeSub = $("subCategorySelect").value;
    updateModalSelectButtons();
    render();
  };

  $("mainCategorySelectBtn").onclick = () => openCategoryModal("searchMain");
  $("subCategorySelectBtn").onclick = () => openCategoryModal("searchSub");
  $("addMainCategoryBtn").onclick = () => openCategoryModal("addMain");
  $("addSubCategoryBtn").onclick = () => openCategoryModal("addSub");
  $("bulkMainCategoryBtn").onclick = () => openCategoryModal("bulkMain");
  $("bulkSubCategoryBtn").onclick = () => openCategoryModal("bulkSub");

  document.addEventListener("keydown", event => {
    if (event.key === "Escape") closeCategoryModal();
  });

  $("searchInput").oninput = () => { updateClearButton(); render(); };
  $("clearSearchBtn").onclick = () => { $("searchInput").value = ""; updateClearButton(); render(); $("searchInput").focus(); };

  $("addMainCategory").onchange = () => { updateLinkedSubSelect("addMainCategory", "addSubCategory"); updateLinkedModalButtons(); };
  $("bulkMainCategory").onchange = () => { updateLinkedSubSelect("bulkMainCategory", "bulkSubCategory"); updateLinkedModalButtons(); };

  $("saveTagBtn").onclick = addTag;
  $("bulkAddBtn").onclick = bulkAdd;
  $("exportBtn").onclick = exportJson;
  $("importFile").onchange = importJson;
  $("resetBtn").onclick = resetAll;

  $("cloudUploadBtn").onclick = uploadToCloud;
  $("cloudDownloadBtn").onclick = downloadFromCloud;

  $("copySelectedBtn").onclick = () => copySelected(", ");
  $("copySelectedLineBtn").onclick = () => copySelected("\n");
  $("clearSelectedBtn").onclick = clearSelected;

  document.querySelectorAll(".nav").forEach(btn => {
    btn.onclick = () => {
      document.querySelectorAll(".nav").forEach(b => b.classList.remove("active"));
      btn.classList.add("active");
      document.querySelectorAll(".page").forEach(p => p.classList.remove("active"));
      $(btn.dataset.page).classList.add("active");
      if (btn.dataset.page === "pageSearch") $("searchInput").focus();
    };
  });

  updateClearButton();
  updateSelectionBar();
  render();
});

function openDB() {
  return new Promise((resolve) => {
    const req = indexedDB.open(DB_NAME, DB_VERSION);
    req.onupgradeneeded = event => {
      const db = event.target.result;
      if (!db.objectStoreNames.contains(STORE_NAME)) db.createObjectStore(STORE_NAME, { keyPath:"tag" });
    };
    req.onsuccess = () => resolve(req.result);
  });
}

function txStore(mode="readonly") { return db.transaction(STORE_NAME, mode).objectStore(STORE_NAME); }

function getAllTags() {
  return new Promise((resolve) => {
    const req = txStore().getAll();
    req.onsuccess = () => resolve(req.result || []);
  });
}

function putTag(item) {
  return new Promise((resolve) => {
    const req = txStore("readwrite").put(normalizeItem(item));
    req.onsuccess = () => resolve();
  });
}

function deleteTagFromDB(tag) {
  return new Promise((resolve) => {
    const req = txStore("readwrite").delete(tag);
    req.onsuccess = () => resolve();
  });
}

function clearDB() {
  return new Promise((resolve) => {
    const req = txStore("readwrite").clear();
    req.onsuccess = () => resolve();
  });
}

async function seedIfEmpty() {
  const items = await getAllTags();
  if (items.length > 0) return;
  for (const item of starterTags) await putTag(item);
}

async function refreshAll() {
  allTags = await getAllTags();
  allTags.sort((a,b) => (b.usage || 0) - (a.usage || 0) || a.tag.localeCompare(b.tag));
}

function initCategorySelects() {
  fillMainSelect("mainCategorySelect", true); fillMainSelect("addMainCategory", false); fillMainSelect("bulkMainCategory", false);
  activeMain = "전체"; activeSub = "전체";
  $("mainCategorySelect").value = activeMain; $("addMainCategory").value = "이미지 구성"; $("bulkMainCategory").value = "이미지 구성";
  updateMainHelp(); updateSubSelect(); updateModalSelectButtons();
  updateLinkedSubSelect("addMainCategory", "addSubCategory"); updateLinkedSubSelect("bulkMainCategory", "bulkSubCategory");
  updateLinkedModalButtons();
}

function fillMainSelect(id, includeSpecial) {
  const select = $(id); select.innerHTML = "";
  Object.keys(CATEGORY_TREE).forEach(main => {
    if (!includeSpecial && main === "전체") return;
    const opt = document.createElement("option"); opt.value = main; opt.textContent = main; select.appendChild(opt);
  });
}

function updateMainHelp() { $("mainHelp").textContent = CATEGORY_TREE[activeMain]?.help || ""; }

function updateSubSelect() {
  updateMainHelp(); const subs = getSubs(activeMain); const select = $("subCategorySelect"); select.innerHTML = "";
  subs.forEach(sub => { const opt = document.createElement("option"); opt.value = sub; opt.textContent = sub; select.appendChild(opt); });
  select.value = activeSub; updateModalSelectButtons();
}

function updateModalSelectButtons() {
  if ($("mainCategorySelectBtn")) $("mainCategorySelectBtn").textContent = activeMain || "전체";
  if ($("subCategorySelectBtn")) $("subCategorySelectBtn").textContent = activeSub || "전체";
}

function openCategoryModal(type, context = null) {
  let dropdown = $("inlineDropdown");
  if (!dropdown) { dropdown = document.createElement("div"); dropdown.id = "inlineDropdown"; dropdown.className = "inlineDropdown"; }
  if (modalPicker === type && dropdown.classList.contains("show") && editModalContext === context) { closeCategoryModal(); return; }
  modalPicker = type; editModalContext = context;
  let btn = null;
  if (type === "searchMain") btn = $("mainCategorySelectBtn");
  else if (type === "searchSub") btn = $("subCategorySelectBtn");
  else if (type === "addMain") btn = $("addMainCategoryBtn");
  else if (type === "addSub") btn = $("addSubCategoryBtn");
  else if (type === "bulkMain") btn = $("bulkMainCategoryBtn");
  else if (type === "bulkSub") btn = $("bulkSubCategoryBtn");
  else if (context) {
    if (type === "editMain") btn = context.mainBtn;
    if (type === "editSub") btn = context.subBtn;
  }
  if (!btn) return;
  btn.parentNode.insertBefore(dropdown, btn.nextSibling);
  const selected = getModalSelectedValue(); dropdown.innerHTML = "";
  getModalOptions().forEach(name => {
    const optBtn = document.createElement("button"); optBtn.type = "button"; optBtn.className = "modalOptionBtn" + (name === selected ? " active" : "");
    optBtn.innerHTML = `<span>${esc(name)}</span>${name === selected ? '<span class="checkMark">✓</span>' : ''}`;
    optBtn.onclick = () => { dropdown.classList.remove("show"); setTimeout(() => { selectCategoryFromModal(name); closeCategoryModal(); }, 200); };
    dropdown.appendChild(optBtn);
  });
  requestAnimationFrame(() => dropdown.classList.add("show"));
}

function closeCategoryModal() { const d = $("inlineDropdown"); if (d) d.classList.remove("show"); modalPicker = null; editModalContext = null; }

// ☁️ 무선 클라우드 비동기 데이터 업로드 엔진
async function uploadToCloud() {
  const token = localStorage.getItem("cloud_token"); const gistId = localStorage.getItem("cloud_gist_id");
  if (!token || !gistId) return toast("상단에 발급받은 토큰 열쇠와 Gist ID를 각인해주세요.");
  toast("☁️ 원격 클라우드로 데이터 동기화 중...");
  await refreshAll();
  try {
    const res = await fetch(`https://api.github.com/gists/${gistId}`, {
      method: "PATCH",
      headers: { "Authorization": `token ${token}`, "Content-Type": "application/json" },
      body: JSON.stringify({ files: { "tags_backup.json": { content: JSON.stringify(allTags, null, 2) } } })
    });
    if (res.ok) toast("🚀 클라우드 데이터 업로드 성공!"); else toast("동기화 실패: 토큰 권한을 확인하세요.");
  } catch (e) { toast("네트워크 오류 발생"); }
}

// ☁️ 무선 클라우드 비동기 데이터 다운로드 복원 엔진 (요청하신 기능 연동 완결)
async function downloadFromCloud() {
  const token = localStorage.getItem("cloud_token"); const gistId = localStorage.getItem("cloud_gist_id");
  if (!token || !gistId) return toast("상단에 발급받은 토큰 열쇠와 Gist ID를 각인해주세요.");
  if (!confirm("원격 클라우드 데이터를 내려받으시겠습니까?\n주의: 현재 기기의 로컬 데이터가 완전히 교체됩니다.")) return;
  toast("📡 클라우드 보관함에서 데이터 수신 중...");
  try {
    const res = await fetch(`https://api.github.com/gists/${gistId}`, {
      method: "GET",
      headers: { "Authorization": `token ${token}`, "Cache-Control": "no-cache" }
    });
    if (!res.ok) return toast(`수신 실패: 인증 상태를 확인하세요. (${res.status})`);
    const gistData = await res.json();
    const file = gistData.files["tags_backup.json"];
    if (!file || !file.content || file.content === "{}") return toast("Gist 저장 공간이 완전히 비어있습니다.");
    const data = JSON.parse(file.content);
    if (!Array.isArray(data)) return toast("배열 데이터 형식이 올바르지 않습니다.");
    await clearDB(); let count = 0;
    for (const item of data) { if (!item.tag) continue; await putTag(item); count++; }
    await refreshAll(); toast(`✅ 성공: 클라우드에서 ${count}개 데이터를 안전하게 수집했습니다!`); render();
  } catch (e) { toast("불러오기 실패: JSON 파싱 오류"); }
}

function getModalOptions() {
  if (!modalPicker) return [];
  if (modalPicker === "searchMain") return Object.keys(CATEGORY_TREE);
  if (modalPicker === "searchSub") return getSubs(activeMain);
  if (modalPicker === "addMain" || modalPicker === "bulkMain") return Object.keys(CATEGORY_TREE).filter(x => x !== "전체");
  if (modalPicker === "addSub") return getSubs($("addMainCategory").value).filter(x => x !== "전체");
  if (modalPicker === "bulkSub") return getSubs($("bulkMainCategory").value).filter(x => x !== "전체");
  if (modalPicker === "editMain" && editModalContext) return Object.keys(CATEGORY_TREE).filter(x => x !== "전체");
  if (modalPicker === "editSub" && editModalContext) return getSubs(editModalContext.mainSelect.value).filter(x => x !== "전체");
  return [];
}

function getModalSelectedValue() {
  if (modalPicker === "searchMain") return activeMain; if (modalPicker === "searchSub") return activeSub;
  if (modalPicker === "addMain") return $("addMainCategory").value; if (modalPicker === "addSub") return $("addSubCategory").value;
  if (modalPicker === "bulkMain") return $("bulkMainCategory").value; if (modalPicker === "bulkSub") return $("bulkSubCategory").value;
  if (modalPicker === "editMain" && editModalContext) return editModalContext.mainSelect.value;
  if (modalPicker === "editSub" && editModalContext) return editModalContext.subSelect.value;
  return "";
}

function selectCategoryFromModal(name) {
  if (modalPicker === "searchMain") { activeMain = name; activeSub = "전체"; $("mainCategorySelect").value = activeMain; updateSubSelect(); render(); }
  else if (modalPicker === "searchSub") { activeSub = name; $("subCategorySelect").value = activeSub; updateModalSelectButtons(); render(); }
  else if (modalPicker === "addMain") { $("addMainCategory").value = name; updateLinkedSubSelect("addMainCategory", "addSubCategory"); updateLinkedModalButtons(); }
  else if (modalPicker === "addSub") { $("addSubCategory").value = name; updateLinkedModalButtons(); }
  else if (modalPicker === "bulkMain") { $("bulkMainCategory").value = name; updateLinkedSubSelect("bulkMainCategory", "bulkSubCategory"); updateLinkedModalButtons(); }
  else if (modalPicker === "bulkSub") { $("bulkSubCategory").value = name; updateLinkedModalButtons(); }
  else if (modalPicker === "editMain" && editModalContext) { editModalContext.mainSelect.value = name; editModalContext.subSelect.innerHTML = subOptions(name, "기타"); if (!editModalContext.subSelect.value) editModalContext.subSelect.selectedIndex = 0; updateEditModalButtons(editModalContext); }
  else if (modalPicker === "editSub" && editModalContext) { editModalContext.subSelect.value = name; updateEditModalButtons(editModalContext); }
  closeCategoryModal();
}

function updateLinkedSubSelect(mainId, subId) {
  const main = $(mainId).value; const subs = getSubs(main).filter(x => x !== "전체"); const select = $(subId); const previous = select.value; select.innerHTML = "";
  subs.forEach(sub => { const opt = document.createElement("option"); opt.value = sub; opt.textContent = sub; select.appendChild(opt); });
  if (subs.length === 0) { const opt = document.createElement("option"); opt.value = "기타"; opt.textContent = "기타"; select.appendChild(opt); }
  if (subs.includes(previous)) select.value = previous;
}

function updateLinkedModalButtons() {
  if ($("addMainCategoryBtn")) $("addMainCategoryBtn").textContent = $("addMainCategory").value || "대분류 선택";
  if ($("addSubCategoryBtn")) $("addSubCategoryBtn").textContent = $("addSubCategory").value || "소분류 선택";
  if ($("bulkMainCategoryBtn")) $("bulkMainCategoryBtn").textContent = $("bulkMainCategory").value || "대분류 선택";
  if ($("bulkSubCategoryBtn")) $("bulkSubCategoryBtn").textContent = $("bulkSubCategory").value || "소분류 선택";
}

function getSubs(main) { return CATEGORY_TREE[main]?.subs || ["전체", "기타"]; }
function updateClearButton() { $("clearSearchBtn").classList.toggle("show", $("searchInput").value.length > 0); }
function normalizeTagText(text) { return String(text || "").replaceAll("_", " ").replace(/\s+/g, " ").trim(); }

function normalizeItem(item) {
  const oldCategory = item.category || ""; const mainCategory = normalizeMainCategory(String(item.mainCategory || oldCategory || "기타").trim() || "기타");
  const subCategory = normalizeSubCategory(mainCategory, String(item.subCategory || "기타").trim() || "기타");
  return {
    tag: normalizeTagText(item.tag), kr: String(item.kr || "").trim(), mainCategory, subCategory,
    desc: repairMergedDescription(String(item.desc || item.note || "").trim()), keywords: String(item.keywords || "").trim(),
    favorite: Boolean(item.favorite), usage: Number(item.usage || 0), createdAt: item.createdAt || new Date().toISOString()
  };
}

function normalizeMainCategory(value) {
  const v = String(value || "").trim(); const alias = { "이미지의 구성": "이미지 구성", "개념 및 태마": "개념 및 테마", "복장 및 액세서리": "복장 및 악세서리", "악세서리": "복장 및 악세서리" };
  const fixed = alias[v] || v; return CATEGORY_TREE[fixed] ? fixed : "기타";
}

function normalizeSubCategory(main, value) { const v = String(value || "").trim() || "기타"; const subs = getSubs(main).filter(x => x !== "전체"); return subs.includes(v) ? v : "기타"; }

function repairMergedDescription(desc) {
  let text = String(desc || "").trim(); if (!text) return "";
  const pattern = /\s+[가-힣A-Za-z0-9!?\'"財富时代“”‘’「」『』·\-\s]{1,80}\s*\((?:[^()]|\\[()]){1,140},\s*[0-9][0-9,]*\)\s*/;
  const found = text.search(pattern); if (found > 40) { text = text.slice(0, found).trim(); } return text;
}

function hasMergedDescriptionWarning(item) { const desc = String(item?.desc || ""); if (!desc) return false; const pattern = /\s+[가-힣A-Za-z0-9!?\'"“”‘’「」『』·\-\s]{1,80}\s*\((?:[^()]|\\[()]){1,140},\s*[0-9][0-9,]*\)\s*/; return pattern.test(desc); }

async function addTag() {
  const tag = normalizeTagText($("tagInput").value); if (!tag) return toast("태그를 입력하세요.");
  await putTag({ tag, kr:$("krInput").value, mainCategory:$("addMainCategory").value, subCategory:$("addSubCategory").value, desc:repairMergedDescription($("descInput").value), keywords:$("keywordsInput").value, favorite:false, usage:0 });
  await refreshAll(); $("tagInput").value = ""; $("krInput").value = ""; $("descInput").value = ""; $("keywordsInput").value = ""; toast("저장했습니다."); render();
}

async function bulkAdd() {
  const text = $("bulkText").value.trim(); if (!text) return toast("추가할 내용을 입력하세요.");
  const items = parseBulkText(text, $("bulkMainCategory").value, $("bulkSubCategory").value); let count = 0;
  for (const item of items) { if (!item.tag) continue; await putTag(item); count++; }
  await refreshAll(); $("bulkText").value = ""; toast(`${count}개 저장했습니다.`); render();
}

function parseBulkText(text, defaultMainCategory, defaultSubCategory) {
  const normalized = text.replace(/\r\n/g, "\n").trim(); const lines = normalized.split("\n"); const hasAdvancedFormat = lines.some(line => isAdvancedTitleLine(line));
  if (!hasAdvancedFormat) { return normalized.split(/\r?\n/).map(x => x.trim()).filter(Boolean).map(line => parseLine(line, defaultMainCategory, defaultSubCategory)); }
  const blocks = []; let current = [];
  for (const rawLine of lines) {
    const line = rawLine.trim(); if (isAdvancedTitleLine(line)) { if (current.length) blocks.push(current.join("\n")); current = [line]; continue; }
    if (current.length || line) current.push(rawLine);
  }
  if (current.length) blocks.push(current.join("\n")); return blocks.map(block => parseAdvancedBlock(block, defaultMainCategory, defaultSubCategory)).filter(item => item && item.tag);
}

function isAdvancedTitleLine(line) { const s = String(line || "").trim(); if (!s || s.length > 180 || /[。.!?]$/.test(s)) return false; return /^.+?\s*\((?:[^()]|\\[()]){1,140},\s*[0-9][0-9,]*\)\s*$/.test(s); }

function matchAdvancedTitleLine(line) {
  const s = String(line || "").trim(); if (!isAdvancedTitleLine(s)) return null;
  const match = s.match(/^(.+?)\s*\(((?:[^()]|\\[()]){1,140}),\s*([0-9][0-9,]*)\)\s*$/); if (!match) return null;
  return { kr: match[1].trim(), tag: normalizeTagText(match[2].replace(/\\([()])/g, "$1").trim()), usage: Number(match[3].replace(/,/g, "")) || 0 };
}

function parseAdvancedBlock(block, defaultMainCategory, defaultSubCategory) {
  const lines = block.split("\n").map(v => v.trim()).filter(Boolean); if (!lines.length) return null;
  const title = matchAdvancedTitleLine(lines[0]); if (!title) return parseLine(lines[0], defaultMainCategory, defaultSubCategory);
  const kr = title.kr; const tag = title.tag; const usage = title.usage;
  let mainCategory = defaultMainCategory; let subCategory = defaultSubCategory; let descLines = []; let keywords = "";
  for (const line of lines.slice(1)) {
    if (line.includes("→")) { const parts = line.split("→"); mainCategory = normalizeMainCategory(parts[0].trim() || defaultMainCategory); subCategory = normalizeSubCategory(mainCategory, parts.slice(1).join("→").trim() || defaultSubCategory); }
    else if (/^키워드\s*[:：]/.test(line)) { keywords = line.replace(/^키워드\s*[:：]\s*/, "").trim(); }
    else if (isAdvancedTitleLine(line)) { break; } else { descLines.push(line); }
  }
  return { tag, kr, mainCategory, subCategory, desc: repairMergedDescription(descLines.join("\n").trim()), keywords, favorite: false, usage };
}

function parseLine(line, mainCategory, subCategory) {
  let cleaned = line.replace(/^[\-\*\•]\s*/, "").replace(/\u200b/g, "").trim(); let tag = ""; let kr = ""; let desc = ""; let keywords = "";
  const colonIndex = cleaned.indexOf(":");
  if (colonIndex >= 0) {
    tag = cleaned.slice(0, colonIndex).trim(); const rest = cleaned.slice(colonIndex + 1).trim(); const match = rest.match(/\(([^)]{1,80})\)\s*(.*)/);
    if (match) { kr = match[1].trim(); desc = match[2].trim(); } else { desc = rest; }
  } else { tag = cleaned; }
  tag = normalizeTagText(tag).replace(/\s+\/\s+.*$/, "").replace(/\s+：.*$/, "").trim(); return {tag, kr, mainCategory, subCategory, desc, keywords, favorite:false, usage:0};
}

function render() { renderTags(); }

function renderTags() {
  const q = $("searchInput").value.trim().toLowerCase();
  let filtered = allTags.filter(t => {
    const blob = `${t.tag} ${t.kr || ""} ${t.mainCategory || ""} ${t.subCategory || ""} ${t.desc || ""} ${t.keywords || ""}`.toLowerCase();
    return (q ? blob.includes(q) : true) && (q || activeMain === "전체" || t.mainCategory === activeMain) && (q || activeSub === "전체" || t.subCategory === activeSub);
  });
  $("totalCount").textContent = allTags.length; $("shownCount").textContent = Math.min(filtered.length, DISPLAY_LIMIT); $("tagList").innerHTML = "";
  if (filtered.length === 0) { $("tagList").innerHTML = "<p class='desc'>검색 결과가 없습니다.</p>"; return; }
  filtered.slice(0, DISPLAY_LIMIT).forEach(t => {
    const div = document.createElement("div"); div.className = "tag"; const suspect = hasMergedDescriptionWarning(t);
    if (selectedTags.has(t.tag)) div.classList.add("selected"); if (suspect) div.classList.add("suspect");
    div.innerHTML = `
      <div class="tagName">${esc(t.kr || t.tag)} (${esc(t.tag)})</div><div class="tagPath">${esc(t.mainCategory || "기타")} → ${esc(t.subCategory || "기타")}</div>
      ${suspect ? `<div class="suspectBox">⚠️ 설명 통합 정비 대상입니다.</div>` : ""}
      ${t.desc ? `<div class="tagDesc">${esc(t.desc)}</div>` : ""}${t.keywords ? `<div class="tagKeywords">키워드: ${esc(t.keywords)}</div>` : ""}
      <div class="tagBtns"><button class="copyBtn">복사</button><button class="edit">수정</button><button class="selectBtn">${selectedTags.has(t.tag) ? "✓" : "＋"}</button><button class="del danger">×</button></div>
      <div class="editBox">
        <input class="editTag" value="${attr(t.tag)}" placeholder="태그" /><input class="editKr" value="${attr(t.kr || "")}" placeholder="한글 이름" />
        <button type="button" class="modalSelectBtn editMainBtn">${esc(t.mainCategory || "대분류 선택")}</button><select class="editMain" style="display:none">${mainOptions(t.mainCategory)}</select>
        <button type="button" class="modalSelectBtn editSubBtn">${esc(t.subCategory || "소분류 선택")}</button><select class="editSub" style="display:none">${subOptions(t.mainCategory, t.subCategory)}</select>
        <textarea class="editDesc" placeholder="설명">${esc(t.desc || "")}</textarea><input class="editKeywords" value="${attr(t.keywords || "")}" placeholder="키워드" />
        <div class="editActions"><button class="saveEdit ok">수정 저장</button><button class="cancelEdit ghost">취소</button></div>
      </div>
    `;
    div.querySelector(".copyBtn").onclick = () => copyTag(t.tag); div.querySelector(".edit").onclick = () => toggleEditBox(div); div.querySelector(".selectBtn").onclick = () => toggleSelect(t.tag); div.querySelector(".del").onclick = () => deleteTag(t.tag);
    setupEditCategoryModal(div); div.querySelector(".saveEdit").onclick = () => saveEdit(t.tag, div); div.querySelector(".cancelEdit").onclick = () => { closeCategoryModal(); div.querySelector(".editBox").classList.remove("active"); };
    $("tagList").appendChild(div);
  });
}

function setupEditCategoryModal(div) {
  const context = { mainSelect: div.querySelector(".editMain"), subSelect: div.querySelector(".editSub"), mainBtn: div.querySelector(".editMainBtn"), subBtn: div.querySelector(".editSubBtn") };
  context.mainBtn.onclick = () => openCategoryModal("editMain", context); context.subBtn.onclick = () => openCategoryModal("editSub", context);
  context.mainSelect.onchange = () => { context.subSelect.innerHTML = subOptions(context.mainSelect.value, "기타"); updateEditModalButtons(context); }; updateEditModalButtons(context);
}
function updateEditModalButtons(context) { if (context) { context.mainBtn.textContent = context.mainSelect.value; context.subBtn.textContent = context.subSelect.value; } }
function toggleSelect(tag) { if (selectedTags.has(tag)) selectedTags.delete(tag); else selectedTags.add(tag); updateSelectionBar(); renderTags(); }
function updateSelectionBar() { $("selectionBar").classList.toggle("show", selectedTags.size > 0); $("selectionCount").textContent = `선택 ${selectedTags.size}개`; }
async function copySelected(separator) { if (selectedTags.size === 0) return toast("선택된 태그가 없습니다."); await copyText(Array.from(selectedTags).join(separator)); toast("복사 완료"); }
function clearSelected() { selectedTags.clear(); updateSelectionBar(); renderTags(); }
function mainOptions(selected) { return Object.keys(CATEGORY_TREE).filter(x => x !== "전체").map(main => `<option value="${attr(main)}" ${main === selected ? "selected" : ""}>${esc(main)}</option>`).join(""); }
function subOptions(main, selected) { return getSubs(main).filter(x => x !== "전체").map(sub => `<option value="${attr(sub)}" ${sub === selected ? "selected" : ""}>${esc(sub)}</option>`).join(""); }
function toggleEditBox(div) { const box = div.querySelector(".editBox"); const active = box.classList.contains("active"); closeCategoryModal(); document.querySelectorAll(".editBox").forEach(el => el.classList.remove("active")); if (!active) box.classList.add("active"); }

async function saveEdit(oldTag, div) {
  const oldItem = allTags.find(x => x.tag === oldTag); if (!oldItem) return toast("오류");
  const updated = { ...oldItem, tag: normalizeTagText(div.querySelector(".editTag").value), kr: div.querySelector(".editKr").value.trim(), mainCategory: div.querySelector(".editMain").value, subCategory: div.querySelector(".editSub").value, desc: repairMergedDescription(div.querySelector(".editDesc").value.trim()), keywords: div.querySelector(".editKeywords").value.trim() };
  if (!updated.tag) return toast("태그 이름 필수"); if (updated.tag !== oldTag) { await deleteTagFromDB(oldTag); selectedTags.delete(oldTag); }
  await putTag(updated); await refreshAll(); toast("수정했습니다."); updateSelectionBar(); render();
}

async function copyTag(tag) { await copyText(tag); const item = allTags.find(x => x.tag === tag); if (item) { item.usage = (item.usage || 0) + 1; await putTag(item); await refreshAll(); } toast(`복사됨: ${tag}`); renderTags(); }
async function copyText(text) { try { await navigator.clipboard.writeText(text); } catch { const temp = document.createElement("textarea"); temp.value = text; document.body.appendChild(temp); temp.select(); document.execCommand("copy"); document.body.removeChild(temp); } }
async function deleteTag(tag) { if (!confirm(`"${tag}" 삭제할까요?`)) return; await deleteTagFromDB(tag); selectedTags.delete(tag); await refreshAll(); updateSelectionBar(); render(); }
async function exportJson() { await refreshAll(); const blob = new Blob([JSON.stringify(allTags, null, 2)], { type:"application/json" }); const url = URL.createObjectURL(blob); const a = document.createElement("a"); a.href = url; a.download = `태그_백업_${Date.now()}.json`; a.click(); URL.revokeObjectURL(url); }

async function importJson(e) {
  const file = e.target.files[0]; if (!file) return; const reader = new FileReader();
  reader.onload = async () => {
    try {
      const data = JSON.parse(reader.result); if (!Array.isArray(data)) return toast("잘못된 파일입니다."); let count = 0;
      for (const item of data) { if (!item.tag) continue; await putTag(item); count++; }
      await refreshAll(); toast(`${count}개 복원 완료`); render();
    } catch { toast("읽기 실패"); }
  }; reader.readAsText(file);
}
async function resetAll() { if (!confirm("정말 전체 삭제할까요?")) return; await clearDB(); allTags = []; selectedTags.clear(); updateSelectionBar(); render(); toast("삭제했습니다."); }
function toast(msg) { const el = $("toast"); el.textContent = msg; el.classList.add("show"); setTimeout(() => el.classList.remove("show"), 1600); }
function esc(s) { return String(s).replaceAll("&","&amp;").replaceAll("<","&lt;").replaceAll(">","&gt;").replaceAll('"',"&quot;").replaceAll("'","&#039;"); }
function attr(s) { return esc(s); }
</script>
</body>
</html>
