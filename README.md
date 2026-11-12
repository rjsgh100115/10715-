# 10715-
동물 맞히기

<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>동물 사진 분류</title>

<style>
  * {
    box-sizing: border-box;
  }

  :root {
    color-scheme: light;
    font-family: "맑은 고딕", system-ui, sans-serif;
    color: #242424;
    background: #f3f3f3;
  }

  body {
    margin: 0;
    padding: 56px 20px;
  }

  main {
    max-width: 920px;
    margin: auto;
  }

  header {
    margin-bottom: 28px;
  }

  h1 {
    margin: 0 0 12px;
    font-size: 30px;
    letter-spacing: -1px;
  }

  header p {
    margin: 0;
    color: #777;
    font-size: 14px;
    line-height: 1.7;
  }

  .layout {
    display: grid;
    grid-template-columns: 1.1fr 1fr;
    gap: 20px;
  }

  .panel {
    padding: 26px;
    background: #fff;
    border: 1px solid #dedede;
    border-radius: 12px;
  }

  h2 {
    margin: 0 0 20px;
    font-size: 16px;
  }

  .preview {
    height: 310px;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    background: #f7f7f7;
    border: 1px solid #e9e9e9;
    border-radius: 6px;
    margin-bottom: 20px;
  }

  .preview p {
    padding: 16px;
    color: #909090;
    font-size: 14px;
    text-align: center;
  }

  canvas {
    display: block;
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
  }

  [hidden] {
    display: none !important;
  }

  input[type="file"] {
    width: 100%;
    color: #777;
    font: inherit;
    font-size: 13px;
  }

  input::file-selector-button {
    margin-right: 10px;
    padding: 9px 12px;
    border: 1px solid #d5d5d5;
    border-radius: 5px;
    background: #fff;
    color: #333;
    font: inherit;
    cursor: pointer;
  }

  button {
    min-height: 44px;
    padding: 12px;
    border: 0;
    border-radius: 6px;
    background: #292929;
    color: #fff;
    font: inherit;
    font-size: 14px;
    font-weight: 600;
    cursor: pointer;
    transition:
      background 150ms ease,
      transform 150ms ease;
  }

  button:hover:not(:disabled) {
    background: #444;
  }

  button:active:not(:disabled) {
    transform: translateY(1px);
  }

  button:disabled {
    background: #e6e6e6;
    color: #999;
    cursor: not-allowed;
  }

  #analyze {
    width: 100%;
    margin-top: 22px;
  }

  :focus-visible {
    outline: 2px solid #555;
    outline-offset: 4px;
  }

  #status {
    min-height: 20px;
    margin: 12px 0 0;
    color: #777;
    font-size: 12px;
    line-height: 1.7;
  }

  #headline {
    margin: 28px 0 8px;
    font-size: 26px;
    font-weight: 700;
    letter-spacing: -0.7px;
    overflow-wrap: anywhere;
  }

  #description {
    color: #888;
    font-size: 13px;
    line-height: 1.7;
  }

  #results {
    margin-top: 30px;
  }

  .result-item {
    margin-bottom: 24px;
    animation: appear 220ms ease-out both;
  }

  .result-row {
    display: flex;
    justify-content: space-between;
    gap: 14px;
    margin-bottom: 10px;
    font-size: 14px;
    line-height: 1.5;
  }

  .result-name {
    overflow-wrap: anywhere;
  }

  .result-score {
    color: #666;
    white-space: nowrap;
    font-variant-numeric: tabular-nums;
  }

  .track {
    height: 6px;
    background: #ededed;
    border-radius: 3px;
    overflow: hidden;
  }

  .fill {
    height: 100%;
    background: #454545;
    border-radius: 3px;
    transform-origin: left;
    animation: bar-appear 350ms ease-out both;
  }

  .result-item:nth-child(2) .fill {
    background: #808080;
  }

  .result-item:nth-child(3) .fill {
    background: #aaa;
  }

  .wiki-panel,
  .chat-panel {
    margin-top: 20px;
  }

  .wiki-panel:not([hidden]),
  .chat-panel:not([hidden]) {
    animation: appear 280ms ease-out both;
  }

  .section-top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 16px;
    margin-bottom: 18px;
  }

  .section-top h2 {
    margin: 0;
  }

  .section-tag {
    color: #888;
    font-size: 12px;
  }

  #wikiTitle {
    margin: 0 0 14px;
    font-size: 22px;
    letter-spacing: -0.5px;
  }

  #wikiText {
    margin: 0;
    color: #555;
    font-size: 15px;
    line-height: 1.95;
    white-space: pre-line;
    overflow-wrap: anywhere;
  }

  #wikiSource {
    margin-top: 22px;
    padding-top: 14px;
    border-top: 1px solid #eee;
    color: #888;
    font-size: 12px;
    line-height: 1.8;
  }

  a {
    color: #666;
    text-underline-offset: 3px;
  }

  .animal-label {
    max-width: 55%;
    padding: 5px 9px;
    border: 1px solid #e4e4e4;
    border-radius: 5px;
    color: #777;
    font-size: 12px;
    overflow-wrap: anywhere;
  }

  .chat-messages {
    height: 310px;
    overflow-y: auto;
    padding: 6px 4px 12px;
    scrollbar-width: thin;
    scrollbar-color: #ccc transparent;
  }

  .chat-line {
    display: flex;
    margin-bottom: 14px;
    animation: message-appear 180ms ease-out both;
  }

  .chat-line.user {
    justify-content: flex-end;
  }

  .chat-bubble {
    max-width: 88%;
    padding: 12px 15px;
    border: 1px solid #e9e9e9;
    border-radius: 8px;
    background: #f7f7f7;
    color: #444;
    font-size: 14px;
    line-height: 1.85;
    white-space: pre-line;
    overflow-wrap: anywhere;
  }

  .user .chat-bubble {
    border-color: #333;
    background: #333;
    color: #fff;
  }

  .chat-source {
    display: block;
    width: fit-content;
    margin-top: 10px;
    color: #888;
    font-size: 11px;
  }

  .suggestions {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin: 16px 0;
  }

  .suggestion {
    min-height: auto;
    padding: 7px 11px;
    border: 1px solid #dedede;
    border-radius: 5px;
    background: #fff;
    color: #666;
    font-size: 12px;
    font-weight: 400;
  }

  .suggestion:hover:not(:disabled) {
    background: #f2f2f2;
  }

  .chat-form {
    display: flex;
    gap: 10px;
    padding-top: 16px;
    border-top: 1px solid #eee;
  }

  .chat-input {
    flex: 1;
    min-width: 0;
    height: 44px;
    padding: 0 13px;
    border: 1px solid #dcdcdc;
    border-radius: 6px;
    background: #fff;
    color: #333;
    font: inherit;
    font-size: 14px;
  }

  .chat-send {
    flex-shrink: 0;
    padding: 0 18px;
    font-size: 13px;
  }

  .chat-caption {
    margin: 12px 0 0;
    color: #999;
    font-size: 11px;
    line-height: 1.7;
  }

  @keyframes appear {
    from {
      opacity: 0;
      transform: translateY(6px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes message-appear {
    from {
      opacity: 0;
      transform: translateY(3px);
    }
    to {
      opacity: 1;
      transform: translateY(0);
    }
  }

  @keyframes bar-appear {
    from { transform: scaleX(0); }
    to { transform: scaleX(1); }
  }

  @media (max-width: 680px) {
    body { padding: 32px 16px; }
    h1 { font-size: 26px; }
    .layout { grid-template-columns: 1fr; }
    .panel { padding: 22px; }
    .preview { height: 270px; }
    .chat-bubble { max-width: 94%; }
  }

  @media (prefers-reduced-motion: reduce) {
    *,
    *::before,
    *::after {
      animation: none !important;
      transition: none !important;
    }

    button:active:not(:disabled) {
      transform: none;
    }
  }
</style>
</head>

<body>
<main>
  <header>
    <h1>동물 사진 분류</h1>
    <p>사진을 선택하고 분석하기를 눌러 주세요.</p>
  </header>

  <div class="layout">
    <section class="panel">
      <h2>사진 업로드</h2>

      <div class="preview">
        <p id="placeholder">선택한 사진이 여기에 표시됩니다.</p>
        <canvas id="photo" hidden aria-label="선택한 사진"></canvas>
      </div>

      <input
        id="upload"
        type="file"
        accept="image/jpeg,image/png,image/webp"
        aria-label="분석할 사진 선택"
      >

      <button id="analyze" type="button" disabled>
        분석하기
      </button>

      <p id="status" role="status" aria-live="polite">
        분석 모델을 불러오는 중입니다.
      </p>
    </section>

    <section
      class="panel"
      id="resultPanel"
      aria-live="polite"
      aria-busy="false"
    >
      <h2>분석 결과</h2>
      <p id="headline">분석 전</p>
      <p id="description">분석 결과가 여기에 표시됩니다.</p>
      <div id="results"></div>
    </section>
  </div>

  <section
    class="panel wiki-panel"
    id="wikiPanel"
    hidden
    aria-live="polite"
    aria-busy="false"
  >
    <div class="section-top">
      <h2>예측한 동물 알아보기</h2>
      <span class="section-tag">위키백과</span>
    </div>

    <h3 id="wikiTitle"></h3>
    <p id="wikiText"></p>
    <div id="wikiSource" hidden></div>
  </section>

  <section
    class="panel chat-panel"
    id="chatPanel"
    hidden
    aria-label="동물 정보 챗봇"
  >
    <div class="section-top">
      <h2>동물에 대해 질문하기</h2>
      <span id="chatAnimal" class="animal-label"></span>
    </div>

    <div
      id="chatMessages"
      class="chat-messages"
      role="log"
      aria-label="대화 내용"
      aria-live="polite"
      aria-relevant="additions"
      tabindex="0"
    ></div>

    <div class="suggestions">
      <button type="button" class="suggestion">
        어떤 특징이 있나요?
      </button>
      <button type="button" class="suggestion">
        무엇을 먹나요?
      </button>
      <button type="button" class="suggestion">
        어디에 사나요?
      </button>
      <button type="button" class="suggestion">
        수명은 얼마나 되나요?
      </button>
    </div>

    <form id="chatForm" class="chat-form">
      <input
        id="chatInput"
        class="chat-input"
        type="text"
        placeholder="이 동물에 대해 질문해 주세요."
        aria-label="동물에 대한 질문"
        maxlength="300"
        autocomplete="off"
        required
      >
      <button id="chatSend" class="chat-send" type="submit">
        보내기
      </button>
    </form>

    <p class="chat-caption">
      예측한 동물의 위키백과 문서에서 관련 내용을 찾아 답합니다.
    </p>
  </section>
</main>

<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@4.22.0/dist/tf.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/mobilenet@2.1.1/dist/mobilenet.min.js"></script>

<script>
"use strict";

const $ = id => document.getElementById(id);

let model = null;
let modelFailed = false;
let photoReady = false;
let busy = false;

// 새 사진으로 바뀐 뒤 이전 질문의 답변이 섞이지 않게 합니다.
let chatSession = 0;
let chatSending = false;

function emptyChat() {
  return {
    name: "",
    label: "",
    page: null,
    text: "",
    loading: Promise.resolve()
  };
}

let chat = emptyChat();

// 분류명: [화면 표시 이름, 한국어 위키백과 문서 제목]
const animals = {
  "tabby": ["줄무늬 고양이", "고양이"],
  "tiger cat": ["호랑이무늬 고양이", "고양이"],
  "Persian cat": ["페르시안 고양이", "페르시안"],
  "Siamese cat": ["샴 고양이", "샴고양이"],
  "Egyptian cat": ["이집트 고양이", "이집션 마우"],
  "golden retriever": ["골든 리트리버", "골든 리트리버"],
  "Labrador retriever": ["래브라도 리트리버", "래브라도 리트리버"],
  "German shepherd": ["저먼 셰퍼드", "저먼 셰퍼드"],
  "Chihuahua": ["치와와", "치와와 (개)"],
  "pug": ["퍼그", "퍼그"],
  "beagle": ["비글", "비글"],
  "Pomeranian": ["포메라니안", "포메라니안"],
  "Siberian husky": ["시베리안 허스키", "시베리안 허스키"],
  "toy poodle": ["토이 푸들", "푸들"],
  "miniature poodle": ["미니어처 푸들", "푸들"],
  "standard poodle": ["스탠더드 푸들", "푸들"],
  "Shih-Tzu": ["시추", "시추"],
  "Maltese dog": ["말티즈", "말티즈"],
  "dalmatian": ["달마티안", "달마티안"],
  "tiger": ["호랑이", "호랑이"],
  "lion": ["사자", "사자"],
  "cheetah": ["치타", "치타"],
  "leopard": ["표범", "표범"],
  "snow leopard": ["눈표범", "눈표범"],
  "zebra": ["얼룩말", "얼룩말"],
  "African elephant": ["아프리카코끼리", "아프리카코끼리"],
  "Indian elephant": ["인도코끼리", "인도코끼리"],
  "giant panda": ["자이언트판다", "대왕판다"],
  "lesser panda": ["레서판다", "레서판다"],
  "brown bear": ["불곰", "불곰"],
  "ice bear": ["북극곰", "북극곰"],
  "American black bear": ["아메리카흑곰", "아메리카흑곰"],
  "red fox": ["붉은여우", "붉은여우"],
  "koala": ["코알라", "코알라"],
  "meerkat": ["미어캣", "미어캣"],
  "hamster": ["햄스터", "햄스터"],
  "guinea pig": ["기니피그", "기니피그"],
  "goldfish": ["금붕어", "금붕어"],
  "gorilla": ["고릴라", "고릴라"],
  "chimpanzee": ["침팬지", "침팬지"],
  "orangutan": ["오랑우탄", "오랑우탄"],
  "otter": ["수달", "수달"],
  "hedgehog": ["고슴도치", "고슴도치"],
  "wood rabbit": ["솜꼬리토끼", "솜꼬리토끼속"],
  "Angora": ["앙고라토끼", "앙고라토끼"],
  "king penguin": ["임금펭귄", "임금펭귄"],
  "hippopotamus": ["하마", "하마"],
  "wombat": ["웜뱃", "웜뱃"],
  "bison": ["들소", "들소"],
  "impala": ["임팔라", "임팔라"],
  "gazelle": ["가젤", "가젤"],
  "flamingo": ["홍학", "홍학"],
  "peacock": ["공작", "공작 (새)"],
  "bald eagle": ["흰머리수리", "흰머리수리"],
  "African crocodile": ["나일악어", "나일악어"],
  "American alligator": ["미국악어", "미국악어"]
};

const englishTitles = {
  "tabby": "Tabby cat",
  "tiger cat": "Tabby cat",
  "Egyptian cat": "Egyptian Mau",
  "ice bear": "Polar bear",
  "lesser panda": "Red panda",
  "toy poodle": "Poodle",
  "miniature poodle": "Poodle",
  "standard poodle": "Poodle",
  "Angora": "Angora rabbit",
  "wood rabbit": "Cottontail rabbit",
  "African crocodile": "Nile crocodile"
};

function firstLabel(label) {
  return label.split(",")[0].trim();
}

function displayName(label) {
  const key = firstLabel(label);
  return animals[key]?.[0] || key;
}

function updateAnalyzeButton() {
  $("analyze").disabled = !model || !photoReady || busy;
}

function readyMessage() {
  if (modelFailed) {
    return "모델 연결에 실패했습니다. 인터넷 연결을 확인하고 새로고침해 주세요.";
  }

  if (!model) {
    return "분석 모델을 불러오는 중입니다.";
  }

  return photoReady
    ? "사진이 준비되었습니다."
    : "사진을 선택해 주세요.";
}

function updateChatControls() {
  $("chatInput").disabled = chatSending;
  $("chatSend").disabled = chatSending;
  $("chatSend").textContent = chatSending ? "확인 중" : "보내기";

  document.querySelectorAll(".suggestion").forEach(button => {
    button.disabled = chatSending;
  });
}

function resetChat() {
  chatSession++;
  chatSending = false;
  chat = emptyChat();

  $("chatPanel").hidden = true;
  $("chatMessages").replaceChildren();
  $("chatInput").value = "";

  updateChatControls();
}

function clearResults() {
  resetChat();

  $("results").replaceChildren();
  $("headline").textContent = "분석 전";
  $("description").textContent = "분석 결과가 여기에 표시됩니다.";

  $("wikiPanel").hidden = true;
  $("wikiPanel").setAttribute("aria-busy", "false");
  $("wikiTitle").textContent = "";
  $("wikiText").textContent = "";
  $("wikiSource").replaceChildren();
  $("wikiSource").hidden = true;
}

async function initialize() {
  try {
    if (!window.tf || !window.mobilenet) {
      throw new Error("라이브러리 연결 실패");
    }

    await tf.ready();

    model = await mobilenet.load({
      version: 2,
      alpha: 1.0
    });
  } catch (error) {
    console.error(error);
    modelFailed = true;
  } finally {
    if (!busy) {
      $("status").textContent = readyMessage();
    }

    updateAnalyzeButton();
  }
}

// ---------- 사진 선택 ----------

$("upload").addEventListener("change", async event => {
  const file = event.target.files[0];
  if (!file) return;

  photoReady = false;
  clearResults();
  updateAnalyzeButton();

  $("photo").hidden = true;
  $("placeholder").hidden = false;

  if (!["image/jpeg", "image/png", "image/webp"].includes(file.type)) {
    $("placeholder").textContent = "사진을 다시 선택해 주세요.";
    $("status").textContent = "JPG, PNG, WebP 사진을 선택해 주세요.";
    event.target.value = "";
    return;
  }

  if (file.size > 15 * 1024 * 1024) {
    $("placeholder").textContent = "사진을 다시 선택해 주세요.";
    $("status").textContent = "15MB 이하의 사진을 선택해 주세요.";
    event.target.value = "";
    return;
  }

  busy = true;
  $("upload").disabled = true;
  $("placeholder").textContent = "사진을 불러오는 중입니다.";
  $("status").textContent = "사진을 준비하고 있습니다.";
  updateAnalyzeButton();

  const url = URL.createObjectURL(file);

  try {
    const image = new Image();
    image.src = url;
    await image.decode();

    const scale = Math.min(
      1,
      1024 / Math.max(image.naturalWidth, image.naturalHeight)
    );

    const canvas = $("photo");

    canvas.width = Math.max(
      1,
      Math.round(image.naturalWidth * scale)
    );

    canvas.height = Math.max(
      1,
      Math.round(image.naturalHeight * scale)
    );

    canvas.getContext("2d").drawImage(
      image,
      0,
      0,
      canvas.width,
      canvas.height
    );

    canvas.hidden = false;
    $("placeholder").hidden = true;
    photoReady = true;
  } catch (error) {
    console.error(error);
    $("placeholder").textContent = "사진을 표시할 수 없습니다.";
    $("status").textContent = "다른 사진을 선택해 주세요.";
  } finally {
    URL.revokeObjectURL(url);
    busy = false;
    $("upload").disabled = false;

    if (photoReady) {
      $("status").textContent = readyMessage();
    }

    updateAnalyzeButton();
  }
});

// ---------- 위키백과 ----------

async function wikiRequest(language, parameters) {
  const endpoint = new URL(
    `https://${language}.wikipedia.org/w/api.php`
  );

  endpoint.search = new URLSearchParams({
    action: "query",
    format: "json",
    formatversion: "2",
    origin: "*",
    redirects: "1",
    ...parameters
  });

  const controller = new AbortController();
  const timer = setTimeout(() => controller.abort(), 10000);

  try {
    const response = await fetch(endpoint, {
      signal: controller.signal,
      credentials: "omit"
    });

    if (!response.ok) {
      throw new Error("위키백과 연결 실패");
    }

    const data = await response.json();

    if (data.error) {
      throw new Error("위키백과 API 오류");
    }

    return data;
  } finally {
    clearTimeout(timer);
  }
}

async function getWikiPage(language, title) {
  const data = await wikiRequest(language, {
    titles: title,
    prop: "extracts|info|pageprops|langlinks",
    exintro: "1",
    explaintext: "1",
    inprop: "url",
    ppprop: "disambiguation",
    lllang: "ko",
    lllimit: "1"
  });

  const page = data.query?.pages?.[0];

  if (
    !page ||
    page.missing ||
    page.invalid ||
    Object.prototype.hasOwnProperty.call(
      page.pageprops || {},
      "disambiguation"
    )
  ) {
    return null;
  }

  return { ...page, language };
}

async function findWikiArticle(label) {
  const key = firstLabel(label);
  const koreanTitle = animals[key]?.[1];

  if (koreanTitle) {
    const koreanPage = await getWikiPage("ko", koreanTitle)
      .catch(() => null);

    if (koreanPage?.extract?.trim()) {
      return koreanPage;
    }
  }

  const englishPage = await getWikiPage(
    "en",
    englishTitles[key] || key
  ).catch(() => null);

  if (!englishPage) return null;

  const koreanLink = englishPage.langlinks?.find(
    link => link.lang === "ko"
  );

  if (koreanLink?.title) {
    const koreanPage = await getWikiPage("ko", koreanLink.title)
      .catch(() => null);

    if (koreanPage?.extract?.trim()) {
      return koreanPage;
    }
  }

  return englishPage.extract?.trim() ? englishPage : null;
}

function splitSentences(text, language) {
  const cleaned = text.replace(/\s+/g, " ").trim();
  if (!cleaned) return [];

  if (typeof Intl.Segmenter === "function") {
    const segmenter = new Intl.Segmenter(language, {
      granularity: "sentence"
    });

    return [...segmenter.segment(cleaned)]
      .map(item => item.segment.trim())
      .filter(Boolean);
  }

  return cleaned.match(/[^.!?]+[.!?]+|[^.!?]+$/g) || [cleaned];
}

function shortExtract(text, language) {
  const excerpt = splitSentences(text, language)
    .slice(0, 3)
    .join(" ");

  if (excerpt.length <= 800) return excerpt;

  return excerpt.slice(0, 800).trim() + "…";
}

function pageUrl(page) {
  return page.fullurl ||
    `https://${page.language}.wikipedia.org/wiki/` +
    encodeURIComponent(page.title.replaceAll(" ", "_"));
}

function makeLink(text, href) {
  const link = document.createElement("a");
  link.textContent = text;
  link.href = href;
  link.target = "_blank";
  link.rel = "noopener noreferrer";
  return link;
}

function showSource(page) {
  const source = $("wikiSource");
  source.replaceChildren();

  source.append(
    makeLink(`위키백과 「${page.title}」 원문`, pageUrl(page)),
    document.createElement("br"),
    document.createTextNode(
      "도입부 일부 발췌 · 위키백과 기여자 · "
    ),
    makeLink(
      "CC BY-SA 4.0",
      "https://creativecommons.org/licenses/by-sa/4.0/"
    )
  );

  source.hidden = false;
}

async function getFullArticle(page) {
  const data = await wikiRequest(page.language, {
    titles: page.title,
    prop: "extracts",
    explaintext: "1",
    exsectionformat: "plain"
  });

  const text = data.query?.pages?.[0]?.extract;

  if (!text?.trim()) {
    throw new Error("본문 없음");
  }

  return text;
}

async function showWikiDescription(label) {
  $("wikiPanel").hidden = false;
  $("wikiPanel").setAttribute("aria-busy", "true");
  $("wikiTitle").textContent = displayName(label);
  $("wikiText").textContent = "위키백과 설명을 불러오는 중입니다.";

  let page = null;

  try {
    page = await findWikiArticle(label);

    if (!page) {
      $("wikiText").textContent =
        "이 예측 결과에 대응하는 위키백과 설명을 찾지 못했습니다.";
      return;
    }

    const excerpt = shortExtract(page.extract, page.language);

    $("wikiText").textContent = page.language === "en"
      ? "한국어 문서를 찾지 못해 영어 설명을 표시합니다.\n\n" + excerpt
      : excerpt;

    showSource(page);
  } catch (error) {
    console.error(error);

    $("wikiText").textContent =
      "위키백과 설명을 불러오지 못했습니다. 인터넷 연결을 확인해 주세요.";
  } finally {
    $("wikiPanel").setAttribute("aria-busy", "false");

    // 문서를 못 찾더라도 대화창과 첫 메시지는 표시합니다.
    prepareChat(label, page);
  }
}

// ---------- 신경망 분석 ----------

function renderPredictions(predictions) {
  $("results").replaceChildren();

  predictions.forEach((prediction, index) => {
    const percent = prediction.probability * 100;

    const item = document.createElement("div");
    item.className = "result-item";
    item.style.animationDelay = `${index * 45}ms`;

    const row = document.createElement("div");
    row.className = "result-row";

    const name = document.createElement("span");
    name.className = "result-name";
    name.textContent =
      `${index + 1}. ${displayName(prediction.className)}`;
    name.title = prediction.className;

    const score = document.createElement("span");
    score.className = "result-score";
    score.textContent = `${percent.toFixed(1)}%`;

    const track = document.createElement("div");
    track.className = "track";

    const fill = document.createElement("div");
    fill.className = "fill";
    fill.style.width = `${Math.max(0, Math.min(100, percent))}%`;

    row.append(name, score);
    track.append(fill);
    item.append(row, track);

    $("results").append(item);
  });
}

$("analyze").addEventListener("click", async () => {
  if (!model || !photoReady || busy) return;

  busy = true;
  clearResults();

  $("upload").disabled = true;
  $("analyze").textContent = "분석 중…";
  $("resultPanel").setAttribute("aria-busy", "true");
  $("headline").textContent = "분석 중";
  $("description").textContent = "잠시 기다려 주세요.";
  $("status").textContent = "신경망으로 사진을 분석하고 있습니다.";

  updateAnalyzeButton();

  try {
    await tf.nextFrame();

    // 실제 신경망 추론은 버튼을 눌렀을 때만 실행합니다.
    const predictions = await model.classify($("photo"), 3);

    if (!predictions.length) {
      throw new Error("예측 결과 없음");
    }

    const top = predictions[0];

    $("headline").textContent = displayName(top.className);
    $("description").textContent =
      "가장 높은 점수를 받은 예측입니다.";

    renderPredictions(predictions);

    $("resultPanel").setAttribute("aria-busy", "false");
    $("status").textContent = "위키백과 설명을 불러오고 있습니다.";

    await showWikiDescription(top.className);

    $("status").textContent = "사진 분석이 완료되었습니다.";
  } catch (error) {
    console.error(error);

    $("headline").textContent = "분석 실패";
    $("description").textContent =
      "다시 시도하거나 다른 사진을 선택해 주세요.";
    $("status").textContent = "사진 분석 중 오류가 발생했습니다.";
  } finally {
    busy = false;
    $("upload").disabled = false;
    $("analyze").textContent = "분석하기";
    $("resultPanel").setAttribute("aria-busy", "false");

    updateAnalyzeButton();
  }
});

// ---------- 챗봇 ----------

function addMessage(role, text, cite = false) {
  const line = document.createElement("div");
  line.className = `chat-line ${role}`;

  const bubble = document.createElement("div");
  bubble.className = "chat-bubble";

  // 사용자 입력과 외부 문서는 HTML이 아닌 텍스트로 넣습니다.
  bubble.textContent = text;

  if (cite && chat.page) {
    const source = makeLink(
      `출처: 위키백과 「${chat.page.title}」`,
      pageUrl(chat.page)
    );

    source.className = "chat-source";
    bubble.append(source);
  }

  line.append(bubble);
  $("chatMessages").append(line);

  $("chatMessages").scrollTop =
    $("chatMessages").scrollHeight;
}

function prepareChat(label, page) {
  resetChat();

  const currentSession = chatSession;

  chat.name = displayName(label);
  chat.label = label;
  chat.page = page;
  chat.text = page?.extract || "";

  $("chatAnimal").textContent = chat.name;
  $("chatPanel").hidden = false;

  addMessage(
    "assistant",
    `${chat.name}에 대해 더 궁금한 점이 있나요?`
  );

  if (!page) {
    addMessage(
      "assistant",
      "지금은 위키백과 정보를 불러오지 못했어요. " +
      "인터넷 연결을 확인하고 다시 분석하면 문서를 바탕으로 답할 수 있어요."
    );
    return;
  }

  // 질문 범위를 넓히기 위해 본문도 미리 불러옵니다.
  // 실패하면 이미 가져온 도입부를 사용합니다.
  chat.loading = getFullArticle(page)
    .then(text => {
      if (chatSession === currentSession) {
        chat.text = text;
      }
    })
    .catch(error => {
      console.warn("도입부 정보만 사용합니다.", error);
    });
}

const topics = [
  {
    name: "먹이",
    question: /먹|식성|식단|사료|사냥|diet|eat|food/i,
    words: [
      "먹이", "먹는다", "먹으며", "먹고", "먹는",
      "섭취", "식성", "육식", "초식", "잡식", "사냥",
      "diet", "feed", "eats", "prey", "carnivor", "herbivor"
    ]
  },
  {
    name: "서식지",
    question: /어디|서식|분포|사는 곳|살아|habitat|where/i,
    words: [
      "서식", "분포", "서식지", "살고", "살며",
      "숲", "초원", "사막", "습지", "해안",
      "habitat", "native", "inhabit", "distributed"
    ]
  },
  {
    name: "수명",
    question: /수명|몇 년|얼마나.*살|오래.*살|lifespan/i,
    words: [
      "수명", "기대 수명", "살 수", "살기도",
      "lifespan", "life span", "longevity", "life expectancy"
    ]
  },
  {
    name: "크기",
    question: /크기|몸무게|무게|키|길이|얼마나.*크|size|weight/i,
    words: [
      "몸길이", "몸무게", "무게", "체중", "어깨높이",
      "센티미터", "킬로그램", "kg", "cm",
      "weigh", "length", "height"
    ]
  },
  {
    name: "특징",
    question: /특징|생김새|외모|털|색깔|모습|외형|feature/i,
    words: [
      "특징", "털", "무늬", "꼬리", "귀", "다리",
      "머리", "깃털", "부리", "coat", "fur",
      "plumage", "tail", "appearance"
    ]
  },
  {
    name: "행동",
    question: /성격|성향|행동|습성|야행|활동|behavior|temperament/i,
    words: [
      "성격", "성향", "행동", "습성", "야행성",
      "무리", "활동", "온순", "영역",
      "behavior", "behaviour", "temperament", "nocturnal", "social"
    ]
  },
  {
    name: "번식",
    question: /번식|임신|새끼|알을|산란|짝짓|reproduction/i,
    words: [
      "번식", "임신", "새끼", "산란", "짝짓기", "부화",
      "gestation", "breeding", "offspring", "reproduction"
    ]
  },
  {
    name: "보전 상태",
    question: /멸종|보호|보전|위기|endangered/i,
    words: [
      "멸종", "보전", "보호", "위협", "개체 수",
      "endangered", "conservation", "threatened", "vulnerable"
    ]
  },
  {
    name: "기원",
    question: /기원|유래|역사|원산|품종|origin|history/i,
    words: [
      "기원", "유래", "원산", "품종", "개량",
      "origin", "bred", "breed", "developed"
    ]
  }
];

function articleSentences(text, language) {
  const mainText = text.split(
    /\n\s*(?:각주|참고 문헌|참고문헌|외부 링크|References|External links)\s*\n/i
  )[0];

  const sentences = [];

  for (const paragraph of mainText.split(/\n+/)) {
    const clean = paragraph.replace(/\s+/g, " ").trim();

    if (clean.length < 20) continue;

    sentences.push(...splitSentences(clean, language));
  }

  return [...new Set(sentences)].filter(sentence =>
    sentence.length >= 20 && sentence.length <= 700
  );
}

function buildAnswer(question) {
  if (/^(안녕|안녕하세요|하이|hello|hi)[!.\s~]*$/i.test(question)) {
    return {
      text: `${chat.name}의 특징, 먹이, 서식지 등을 물어봐 주세요.`,
      cite: false
    };
  }

  if (/^(고마워|고맙습니다|감사합니다|감사해요)[!.\s~]*$/.test(question)) {
    return {
      text: "다른 궁금한 점이 있으면 물어봐 주세요.",
      cite: false
    };
  }

  if (/정확|확실|맞아|맞나요|정답/.test(question)) {
    return {
      text:
        "현재 사진에서 신경망이 가장 높은 점수를 준 분류를 기준으로 " +
        "설명하고 있어요. 실제 사진의 동물과 다를 수 있어요.",
      cite: false
    };
  }

  if (!chat.page || !chat.text.trim()) {
    return {
      text:
        "지금은 참고할 위키백과 정보를 불러오지 못했어요. " +
        "다시 분석한 뒤 질문해 주세요.",
      cite: false
    };
  }

  const selectedTopics = topics.filter(topic =>
    topic.question.test(question)
  );

  const introductionQuestion =
    /소개|요약|어떤 동물|무슨 동물|설명|알려/.test(question);

  if (introductionQuestion && !selectedTopics.length) {
    const intro = shortExtract(chat.page.extract, chat.page.language);

    return {
      text: chat.page.language === "en"
        ? "영문 위키백과의 소개 내용이에요.\n\n" + intro
        : intro,
      cite: true
    };
  }

  let words = selectedTopics.flatMap(topic => topic.words);

  if (!words.length) {
    const stopWords = new Set([
      "동물", "사진", "대해", "대해서", "알려줘",
      "알려주세요", "궁금해", "궁금해요",
      "무엇", "뭐야", "뭔가요", "어떻게", "그리고"
    ]);

    words = question
      .toLowerCase()
      .replace(/[^\p{L}\p{N}\s]/gu, " ")
      .split(/\s+/)
      .map(word =>
        word.replace(/(인가요|인가|나요|은|는|을|를|이|가|의|에|도)$/, "")
      )
      .filter(word =>
        word.length >= 2 && !stopWords.has(word)
      );
  }

  words = [...new Set(words.map(word => word.toLowerCase()))];

  const candidates = articleSentences(
    chat.text,
    chat.page.language
  )
    .map((sentence, index) => {
      const lower = sentence.toLowerCase();

      const score = words.reduce(
        (sum, word) => sum + (lower.includes(word) ? 1 : 0),
        0
      );

      return { sentence, score, index };
    })
    .filter(item => item.score > 0)
    .sort((a, b) =>
      b.score - a.score || a.index - b.index
    );

  if (!candidates.length) {
    const names = selectedTopics
      .map(topic => topic.name)
      .join("·");

    return {
      text: names
        ? `불러온 문서에서는 ${names}에 대한 내용을 찾지 못했어요. ` +
          "다른 특징을 물어보거나 위키백과 원문을 확인해 주세요."
        : "질문과 관련된 내용을 불러온 문서에서 찾지 못했어요. " +
          "특징, 먹이, 서식지처럼 주제를 구체적으로 물어봐 주세요.",
      cite: false
    };
  }

  const selected = candidates
    .slice(0, 2)
    .sort((a, b) => a.index - b.index);

  const excerpt = selected
    .map(item => item.sentence)
    .join("\n\n");

  return {
    text:
      (chat.page.language === "en"
        ? "영문 위키백과에서 찾은 관련 내용이에요.\n\n"
        : "위키백과에서 찾은 관련 내용이에요.\n\n") +
      excerpt,
    cite: true
  };
}

async function submitQuestion(question) {
  question = question.trim().slice(0, 300);

  if (!question || chatSending || $("chatPanel").hidden) {
    return;
  }

  const currentSession = chatSession;
  chatSending = true;

  addMessage("user", question);
  $("chatInput").value = "";
  updateChatControls();

  try {
    // 본문 조회가 진행 중이면 완료될 때까지 기다립니다.
    await chat.loading;

    if (currentSession !== chatSession) return;

    const answer = buildAnswer(question);
    addMessage("assistant", answer.text, answer.cite);
  } catch (error) {
    console.error(error);

    if (currentSession === chatSession) {
      addMessage(
        "assistant",
        "답변을 찾는 중 문제가 생겼어요. 다시 질문해 주세요."
      );
    }
  } finally {
    if (currentSession === chatSession) {
      chatSending = false;
      updateChatControls();
      $("chatInput").focus({ preventScroll: true });
    }
  }
}

$("chatForm").addEventListener("submit", event => {
  event.preventDefault();
  submitQuestion($("chatInput").value);
});

document.querySelectorAll(".suggestion").forEach(button => {
  button.addEventListener("click", () => {
    submitQuestion(button.textContent.trim());
  });
});

initialize();
</script>
</body>
</html>
echo "# -" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/rjsgh100115/-.git
git push -u origin main
