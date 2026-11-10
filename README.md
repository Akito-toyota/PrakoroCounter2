<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover">
<title>プラコロ対戦カウンター</title>

<style>
:root{
  --bg:#05070a;
  --panel:#0b1017;
  --monitor:#070a0f;
  --line:#263447;
  --text:#eaf2ff;
  --muted:#718096;
  --green:#00d47b;
  --yellow:#ffd400;
  --red:#ff334f;
}

*{
  box-sizing:border-box;
  -webkit-tap-highlight-color:transparent;
}

html,
body{
  width:100%;
  height:100%;
  margin:0;
  overflow:hidden;
  overscroll-behavior:none;
}

body{
  position:fixed;
  inset:0;
  background:var(--bg);
  color:var(--text);
  font-family:system-ui,-apple-system,"Hiragino Kaku Gothic ProN","Yu Gothic",sans-serif;
  touch-action:none;
  user-select:none;
  -webkit-user-select:none;
}

button,
select{
  font-family:inherit;
}

.app{
  width:100%;
  height:100dvh;
  padding:max(4px,env(safe-area-inset-top))
          max(6px,env(safe-area-inset-right))
          max(4px,env(safe-area-inset-bottom))
          max(6px,env(safe-area-inset-left));
  display:flex;
  flex-direction:column;
  gap:4px;
  overflow:hidden;
}

header{
  height:36px;
  flex:0 0 36px;
  padding:3px 9px;
  display:flex;
  align-items:center;
  justify-content:space-between;
  background:#090d13;
  border:1px solid var(--line);
  border-radius:10px;
}

.title-area{
  display:flex;
  align-items:baseline;
  gap:10px;
  min-width:0;
}

.system-label{
  color:#64748b;
  font-family:"Courier New",monospace;
  font-size:8px;
  font-weight:900;
  letter-spacing:1.5px;
  white-space:nowrap;
}

h1{
  margin:0;
  color:#fff;
  font-size:clamp(15px,2.8vw,22px);
  font-weight:950;
  white-space:nowrap;
}

.all-reset{
  padding:5px 10px;
  background:#171f2b;
  color:#fff;
  border:1px solid #3a4656;
  border-radius:7px;
  font-size:10px;
  font-weight:900;
  cursor:pointer;
  touch-action:manipulation;
}

.all-reset:active,
.player-reset:active{
  transform:scale(.96);
  background:#263244;
}

.battle{
  flex:1;
  min-height:0;
  display:grid;
  grid-template-columns:1fr 1fr;
  gap:4px;
  overflow:hidden;
}

.player{
  min-width:0;
  min-height:0;
  padding:4px;
  background:var(--panel);
  border:1px solid var(--line);
  border-radius:12px;
  display:flex;
  flex-direction:column;
  overflow:hidden;
  position:relative;
}

.player1{
  border-top:3px solid #278cff;
}

.player2{
  border-top:3px solid #ff3f61;
}

.player-head{
  width:100%;
  flex:0 0 auto;
  display:grid;
  grid-template-columns:1fr auto;
  gap:4px;
  align-items:end;
}

.select-frame{
  min-width:0;
  position:relative;
}

.select-title{
  margin:0 0 2px 2px;
  color:#7d8998;
  font-family:"Courier New",monospace;
  font-size:7px;
  font-weight:900;
  letter-spacing:1px;
}

.pokemon-select{
  width:100%;
  height:34px;
  padding:3px 27px 3px 6px;
  appearance:none;
  -webkit-appearance:none;
  background:#0c121b;
  color:#fff;
  border:1px solid #354255;
  border-radius:7px;
  font-size:clamp(12px,2.3vw,18px);
  font-weight:950;
  text-align:center;
  text-align-last:center;
  outline:none;
  touch-action:manipulation;
}

.pokemon-select option{
  background:#fff;
  color:#000;
  font-weight:800;
}

.select-frame::after{
  content:"▼";
  position:absolute;
  right:8px;
  bottom:10px;
  color:#fff;
  font-size:8px;
  pointer-events:none;
}

.player-reset{
  height:34px;
  padding:3px 7px;
  background:#171f2b;
  color:#fff;
  border:1px solid #3a4656;
  border-radius:7px;
  font-size:8px;
  font-weight:900;
  cursor:pointer;
  touch-action:manipulation;
}

.digital-unit{
  width:100%;
  flex:1;
  min-height:0;
  margin-top:3px;
  padding:2px;
  background:var(--monitor);
  border:1px solid var(--line);
  border-radius:10px;
  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;
  overflow:hidden;
  position:relative;
}

.digital-header{
  width:100%;
  padding:0 4px;
  display:flex;
  justify-content:space-between;
  color:#69778a;
  font-family:"Courier New",monospace;
  font-size:7px;
  font-weight:900;
  letter-spacing:.8px;
}

.status-area{
  display:flex;
  align-items:center;
  gap:3px;
}

.status-light{
  width:5px;
  height:5px;
  background:var(--green);
  border-radius:50%;
}

.dial-container{
  height:min(69vh,calc(100dvh - 86px));
  max-height:100%;
  max-width:100%;
  aspect-ratio:1/1;
  margin:0 auto;
  position:relative;
  flex:0 1 auto;
  touch-action:none;
  overscroll-behavior:none;
  cursor:grab;
}

.dial-container.dragging{
  cursor:grabbing;
}

.hp-dial{
  width:100%;
  height:100%;
  display:block;
  overflow:visible;
  touch-action:none;
}

.outer-case{
  fill:#05080c;
  stroke:#263447;
  stroke-width:5;
}

.outer-detail{
  fill:none;
  stroke:#111924;
  stroke-width:10;
}

.dial-track{
  fill:none;
  stroke:#1d2734;
  stroke-width:24;
}

.dial-progress{
  fill:none;
  stroke:var(--green);
  stroke-width:24;
  transform:rotate(-90deg);
  transform-origin:180px 180px;
}

.dial-center{
  fill:#05080c;
  stroke:#28374a;
  stroke-width:4;
}

.dial-center-inner{
  fill:#090e14;
  stroke:#162333;
  stroke-width:2;
}

.dial-hp-label{
  fill:#69778a;
  font-family:"Courier New",monospace;
  font-size:17px;
  font-weight:900;
  letter-spacing:6px;
  text-anchor:middle;
}

.dial-hp-value{
  fill:var(--green);
  font-family:"Courier New",monospace;
  font-size:82px;
  font-weight:950;
  font-variant-numeric:tabular-nums;
  text-anchor:middle;
}

.dial-percent{
  fill:#8290a3;
  font-family:"Courier New",monospace;
  font-size:13px;
  font-weight:900;
  text-anchor:middle;
}

.scale-line{
  stroke:#46566a;
  stroke-width:2;
  stroke-linecap:round;
  pointer-events:none;
}

.scale-line.major{
  stroke:#8d9bad;
  stroke-width:4;
}

.scale-label{
  fill:#aebccc;
  font-family:"Courier New",monospace;
  font-size:12px;
  font-weight:900;
  text-anchor:middle;
  pointer-events:none;
}

.dial-handle{
  fill:#05080c;
  stroke:var(--green);
  stroke-width:6;
  pointer-events:none;
}

.handle-center{
  fill:var(--green);
  pointer-events:none;
}

.handle-line{
  stroke:#05080c;
  stroke-width:2;
  stroke-linecap:round;
  pointer-events:none;
}

.evolution-button{
  position:absolute;
  right:8px;
  bottom:19px;
  z-index:10;
  width:54px;
  height:54px;
  padding:0;
  display:none;
  align-items:center;
  justify-content:center;
  background:#16a34a;
  color:#fff;
  border:3px solid #86efac;
  border-radius:50%;
  box-shadow:0 4px 0 #075d2b,0 6px 10px rgba(0,0,0,.4);
  font-size:13px;
  font-weight:950;
  cursor:pointer;
  touch-action:manipulation;
}

.evolution-button.show{
  display:flex;
}

.evolution-button:active{
  transform:translateY(3px) scale(.96);
  box-shadow:0 1px 0 #075d2b;
}

.evolution-button:disabled{
  display:none;
}

.evolution-overlay{
  position:absolute;
  inset:0;
  z-index:20;
  display:none;
  align-items:center;
  justify-content:center;
  overflow:hidden;
  background:rgba(1,6,8,.68);
  pointer-events:none;
}

.evolution-overlay.active{
  display:flex;
  animation:evolutionBackground 1.5s ease both;
}

.evolution-ring{
  position:absolute;
  width:30px;
  height:30px;
  border:5px solid #fff;
  border-radius:50%;
  opacity:0;
}

.evolution-overlay.active .evolution-ring{
  animation:evolutionRing 1.35s ease-out both;
}

.evolution-ring.second{
  animation-delay:.18s !important;
}

.evolution-ring.third{
  animation-delay:.36s !important;
}

.evolution-text{
  position:relative;
  z-index:2;
  color:#fff;
  font-family:"Courier New",monospace;
  font-size:clamp(18px,4vw,32px);
  font-weight:950;
  letter-spacing:2px;
  text-align:center;
  opacity:0;
  text-shadow:0 2px 0 #087a39,0 0 12px #22c55e;
}

.evolution-overlay.active .evolution-text{
  animation:evolutionText 1.5s ease both;
}

.digital-unit.evolving{
  animation:monitorEvolution 1.5s ease;
}

.digital-unit.evolving .dial-progress{
  animation:gaugeEvolution 1.5s ease;
}

.digital-unit.evolving .dial-hp-value{
  animation:numberEvolution 1.5s ease;
}

@keyframes evolutionBackground{
  0%,100%{background:rgba(1,6,8,0)}
  25%{background:rgba(1,15,8,.78)}
  65%{background:rgba(4,35,17,.82)}
}

@keyframes evolutionRing{
  0%{width:30px;height:30px;opacity:0}
  15%{opacity:1}
  70%{opacity:.8;border-color:#22c55e}
  100%{width:550px;height:550px;opacity:0;border-width:2px}
}

@keyframes evolutionText{
  0%{opacity:0;transform:scale(.55);filter:blur(8px)}
  30%{opacity:1;transform:scale(1.15);filter:blur(0)}
  65%{opacity:1;transform:scale(1)}
  100%{opacity:0;transform:scale(1.06)}
}

@keyframes monitorEvolution{
  0%,100%{border-color:#29364a}
  25%{border-color:#fff}
  50%{
    border-color:#22c55e;
    box-shadow:inset 0 0 25px rgba(34,197,94,.35),0 0 18px rgba(34,197,94,.65)
  }
}

@keyframes gaugeEvolution{
  0%,100%{opacity:1}
  20%{opacity:.15;stroke:#fff}
  40%{opacity:1;stroke:#22c55e}
  60%{opacity:.25}
}

@keyframes numberEvolution{
  0%,100%{opacity:1;transform:scale(1);transform-origin:center}
  25%{opacity:0;transform:scale(.55)}
  55%{opacity:1;transform:scale(1.2)}
}

.dial-guide{
  margin-top:-8px;
  padding:1px;
  color:#728095;
  font-family:"Courier New",monospace;
  font-size:7px;
  font-weight:900;
  text-align:center;
  white-space:nowrap;
}

.limit-indicator{
  width:100%;
  padding:2px 5px 1px;
  display:flex;
  justify-content:space-between;
  border-top:1px solid #1f2b3b;
  color:#59697e;
  font-family:"Courier New",monospace;
  font-size:7px;
  font-weight:900;
}

.limit-indicator strong{
  color:#b7c4d4;
}

.rotate-message{
  display:none;
}

@media(max-height:450px) and (orientation:landscape){
  .app{
    gap:2px;
    padding-top:max(2px,env(safe-area-inset-top));
    padding-bottom:max(2px,env(safe-area-inset-bottom));
  }

  header{
    height:29px;
    flex-basis:29px;
  }

  .system-label,
  .select-title,
  .dial-guide,
  .limit-indicator{
    display:none;
  }

  h1{
    font-size:14px;
  }

  .pokemon-select,
  .player-reset{
    height:28px;
  }

  .digital-unit{
    margin-top:2px;
  }

  .dial-container{
    height:min(70vh,calc(100dvh - 65px));
  }

  .evolution-button{
    bottom:7px;
    width:46px;
    height:46px;
    font-size:11px;
  }
}

@media(orientation:portrait){
  .app{
    display:none;
  }

  .rotate-message{
    position:fixed;
    inset:0;
    display:flex;
    flex-direction:column;
    align-items:center;
    justify-content:center;
    padding:25px;
    background:#05070a;
    color:#fff;
    text-align:center;
  }

  .rotate-icon{
    margin-bottom:18px;
    font-size:65px;
    transform:rotate(90deg);
  }

  .rotate-title{
    margin-bottom:10px;
    font-size:24px;
    font-weight:950;
  }

  .rotate-text{
    color:#8b9aae;
    font-size:14px;
    font-weight:800;
    line-height:1.7;
  }
}

@media(prefers-reduced-motion:reduce){
  *,
  *::before,
  *::after{
    animation-duration:.01ms !important;
    animation-iteration-count:1 !important;
  }
}
</style>
</head>

<body>

<div class="app">

  <header>
    <div class="title-area">
      <span class="system-label">DUAL DIGITAL LIFE COUNTER</span>
      <h1>プラコロ対戦カウンター</h1>
    </div>

    <button
      id="allResetBtn"
      class="all-reset"
      type="button"
    >
      ALL RESET
    </button>
  </header>

  <main class="battle">

    <section class="player player1" data-player="0">

      <div class="player-head">

        <div class="select-frame">
          <div class="select-title">PLAYER 1 MONSTER</div>

          <select
            class="pokemon-select"
            aria-label="プレイヤー1のポケモン"
          >
            <option value="">ポケモンを選択</option>
          </select>
        </div>

        <button class="player-reset" type="button">
          RESET
        </button>

      </div>

      <div class="digital-unit">

        <div class="digital-header">
          <span class="monitor-name">PLAYER 1</span>

          <span class="status-area">
            <span class="status-light"></span>
            ACTIVE
          </span>
        </div>

        <div
          class="dial-container"
          role="slider"
          aria-label="プレイヤー1 HP"
          aria-valuemin="0"
          aria-valuemax="120"
          aria-valuenow="120"
          tabindex="0"
        >
          <svg class="hp-dial" viewBox="0 0 360 360">

            <circle class="outer-case" cx="180" cy="180" r="167"></circle>
            <circle class="outer-detail" cx="180" cy="180" r="153"></circle>
            <circle class="dial-track" cx="180" cy="180" r="137"></circle>
            <circle class="dial-progress" cx="180" cy="180" r="137"></circle>

            <g class="scale-lines"></g>
            <g class="scale-labels"></g>

            <circle class="dial-center" cx="180" cy="180" r="96"></circle>
            <circle class="dial-center-inner" cx="180" cy="180" r="87"></circle>

            <text class="dial-hp-label" x="180" y="144">HP</text>
            <text class="dial-hp-value" x="180" y="220">120</text>
            <text class="dial-percent" x="180" y="252">100%</text>

            <circle class="dial-handle" cx="180" cy="43" r="16"></circle>
            <circle class="handle-center" cx="180" cy="43" r="6"></circle>
            <line class="handle-line" x1="180" y1="39" x2="180" y2="47"></line>

          </svg>
        </div>

        <button class="evolution-button" type="button">
          進化
        </button>

        <div class="dial-guide">
          タップで移動 ／ RINGをドラッグ
        </div>

        <div class="limit-indicator">
          <span>MIN <strong>000</strong></span>
          <span>STEP <strong>010</strong></span>
          <span>MAX <strong class="max-hp-label">120</strong></span>
        </div>

        <div class="evolution-overlay">
          <div class="evolution-ring"></div>
          <div class="evolution-ring second"></div>
          <div class="evolution-ring third"></div>
          <div class="evolution-text">EVOLUTION</div>
        </div>

      </div>

    </section>

    <section class="player player2" data-player="1">

      <div class="player-head">

        <div class="select-frame">
          <div class="select-title">PLAYER 2 MONSTER</div>

          <select
            class="pokemon-select"
            aria-label="プレイヤー2のポケモン"
          >
            <option value="">ポケモンを選択</option>
          </select>
        </div>

        <button class="player-reset" type="button">
          RESET
        </button>

      </div>

      <div class="digital-unit">

        <div class="digital-header">
          <span class="monitor-name">PLAYER 2</span>

          <span class="status-area">
            <span class="status-light"></span>
            ACTIVE
          </span>
        </div>

        <div
          class="dial-container"
          role="slider"
          aria-label="プレイヤー2 HP"
          aria-valuemin="0"
          aria-valuemax="120"
          aria-valuenow="120"
          tabindex="0"
        >
          <svg class="hp-dial" viewBox="0 0 360 360">

            <circle class="outer-case" cx="180" cy="180" r="167"></circle>
            <circle class="outer-detail" cx="180" cy="180" r="153"></circle>
            <circle class="dial-track" cx="180" cy="180" r="137"></circle>
            <circle class="dial-progress" cx="180" cy="180" r="137"></circle>

            <g class="scale-lines"></g>
            <g class="scale-labels"></g>

            <circle class="dial-center" cx="180" cy="180" r="96"></circle>
            <circle class="dial-center-inner" cx="180" cy="180" r="87"></circle>

            <text class="dial-hp-label" x="180" y="144">HP</text>
            <text class="dial-hp-value" x="180" y="220">120</text>
            <text class="dial-percent" x="180" y="252">100%</text>

            <circle class="dial-handle" cx="180" cy="43" r="16"></circle>
            <circle class="handle-center" cx="180" cy="43" r="6"></circle>
            <line class="handle-line" x1="180" y1="39" x2="180" y2="47"></line>

          </svg>
        </div>

        <button class="evolution-button" type="button">
          進化
        </button>

        <div class="dial-guide">
          タップで移動 ／ RINGをドラッグ
        </div>

        <div class="limit-indicator">
          <span>MIN <strong>000</strong></span>
          <span>STEP <strong>010</strong></span>
          <span>MAX <strong class="max-hp-label">120</strong></span>
        </div>

        <div class="evolution-overlay">
          <div class="evolution-ring"></div>
          <div class="evolution-ring second"></div>
          <div class="evolution-ring third"></div>
          <div class="evolution-text">EVOLUTION</div>
        </div>

      </div>

    </section>

  </main>

</div>

<div class="rotate-message">
  <div class="rotate-icon">📱</div>

  <div class="rotate-title">
    スマートフォンを横向きにしてください
  </div>

  <div class="rotate-text">
    2人用カウンターは<br>
    横画面で使用できます
  </div>
</div>

<script>
const pokemonData = [
  "ピカチュウ",
  "ヒトカゲ",
  "フシギダネ",
  "ゼニガメ",
  "ミュウ",
  "イーブイ",
  "ルカリオ",
  "メタグロス",
  "ゲンガー",
  "イワーク",
  "ベトベター",
  "カイロス",
  "ファイヤー",
  "サンダー",
  "フリーザー",
  "ヤミラミ",
  "ゲッコウガ",
  "ボスゴドラ",
  "レックウザ",
  "カヌチャン",
  "ラッキー"
];

const evolutionData = {
  "ヒトカゲ":[
    {name:"ヒトカゲ",maxHp:120},
    {name:"リザード",maxHp:150},
    {name:"リザードン",maxHp:180}
  ],
  "フシギダネ":[
    {name:"フシギダネ",maxHp:120},
    {name:"フシギソウ",maxHp:150},
    {name:"フシギバナ",maxHp:180}
  ],
  "ゼニガメ":[
    {name:"ゼニガメ",maxHp:120},
    {name:"カメール",maxHp:150},
    {name:"カメックス",maxHp:180}
  ]
};

const BASE_MAX_HP = 120;
const MIN_HP = 0;
const HP_STEP = 10;
const TAP_MOVE_THRESHOLD = 8;

const CENTER_X = 180;
const CENTER_Y = 180;
const DIAL_RADIUS = 137;
const HANDLE_RADIUS = DIAL_RADIUS;
const SCALE_INNER_RADIUS = 153;
const SCALE_OUTER_RADIUS = 161;
const LABEL_RADIUS = 169;
const CIRCUMFERENCE = 2 * Math.PI * DIAL_RADIUS;

const players = [createPlayer(0),createPlayer(1)];

function createPlayer(index){

  const root = document.querySelector(
    `.player[data-player="${index}"]`
  );

  return {
    index,
    root,

    hp:BASE_MAX_HP,
    maxHp:BASE_MAX_HP,

    selectedBasePokemon:"",
    evolutionStage:0,
    isEvolving:false,

    isDragging:false,
    pointerMoved:false,
    pointerStartX:0,
    pointerStartY:0,
    previousPointerAngle:null,
    accumulatedAngle:0,

    select:root.querySelector(".pokemon-select"),
    reset:root.querySelector(".player-reset"),
    digitalUnit:root.querySelector(".digital-unit"),
    monitorName:root.querySelector(".monitor-name"),

    dialContainer:root.querySelector(".dial-container"),
    svg:root.querySelector(".hp-dial"),
    progress:root.querySelector(".dial-progress"),

    hpValue:root.querySelector(".dial-hp-value"),
    hpPercent:root.querySelector(".dial-percent"),

    handle:root.querySelector(".dial-handle"),
    handleCenter:root.querySelector(".handle-center"),
    handleLine:root.querySelector(".handle-line"),

    scaleLines:root.querySelector(".scale-lines"),
    scaleLabels:root.querySelector(".scale-labels"),

    maxHpLabel:root.querySelector(".max-hp-label"),

    evolutionButton:root.querySelector(".evolution-button"),
    evolutionOverlay:root.querySelector(".evolution-overlay"),
    evolutionText:root.querySelector(".evolution-text")
  };
}

players.forEach(player => {

  pokemonData.forEach(name => {

    const option = document.createElement("option");

    option.value = name;
    option.textContent = name;

    player.select.appendChild(option);
  });

});

function polarToCartesian(
  centerX,
  centerY,
  radius,
  angle
){
  const radians = angle * Math.PI / 180;

  return {
    x:centerX + radius * Math.cos(radians),
    y:centerY + radius * Math.sin(radians)
  };
}

function getEvolutionLine(player){
  return evolutionData[player.selectedBasePokemon] || null;
}

function getCurrentPokemonName(player){

  const line = getEvolutionLine(player);

  if(!line){
    return player.selectedBasePokemon;
  }

  return line[player.evolutionStage].name;
}

function hasNextEvolution(player){

  const line = getEvolutionLine(player);

  return Boolean(
    line &&
    player.evolutionStage < line.length - 1
  );
}

function getDegreesPerStep(player){
  return 360 / (player.maxHp / HP_STEP);
}

function hpToAngle(player,value){
  return -90 + (value / player.maxHp) * 360;
}

function getHpColor(hp){

  if(hp <= 40){
    return "#ff334f";
  }

  if(hp <= 80){
    return "#ffd400";
  }

  return "#00d47b";
}

function restoreOptionNames(player){

  Array.from(player.select.options).forEach(option => {

    if(evolutionData[option.value]){
      option.textContent = option.value;
    }

  });
}

function updateSelectedOption(player){

  const option =
    player.select.options[player.select.selectedIndex];

  if(option && player.select.value){
    option.textContent = getCurrentPokemonName(player);
  }
}

function renderEvolutionButton(player){

  const show =
    hasNextEvolution(player) &&
    !player.isEvolving;

  player.evolutionButton.classList.toggle(
    "show",
    show
  );

  player.evolutionButton.disabled = !show;
}

function createDialScale(player){

  player.scaleLines.innerHTML = "";
  player.scaleLabels.innerHTML = "";

  for(
    let value = 0;
    value < player.maxHp;
    value += HP_STEP
  ){
    const angle =
      -90 + (value / player.maxHp) * 360;

    const isMajor =
      value % 30 === 0;

    const innerRadius =
      isMajor
        ? SCALE_INNER_RADIUS - 6
        : SCALE_INNER_RADIUS;

    const start = polarToCartesian(
      CENTER_X,
      CENTER_Y,
      innerRadius,
      angle
    );

    const end = polarToCartesian(
      CENTER_X,
      CENTER_Y,
      SCALE_OUTER_RADIUS,
      angle
    );

    const line = document.createElementNS(
      "http://www.w3.org/2000/svg",
      "line"
    );

    line.setAttribute("x1",start.x);
    line.setAttribute("y1",start.y);
    line.setAttribute("x2",end.x);
    line.setAttribute("y2",end.y);

    line.setAttribute(
      "class",
      isMajor
        ? "scale-line major"
        : "scale-line"
    );

    player.scaleLines.appendChild(line);

    if(isMajor){

      const position = polarToCartesian(
        CENTER_X,
        CENTER_Y,
        LABEL_RADIUS,
        angle
      );

      const text = document.createElementNS(
        "http://www.w3.org/2000/svg",
        "text"
      );

      text.setAttribute("x",position.x);
      text.setAttribute("y",position.y + 4);
      text.setAttribute("class","scale-label");

      text.textContent =
        value === 0
          ? "0/" + player.maxHp
          : value;

      player.scaleLabels.appendChild(text);
    }
  }
}

function renderHP(player){

  player.hp = Math.max(
    MIN_HP,
    Math.min(player.maxHp,player.hp)
  );

  const ratio =
    player.hp / player.maxHp;

  const color =
    getHpColor(player.hp);

  const dashOffset =
    CIRCUMFERENCE * (1 - ratio);

  const angle =
    hpToAngle(player,player.hp);

  const position = polarToCartesian(
    CENTER_X,
    CENTER_Y,
    HANDLE_RADIUS,
    angle
  );

  player.hpValue.textContent =
    String(player.hp).padStart(3,"0");

  player.hpPercent.textContent =
    Math.round(ratio * 100) + "%";

  player.maxHpLabel.textContent =
    player.maxHp;

  player.dialContainer.setAttribute(
    "aria-valuemax",
    player.maxHp
  );

  player.dialContainer.setAttribute(
    "aria-valuenow",
    player.hp
  );

  player.dialContainer.setAttribute(
    "aria-valuetext",
    `HP ${player.hp}、最大HP ${player.maxHp}`
  );

  player.progress.style.strokeDasharray =
    CIRCUMFERENCE;

  player.progress.style.strokeDashoffset =
    dashOffset;

  player.progress.style.stroke =
    color;

  player.hpValue.style.fill =
    color;

  player.handle.style.stroke =
    color;

  player.handleCenter.style.fill =
    color;

  player.handle.setAttribute(
    "cx",
    position.x
  );

  player.handle.setAttribute(
    "cy",
    position.y
  );

  player.handleCenter.setAttribute(
    "cx",
    position.x
  );

  player.handleCenter.setAttribute(
    "cy",
    position.y
  );

  const inner = polarToCartesian(
    CENTER_X,
    CENTER_Y,
    HANDLE_RADIUS - 4,
    angle
  );

  const outer = polarToCartesian(
    CENTER_X,
    CENTER_Y,
    HANDLE_RADIUS + 4,
    angle
  );

  player.handleLine.setAttribute("x1",inner.x);
  player.handleLine.setAttribute("y1",inner.y);
  player.handleLine.setAttribute("x2",outer.x);
  player.handleLine.setAttribute("y2",outer.y);
}

function renderAll(player){

  createDialScale(player);
  renderHP(player);
  updateSelectedOption(player);
  renderEvolutionButton(player);

  player.monitorName.textContent =
    player.selectedBasePokemon
      ? getCurrentPokemonName(player)
      : `PLAYER ${player.index + 1}`;
}

function getPointerPosition(player,event){

  const rect =
    player.svg.getBoundingClientRect();

  return {
    x:(event.clientX - rect.left) * (360 / rect.width),
    y:(event.clientY - rect.top) * (360 / rect.height)
  };
}

function getPointerAngle(player,event){

  const position =
    getPointerPosition(player,event);

  return Math.atan2(
    position.y - CENTER_Y,
    position.x - CENTER_X
  ) * 180 / Math.PI;
}

function jumpToTappedHp(player,event){

  if(player.isEvolving){
    return;
  }

  const position =
    getPointerPosition(player,event);

  const dx =
    position.x - CENTER_X;

  const dy =
    position.y - CENTER_Y;

  const distance =
    Math.sqrt(dx * dx + dy * dy);

  if(
    distance < DIAL_RADIUS - 45 ||
    distance > DIAL_RADIUS + 45
  ){
    return;
  }

  const angle =
    Math.atan2(dy,dx) * 180 / Math.PI;

  let clockwiseAngle =
    angle + 90;

  if(clockwiseAngle < 0){
    clockwiseAngle += 360;
  }

  if(clockwiseAngle >= 360){
    clockwiseAngle -= 360;
  }

  const degreesPerStep =
    getDegreesPerStep(player);

  let step =
    Math.round(
      clockwiseAngle / degreesPerStep
    );

  const totalSteps =
    player.maxHp / HP_STEP;

  if(step >= totalSteps){
    step = 0;
  }

  let nextHp =
    step * HP_STEP;

  if(step === 0){
    nextHp =
      player.hp >= player.maxHp / 2
        ? player.maxHp
        : 0;
  }

  player.hp = nextHp;
  player.accumulatedAngle = 0;
  player.previousPointerAngle = null;

  renderHP(player);
}

function updateHpFromDrag(player,event){

  if(player.isEvolving){
    return;
  }

  const currentAngle =
    getPointerAngle(player,event);

  if(player.previousPointerAngle === null){

    player.previousPointerAngle =
      currentAngle;

    return;
  }

  let difference =
    currentAngle -
    player.previousPointerAngle;

  if(difference > 180){
    difference -= 360;
  }

  if(difference < -180){
    difference += 360;
  }

  player.accumulatedAngle += difference;

  const stepAngle =
    getDegreesPerStep(player);

  while(
    player.accumulatedAngle >= stepAngle
  ){
    if(player.hp < player.maxHp){

      player.hp = Math.min(
        player.maxHp,
        player.hp + HP_STEP
      );

      renderHP(player);
    }

    player.accumulatedAngle -= stepAngle;
  }

  while(
    player.accumulatedAngle <= -stepAngle
  ){
    if(player.hp > MIN_HP){

      player.hp = Math.max(
        MIN_HP,
        player.hp - HP_STEP
      );

      renderHP(player);
    }

    player.accumulatedAngle += stepAngle;
  }

  if(
    player.hp === player.maxHp &&
    player.accumulatedAngle > 0
  ){
    player.accumulatedAngle = 0;
  }

  if(
    player.hp === MIN_HP &&
    player.accumulatedAngle < 0
  ){
    player.accumulatedAngle = 0;
  }

  player.previousPointerAngle =
    currentAngle;
}

function finishPointer(
  player,
  event,
  allowTap
){
  if(
    player.isDragging &&
    allowTap &&
    !player.pointerMoved &&
    !player.isEvolving
  ){
    jumpToTappedHp(player,event);
  }

  player.isDragging = false;
  player.pointerMoved = false;
  player.previousPointerAngle = null;
  player.accumulatedAngle = 0;

  player.dialContainer.classList.remove(
    "dragging"
  );

  try{
    if(
      player.dialContainer.hasPointerCapture(
        event.pointerId
      )
    ){
      player.dialContainer.releasePointerCapture(
        event.pointerId
      );
    }
  }catch(error){}
}

function evolvePlayer(player){

  if(
    player.isEvolving ||
    !hasNextEvolution(player)
  ){
    return;
  }

  const line =
    getEvolutionLine(player);

  const nextStage =
    player.evolutionStage + 1;

  const next =
    line[nextStage];

  const damage =
    player.maxHp - player.hp;

  player.isEvolving = true;

  player.evolutionButton.disabled = true;

  player.evolutionButton.classList.remove(
    "show"
  );

  player.evolutionText.textContent =
    "EVOLUTION";

  player.evolutionOverlay.classList.add(
    "active"
  );

  player.digitalUnit.classList.add(
    "evolving"
  );

  window.setTimeout(
    () => {

      player.evolutionStage =
        nextStage;

      player.maxHp =
        next.maxHp;

      player.hp = Math.max(
        MIN_HP,
        player.maxHp - damage
      );

      updateSelectedOption(player);

      player.monitorName.textContent =
        next.name;

      createDialScale(player);
      renderHP(player);

      player.evolutionText.textContent =
        next.name;

    },
    650
  );

  window.setTimeout(
    () => {

      player.evolutionOverlay.classList.remove(
        "active"
      );

      player.digitalUnit.classList.remove(
        "evolving"
      );

      player.isEvolving = false;

      renderEvolutionButton(player);

    },
    1550
  );
}

function resetPlayer(player){

  if(player.isEvolving){
    return;
  }

  const selected =
    player.select.value;

  const option =
    player.select.options[
      player.select.selectedIndex
    ];

  if(option && selected){
    option.textContent = selected;
  }

  player.selectedBasePokemon = selected;
  player.evolutionStage = 0;
  player.maxHp = BASE_MAX_HP;
  player.hp = BASE_MAX_HP;

  player.previousPointerAngle = null;
  player.accumulatedAngle = 0;

  renderAll(player);
}

function setupPlayer(player){

  renderAll(player);

  player.select.addEventListener(
    "change",
    () => {

      if(player.isEvolving){
        return;
      }

      restoreOptionNames(player);

      player.selectedBasePokemon =
        player.select.value;

      player.evolutionStage = 0;
      player.maxHp = BASE_MAX_HP;
      player.hp = BASE_MAX_HP;

      renderAll(player);
    }
  );

  player.reset.addEventListener(
    "click",
    () => resetPlayer(player)
  );

  player.evolutionButton.addEventListener(
    "click",
    () => evolvePlayer(player)
  );

  player.dialContainer.addEventListener(
    "pointerdown",
    event => {

      if(player.isEvolving){
        return;
      }

      event.preventDefault();

      player.isDragging = true;
      player.pointerMoved = false;

      player.pointerStartX =
        event.clientX;

      player.pointerStartY =
        event.clientY;

      player.previousPointerAngle =
        getPointerAngle(player,event);

      player.accumulatedAngle = 0;

      player.dialContainer.classList.add(
        "dragging"
      );

      try{
        player.dialContainer.setPointerCapture(
          event.pointerId
        );
      }catch(error){}
    }
  );

  player.dialContainer.addEventListener(
    "pointermove",
    event => {

      if(
        !player.isDragging ||
        player.isEvolving
      ){
        return;
      }

      event.preventDefault();

      const dx =
        event.clientX -
        player.pointerStartX;

      const dy =
        event.clientY -
        player.pointerStartY;

      const distance =
        Math.sqrt(dx * dx + dy * dy);

      if(distance >= TAP_MOVE_THRESHOLD){
        player.pointerMoved = true;
      }

      if(player.pointerMoved){
        updateHpFromDrag(player,event);
      }
    },
    {passive:false}
  );

  player.dialContainer.addEventListener(
    "pointerup",
    event => {
      finishPointer(
        player,
        event,
        true
      );
    }
  );

  player.dialContainer.addEventListener(
    "pointercancel",
    event => {
      finishPointer(
        player,
        event,
        false
      );
    }
  );

  player.dialContainer.addEventListener(
    "touchmove",
    event => {
      event.preventDefault();
    },
    {passive:false}
  );

  player.dialContainer.addEventListener(
    "keydown",
    event => {

      if(player.isEvolving){
        return;
      }

      if(
        event.key === "ArrowLeft" ||
        event.key === "ArrowDown"
      ){
        event.preventDefault();

        player.hp = Math.max(
          MIN_HP,
          player.hp - HP_STEP
        );

        renderHP(player);
      }

      if(
        event.key === "ArrowRight" ||
        event.key === "ArrowUp"
      ){
        event.preventDefault();

        player.hp = Math.min(
          player.maxHp,
          player.hp + HP_STEP
        );

        renderHP(player);
      }
    }
  );
}

document
  .getElementById("allResetBtn")
  .addEventListener(
    "click",
    () => {
      players.forEach(resetPlayer);
    }
  );

players.forEach(setupPlayer);

document.addEventListener(
  "touchmove",
  event => {
    if(
      event.target.closest(
        ".dial-container"
      )
    ){
      event.preventDefault();
    }
  },
  {passive:false}
);
</script>

</body>
</html>
