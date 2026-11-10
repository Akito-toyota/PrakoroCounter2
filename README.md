<html lang="ja">
<head>
<meta charset="utf-8">

<meta
  name="viewport"
  content="width=device-width,initial-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover"
>

<title>プラコロ対戦カウンター</title>

<style>
:root{
  --page-bg:#05070a;
  --panel:#0b1017;
  --monitor:#070a0f;
  --line:#263447;
  --text:#eaf2ff;
  --muted:#718096;

  --green:#00d47b;
  --yellow:#ffd400;
  --red:#ff334f;

  --player1:#278cff;
  --player2:#ff3f61;

  /*
    JavaScriptでスマホブラウザの
    実際の表示高さを設定します。
  */
  --app-height:100dvh;
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
  padding:0;

  overflow:hidden;
  overscroll-behavior:none;
}

body{
  position:fixed;
  inset:0;

  width:100%;
  height:var(--app-height);

  margin:0;
  padding:0;

  overflow:hidden;
  overscroll-behavior:none;

  background:var(--page-bg);
  color:var(--text);

  font-family:
    system-ui,
    -apple-system,
    "Hiragino Kaku Gothic ProN",
    "Yu Gothic",
    sans-serif;

  user-select:none;
  -webkit-user-select:none;

  touch-action:none;
}

button,
select{
  font-family:inherit;
}

button{
  user-select:none;
  -webkit-user-select:none;
}

/* =========================
   全体
========================= */

.app{
  width:100%;
  height:var(--app-height);

  margin:0 auto;

  padding:
    max(3px,env(safe-area-inset-top))
    max(5px,env(safe-area-inset-right))
    max(3px,env(safe-area-inset-bottom))
    max(5px,env(safe-area-inset-left));

  display:flex;
  flex-direction:column;

  gap:3px;

  overflow:hidden;
}

/* =========================
   ヘッダー
========================= */

.header{
  width:100%;
  height:34px;
  flex:0 0 34px;

  padding:3px 8px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  gap:8px;

  background:#090d13;

  border:1px solid var(--line);
  border-radius:9px;
}

.title-area{
  min-width:0;

  display:flex;
  align-items:baseline;

  gap:8px;
}

.system-label{
  color:#64748b;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:7px;
  font-weight:900;

  letter-spacing:1.3px;

  white-space:nowrap;
}

h1{
  margin:0;

  color:#ffffff;

  font-size:clamp(14px,2.5vw,21px);
  font-weight:950;

  line-height:1;

  white-space:nowrap;
}

/* =========================
   ALL RESET
========================= */

.all-reset{
  flex-shrink:0;

  min-height:26px;

  padding:4px 9px;

  background:#7c2d12;
  color:#ffffff;

  border:1px solid #ea580c;
  border-radius:6px;

  font-size:9px;
  font-weight:900;

  cursor:pointer;
  touch-action:manipulation;
}

.all-reset:active,
.player-reset:active,
.evolution-button:active{
  transform:scale(.95);
}

/* =========================
   対戦エリア
========================= */

.battle{
  width:100%;

  flex:1 1 auto;
  min-height:0;

  display:grid;
  grid-template-columns:1fr 1fr;

  gap:3px;

  overflow:hidden;
}

/* =========================
   プレイヤーパネル
========================= */

.player{
  min-width:0;
  min-height:0;

  padding:3px;

  background:var(--panel);

  border:1px solid var(--line);
  border-top:3px solid var(--player1);
  border-radius:10px;

  display:flex;
  flex-direction:column;

  overflow:hidden;
}

.player.player2{
  border-top-color:var(--player2);
}

/* =========================
   プレイヤー上部
========================= */

.player-head{
  width:100%;
  height:32px;
  flex:0 0 32px;

  display:grid;
  grid-template-columns:1fr auto;

  gap:3px;
  align-items:stretch;
}

.select-frame{
  min-width:0;

  position:relative;
}

/* =========================
   ポケモン選択
========================= */

.pokemon-select{
  width:100%;
  height:32px;

  padding:3px 27px 3px 6px;

  appearance:none;
  -webkit-appearance:none;

  background:#0c121b;
  color:#ffffff;

  border:1px solid #354255;
  border-radius:6px;

  font-size:clamp(12px,2.2vw,18px);
  font-weight:950;

  text-align:center;
  text-align-last:center;

  outline:none;

  cursor:pointer;
  touch-action:manipulation;
}

.pokemon-select option{
  background:#ffffff;
  color:#000000;

  font-weight:800;
}

.player1 .pokemon-select:focus{
  border-color:var(--player1);
}

.player2 .pokemon-select:focus{
  border-color:var(--player2);
}

.select-frame::after{
  content:"▼";

  position:absolute;

  right:8px;
  top:50%;

  color:#ffffff;

  font-size:8px;

  pointer-events:none;

  transform:translateY(-50%);
}

/* =========================
   個別RESET
========================= */

.player-reset{
  height:32px;

  padding:3px 7px;

  background:#171f2b;
  color:#ffffff;

  border:1px solid #3a4656;
  border-radius:6px;

  font-size:8px;
  font-weight:900;

  cursor:pointer;
  touch-action:manipulation;
}

/* =========================
   HPモニター
========================= */

.digital-unit{
  width:100%;

  flex:1 1 auto;
  min-height:0;

  margin-top:3px;
  padding:2px;

  background:var(--monitor);

  border:1px solid var(--line);
  border-radius:9px;

  display:flex;
  flex-direction:column;
  align-items:center;
  justify-content:center;

  overflow:hidden;

  position:relative;
}

/* =========================
   モニターヘッダー
========================= */

.digital-header{
  width:100%;
  height:14px;
  flex:0 0 14px;

  padding:0 4px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  color:#69778a;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:6px;
  font-weight:900;

  letter-spacing:.7px;
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

/* =========================
   円形ダイヤル

   実際の表示高さに応じて
   自動的に縮小します。
========================= */

.dial-container{
  height:min(
    calc(var(--app-height) * .67),
    calc(var(--app-height) - 103px),
    410px
  );

  max-height:100%;
  max-width:100%;

  aspect-ratio:1 / 1;

  flex:0 1 auto;

  margin:0 auto;

  position:relative;

  user-select:none;
  -webkit-user-select:none;

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

  user-select:none;
  -webkit-user-select:none;

  touch-action:none;
  overscroll-behavior:none;
}

/* =========================
   円外装
========================= */

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

/* =========================
   HPゲージ
========================= */

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

/* =========================
   中央
========================= */

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

/* =========================
   HP表示
========================= */

.dial-hp-label{
  fill:#69778a;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:17px;
  font-weight:900;

  letter-spacing:6px;

  text-anchor:middle;
}

.dial-hp-value{
  fill:var(--green);

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:82px;
  font-weight:950;

  text-anchor:middle;

  font-variant-numeric:tabular-nums;
}

.dial-unit-label{
  fill:#526175;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:11px;
  font-weight:900;

  letter-spacing:3px;

  text-anchor:middle;
}

.dial-percent{
  fill:#8290a3;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:13px;
  font-weight:900;

  text-anchor:middle;
}

/* =========================
   目盛り
========================= */

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

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:12px;
  font-weight:900;

  text-anchor:middle;

  pointer-events:none;
}

/* =========================
   操作ノブ
========================= */

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

/* =========================
   進化ボタン
========================= */

.evolution-button{
  position:absolute;

  right:6px;
  bottom:18px;

  z-index:10;

  width:48px;
  height:48px;

  padding:0;

  display:none;
  align-items:center;
  justify-content:center;

  background:#16a34a;
  color:#ffffff;

  border:3px solid #86efac;
  border-radius:50%;

  box-shadow:
    0 3px 0 #075d2b,
    0 5px 8px rgba(0,0,0,.4);

  font-size:11px;
  font-weight:950;

  cursor:pointer;
  touch-action:manipulation;
}

.evolution-button.show{
  display:flex;
}

.evolution-button:active{
  transform:
    translateY(3px)
    scale(.96);

  box-shadow:
    0 1px 0 #075d2b;
}

.evolution-button:disabled{
  display:none;
}

/* =========================
   進化演出
========================= */

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

  animation:
    evolutionBackground
    1.5s ease both;
}

.evolution-ring{
  position:absolute;

  width:30px;
  height:30px;

  border:5px solid #ffffff;
  border-radius:50%;

  opacity:0;
}

.evolution-overlay.active .evolution-ring{
  animation:
    evolutionRing
    1.35s ease-out both;
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

  color:#ffffff;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:clamp(18px,4vw,32px);
  font-weight:950;

  letter-spacing:2px;

  text-align:center;

  opacity:0;

  text-shadow:
    0 2px 0 #087a39,
    0 0 12px #22c55e;
}

.evolution-overlay.active .evolution-text{
  animation:
    evolutionText
    1.5s ease both;
}

.digital-unit.evolving{
  animation:
    monitorEvolution
    1.5s ease;
}

.digital-unit.evolving .dial-progress{
  animation:
    gaugeEvolution
    1.5s ease;
}

.digital-unit.evolving .dial-hp-value{
  animation:
    numberEvolution
    1.5s ease;
}

@keyframes evolutionBackground{

  0%{
    background:rgba(1,6,8,0);
  }

  25%{
    background:rgba(1,15,8,.78);
  }

  65%{
    background:rgba(4,35,17,.82);
  }

  100%{
    background:rgba(1,6,8,0);
  }

}

@keyframes evolutionRing{

  0%{
    width:30px;
    height:30px;

    opacity:0;
  }

  15%{
    opacity:1;
  }

  70%{
    opacity:.8;

    border-color:#22c55e;
  }

  100%{
    width:550px;
    height:550px;

    opacity:0;

    border-width:2px;
  }

}

@keyframes evolutionText{

  0%{
    opacity:0;

    transform:scale(.55);

    filter:blur(8px);
  }

  30%{
    opacity:1;

    transform:scale(1.15);

    filter:blur(0);
  }

  65%{
    opacity:1;

    transform:scale(1);
  }

  100%{
    opacity:0;

    transform:scale(1.06);
  }

}

@keyframes monitorEvolution{

  0%{
    border-color:#29364a;
  }

  25%{
    border-color:#ffffff;
  }

  50%{
    border-color:#22c55e;

    box-shadow:
      inset 0 0 25px rgba(34,197,94,.35),
      0 0 18px rgba(34,197,94,.65);
  }

  100%{
    border-color:#29364a;
  }

}

@keyframes gaugeEvolution{

  0%{
    opacity:1;
  }

  20%{
    opacity:.15;

    stroke:#ffffff;
  }

  40%{
    opacity:1;

    stroke:#22c55e;
  }

  60%{
    opacity:.25;
  }

  100%{
    opacity:1;
  }

}

@keyframes numberEvolution{

  0%{
    opacity:1;

    transform:scale(1);

    transform-origin:center;
  }

  25%{
    opacity:0;

    transform:scale(.55);
  }

  55%{
    opacity:1;

    transform:scale(1.2);
  }

  100%{
    opacity:1;

    transform:scale(1);
  }

}

/* =========================
   操作案内
========================= */

.dial-guide{
  width:100%;
  flex:0 0 auto;

  margin-top:-8px;

  padding:1px 3px;

  color:#728095;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:7px;
  font-weight:900;

  text-align:center;

  white-space:nowrap;
}

/* =========================
   下部表示
========================= */

.limit-indicator{
  width:100%;
  flex:0 0 auto;

  padding:2px 5px 1px;

  display:flex;
  align-items:center;
  justify-content:space-between;

  border-top:1px solid #1f2b3b;

  color:#59697e;

  font-family:
    "Courier New",
    ui-monospace,
    monospace;

  font-size:7px;
  font-weight:900;
}

.limit-indicator strong{
  color:#b7c4d4;
}

/* =========================
   横画面で高さが低い場合

   URLバー表示中にも
   収まりやすくします。
========================= */

@media(max-height:500px) and (orientation:landscape){

  .app{
    padding-top:
      max(2px,env(safe-area-inset-top));

    padding-bottom:
      max(2px,env(safe-area-inset-bottom));

    gap:2px;
  }

  .header{
    height:30px;
    flex-basis:30px;

    padding:2px 6px;
  }

  .system-label{
    display:none;
  }

  h1{
    font-size:14px;
  }

  .all-reset{
    min-height:24px;

    padding:3px 7px;

    font-size:8px;
  }

  .player{
    padding:2px;
  }

  .player-head{
    height:28px;
    flex-basis:28px;
  }

  .pokemon-select,
  .player-reset{
    height:28px;
  }

  .pokemon-select{
    font-size:12px;
  }

  .digital-unit{
    margin-top:2px;
    padding:1px;
  }

  .digital-header{
    height:11px;
    flex-basis:11px;

    font-size:5px;
  }

  .dial-container{
    height:min(
      calc(var(--app-height) * .66),
      calc(var(--app-height) - 90px),
      330px
    );
  }

  .dial-guide{
    display:none;
  }

  .limit-indicator{
    padding:1px 4px;

    font-size:6px;
  }

  .evolution-button{
    right:4px;
    bottom:6px;

    width:42px;
    height:42px;

    font-size:10px;
  }

}

/* =========================
   さらに高さが低い横画面
========================= */

@media(max-height:390px) and (orientation:landscape){

  .header{
    height:27px;
    flex-basis:27px;
  }

  h1{
    font-size:13px;
  }

  .all-reset{
    min-height:21px;
  }

  .player-head{
    height:25px;
    flex-basis:25px;
  }

  .pokemon-select,
  .player-reset{
    height:25px;
  }

  .pokemon-select{
    font-size:11px;
  }

  .dial-container{
    height:min(
      calc(var(--app-height) * .65),
      calc(var(--app-height) - 80px),
      285px
    );
  }

  .limit-indicator{
    display:none;
  }

  .evolution-button{
    bottom:3px;

    width:37px;
    height:37px;

    border-width:2px;

    font-size:9px;
  }

}

/* =========================
   縦画面案内
========================= */

.rotate-message{
  display:none;
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
    color:#ffffff;

    text-align:center;
  }

  .rotate-icon{
    margin-bottom:18px;

    font-size:64px;

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

  <header class="header">

    <div class="title-area">

      <span class="system-label">
        DUAL DIGITAL LIFE COUNTER
      </span>

      <h1>
        プラコロ対戦カウンター
      </h1>

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

    <!-- PLAYER 1 -->

    <section
      class="player player1"
      data-player="0"
    >

      <div class="player-head">

        <div class="select-frame">

          <select
            class="pokemon-select"
            aria-label="プレイヤー1のポケモン"
          >
            <option value="">
              PLAYER 1・ポケモン選択
            </option>
          </select>

        </div>

        <button
          class="player-reset"
          type="button"
        >
          RESET
        </button>

      </div>

      <div class="digital-unit">

        <div class="digital-header">

          <span class="monitor-name">
            PLAYER 1
          </span>

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
          aria-valuetext="HP 120"
          tabindex="0"
        >

          <svg
            class="hp-dial"
            viewBox="0 0 360 360"
            aria-hidden="true"
          >

            <circle
              class="outer-case"
              cx="180"
              cy="180"
              r="167"
            ></circle>

            <circle
              class="outer-detail"
              cx="180"
              cy="180"
              r="153"
            ></circle>

            <circle
              class="dial-track"
              cx="180"
              cy="180"
              r="137"
            ></circle>

            <circle
              class="dial-progress"
              cx="180"
              cy="180"
              r="137"
            ></circle>

            <g class="scale-lines"></g>

            <g class="scale-labels"></g>

            <circle
              class="dial-center"
              cx="180"
              cy="180"
              r="96"
            ></circle>

            <circle
              class="dial-center-inner"
              cx="180"
              cy="180"
              r="87"
            ></circle>

            <text
              class="dial-hp-label"
              x="180"
              y="140"
            >
              HP
            </text>

            <text
              class="dial-hp-value"
              x="180"
              y="218"
            >
              120
            </text>

            <text
              class="dial-unit-label"
              x="180"
              y="243"
            >
              LIFE POINT
            </text>

            <text
              class="dial-percent"
              x="180"
              y="268"
            >
              100%
            </text>

            <circle
              class="dial-handle"
              cx="180"
              cy="43"
              r="16"
            ></circle>

            <circle
              class="handle-center"
              cx="180"
              cy="43"
              r="6"
            ></circle>

            <line
              class="handle-line"
              x1="180"
              y1="39"
              x2="180"
              y2="47"
            ></line>

          </svg>

        </div>

        <button
          class="evolution-button"
          type="button"
        >
          進化
        </button>

        <div class="dial-guide">
          タップで移動 ／ RINGをドラッグ
        </div>

        <div class="limit-indicator">

          <span>
            MIN <strong>000</strong>
          </span>

          <span>
            STEP <strong>010</strong>
          </span>

          <span>
            MAX <strong class="max-hp-label">120</strong>
          </span>

        </div>

        <div class="evolution-overlay">

          <div class="evolution-ring"></div>

          <div class="evolution-ring second"></div>

          <div class="evolution-ring third"></div>

          <div class="evolution-text">
            EVOLUTION
          </div>

        </div>

      </div>

    </section>

    <!-- PLAYER 2 -->

    <section
      class="player player2"
      data-player="1"
    >

      <div class="player-head">

        <div class="select-frame">

          <select
            class="pokemon-select"
            aria-label="プレイヤー2のポケモン"
          >
            <option value="">
              PLAYER 2・ポケモン選択
            </option>
          </select>

        </div>

        <button
          class="player-reset"
          type="button"
        >
          RESET
        </button>

      </div>

      <div class="digital-unit">

        <div class="digital-header">

          <span class="monitor-name">
            PLAYER 2
          </span>

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
          aria-valuetext="HP 120"
          tabindex="0"
        >

          <svg
            class="hp-dial"
            viewBox="0 0 360 360"
            aria-hidden="true"
          >

            <circle
              class="outer-case"
              cx="180"
              cy="180"
              r="167"
            ></circle>

            <circle
              class="outer-detail"
              cx="180"
              cy="180"
              r="153"
            ></circle>

            <circle
              class="dial-track"
              cx="180"
              cy="180"
              r="137"
            ></circle>

            <circle
              class="dial-progress"
              cx="180"
              cy="180"
              r="137"
            ></circle>

            <g class="scale-lines"></g>

            <g class="scale-labels"></g>

            <circle
              class="dial-center"
              cx="180"
              cy="180"
              r="96"
            ></circle>

            <circle
              class="dial-center-inner"
              cx="180"
              cy="180"
              r="87"
            ></circle>

            <text
              class="dial-hp-label"
              x="180"
              y="140"
            >
              HP
            </text>

            <text
              class="dial-hp-value"
              x="180"
              y="218"
            >
              120
            </text>

            <text
              class="dial-unit-label"
              x="180"
              y="243"
            >
              LIFE POINT
            </text>

            <text
              class="dial-percent"
              x="180"
              y="268"
            >
              100%
            </text>

            <circle
              class="dial-handle"
              cx="180"
              cy="43"
              r="16"
            ></circle>

            <circle
              class="handle-center"
              cx="180"
              cy="43"
              r="6"
            ></circle>

            <line
              class="handle-line"
              x1="180"
              y1="39"
              x2="180"
              y2="47"
            ></line>

          </svg>

        </div>

        <button
          class="evolution-button"
          type="button"
        >
          進化
        </button>

        <div class="dial-guide">
          タップで移動 ／ RINGをドラッグ
        </div>

        <div class="limit-indicator">

          <span>
            MIN <strong>000</strong>
          </span>

          <span>
            STEP <strong>010</strong>
          </span>

          <span>
            MAX <strong class="max-hp-label">120</strong>
          </span>

        </div>

        <div class="evolution-overlay">

          <div class="evolution-ring"></div>

          <div class="evolution-ring second"></div>

          <div class="evolution-ring third"></div>

          <div class="evolution-text">
            EVOLUTION
          </div>

        </div>

      </div>

    </section>

  </main>

</div>

<div class="rotate-message">

  <div class="rotate-icon">
    📱
  </div>

  <div class="rotate-title">
    スマートフォンを横向きにしてください
  </div>

  <div class="rotate-text">
    2人用カウンターは<br>
    横画面で使用できます
  </div>

</div>

<script>
/* =========================
   ポケモンデータ
========================= */

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

/* =========================
   進化データ
========================= */

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

/* =========================
   基本設定
========================= */

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

const CIRCUMFERENCE =
  2 * Math.PI * DIAL_RADIUS;

/* =========================
   スマホ実表示高さ

   ブラウザ上部の
   サイト名・URL表示を除いた
   実際の高さを使用します。
========================= */

function updateAppHeight(){

  let height =
    window.innerHeight;

  if(
    window.visualViewport &&
    window.visualViewport.height
  ){

    height =
      window.visualViewport.height;

  }

  height =
    Math.floor(height);

  document.documentElement
    .style
    .setProperty(
      "--app-height",
      height + "px"
    );

}

updateAppHeight();

window.addEventListener(
  "resize",
  updateAppHeight
);

window.addEventListener(
  "orientationchange",
  () => {

    updateAppHeight();

    window.setTimeout(
      updateAppHeight,
      100
    );

    window.setTimeout(
      updateAppHeight,
      350
    );

  }
);

if(window.visualViewport){

  window.visualViewport.addEventListener(
    "resize",
    updateAppHeight
  );

  window.visualViewport.addEventListener(
    "scroll",
    updateAppHeight
  );

}

/* =========================
   プレイヤー状態
========================= */

const players = [
  createPlayerState(0),
  createPlayerState(1)
];

function createPlayerState(index){

  const root =
    document.querySelector(
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

    select:
      root.querySelector(
        ".pokemon-select"
      ),

    reset:
      root.querySelector(
        ".player-reset"
      ),

    digitalUnit:
      root.querySelector(
        ".digital-unit"
      ),

    monitorName:
      root.querySelector(
        ".monitor-name"
      ),

    dialContainer:
      root.querySelector(
        ".dial-container"
      ),

    svg:
      root.querySelector(
        ".hp-dial"
      ),

    progress:
      root.querySelector(
        ".dial-progress"
      ),

    hpValue:
      root.querySelector(
        ".dial-hp-value"
      ),

    hpPercent:
      root.querySelector(
        ".dial-percent"
      ),

    handle:
      root.querySelector(
        ".dial-handle"
      ),

    handleCenter:
      root.querySelector(
        ".handle-center"
      ),

    handleLine:
      root.querySelector(
        ".handle-line"
      ),

    scaleLines:
      root.querySelector(
        ".scale-lines"
      ),

    scaleLabels:
      root.querySelector(
        ".scale-labels"
      ),

    maxHpLabel:
      root.querySelector(
        ".max-hp-label"
      ),

    evolutionButton:
      root.querySelector(
        ".evolution-button"
      ),

    evolutionOverlay:
      root.querySelector(
        ".evolution-overlay"
      ),

    evolutionText:
      root.querySelector(
        ".evolution-text"
      )
  };

}

/* =========================
   選択肢の作成
========================= */

players.forEach(player => {

  pokemonData.forEach(name => {

    const option =
      document.createElement(
        "option"
      );

    option.value =
      name;

    option.textContent =
      name;

    player.select.appendChild(
      option
    );

  });

});

/* =========================
   円座標
========================= */

function polarToCartesian(
  centerX,
  centerY,
  radius,
  angle
){

  const radians =
    angle *
    Math.PI /
    180;

  return {

    x:
      centerX +
      radius *
      Math.cos(radians),

    y:
      centerY +
      radius *
      Math.sin(radians)

  };

}

/* =========================
   進化情報
========================= */

function getEvolutionLine(player){

  return evolutionData[
    player.selectedBasePokemon
  ] || null;

}

function getCurrentPokemonName(player){

  const line =
    getEvolutionLine(player);

  if(!line){

    return player.selectedBasePokemon;

  }

  return line[
    player.evolutionStage
  ].name;

}

function hasNextEvolution(player){

  const line =
    getEvolutionLine(player);

  return Boolean(
    line &&
    player.evolutionStage <
      line.length - 1
  );

}

/* =========================
   選択欄表示
========================= */

function restoreOptionNames(player){

  Array.from(
    player.select.options
  ).forEach(option => {

    if(
      evolutionData[
        option.value
      ]
    ){

      option.textContent =
        option.value;

    }

  });

}

function updateSelectedOption(player){

  const option =
    player.select.options[
      player.select.selectedIndex
    ];

  if(
    option &&
    player.select.value
  ){

    option.textContent =
      getCurrentPokemonName(
        player
      );

  }

}

/* =========================
   HP計算
========================= */

function getDegreesPerStep(player){

  return 360 /
    (
      player.maxHp /
      HP_STEP
    );

}

function hpToAngle(
  player,
  value
){

  return -90 +
    (
      value /
      player.maxHp
    ) *
    360;

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

/* =========================
   目盛り
========================= */

function createDialScale(player){

  player.scaleLines.innerHTML =
    "";

  player.scaleLabels.innerHTML =
    "";

  for(
    let value = 0;
    value < player.maxHp;
    value += HP_STEP
  ){

    const angle =
      -90 +
      (
        value /
        player.maxHp
      ) *
      360;

    const isMajor =
      value % 30 === 0;

    const inner =
      isMajor
        ? SCALE_INNER_RADIUS - 6
        : SCALE_INNER_RADIUS;

    const start =
      polarToCartesian(
        CENTER_X,
        CENTER_Y,
        inner,
        angle
      );

    const end =
      polarToCartesian(
        CENTER_X,
        CENTER_Y,
        SCALE_OUTER_RADIUS,
        angle
      );

    const line =
      document.createElementNS(
        "http://www.w3.org/2000/svg",
        "line"
      );

    line.setAttribute(
      "x1",
      start.x
    );

    line.setAttribute(
      "y1",
      start.y
    );

    line.setAttribute(
      "x2",
      end.x
    );

    line.setAttribute(
      "y2",
      end.y
    );

    line.setAttribute(
      "class",
      isMajor
        ? "scale-line major"
        : "scale-line"
    );

    player.scaleLines.appendChild(
      line
    );

    if(isMajor){

      const position =
        polarToCartesian(
          CENTER_X,
          CENTER_Y,
          LABEL_RADIUS,
          angle
        );

      const text =
        document.createElementNS(
          "http://www.w3.org/2000/svg",
          "text"
        );

      text.setAttribute(
        "x",
        position.x
      );

      text.setAttribute(
        "y",
        position.y + 4
      );

      text.setAttribute(
        "class",
        "scale-label"
      );

      text.textContent =
        value === 0
          ? "0/" + player.maxHp
          : value;

      player.scaleLabels.appendChild(
        text
      );

    }

  }

}

/* =========================
   進化ボタン
========================= */

function renderEvolutionButton(player){

  const show =
    hasNextEvolution(player) &&
    !player.isEvolving;

  player.evolutionButton
    .classList
    .toggle(
      "show",
      show
    );

  player.evolutionButton.disabled =
    !show;

}

/* =========================
   HP表示
========================= */

function renderHP(player){

  player.hp =
    Math.max(
      MIN_HP,
      Math.min(
        player.maxHp,
        player.hp
      )
    );

  const ratio =
    player.hp /
    player.maxHp;

  const color =
    getHpColor(
      player.hp
    );

  const dashOffset =
    CIRCUMFERENCE *
    (
      1 -
      ratio
    );

  const angle =
    hpToAngle(
      player,
      player.hp
    );

  const position =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS,
      angle
    );

  player.hpValue.textContent =
    String(
      player.hp
    ).padStart(
      3,
      "0"
    );

  player.hpPercent.textContent =
    Math.round(
      ratio *
      100
    ) +
    "%";

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
    "HP " +
    player.hp +
    "、最大HP " +
    player.maxHp
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

  const inner =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS - 4,
      angle
    );

  const outer =
    polarToCartesian(
      CENTER_X,
      CENTER_Y,
      HANDLE_RADIUS + 4,
      angle
    );

  player.handleLine.setAttribute(
    "x1",
    inner.x
  );

  player.handleLine.setAttribute(
    "y1",
    inner.y
  );

  player.handleLine.setAttribute(
    "x2",
    outer.x
  );

  player.handleLine.setAttribute(
    "y2",
    outer.y
  );

}

/* =========================
   全体表示
========================= */

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

/* =========================
   ポインター位置
========================= */

function getPointerPosition(
  player,
  event
){

  const rect =
    player.svg
      .getBoundingClientRect();

  return {

    x:
      (
        event.clientX -
        rect.left
      ) *
      (
        360 /
        rect.width
      ),

    y:
      (
        event.clientY -
        rect.top
      ) *
      (
        360 /
        rect.height
      )

  };

}

function getPointerAngle(
  player,
  event
){

  const point =
    getPointerPosition(
      player,
      event
    );

  return Math.atan2(
    point.y -
      CENTER_Y,

    point.x -
      CENTER_X
  ) *
  180 /
  Math.PI;

}

/* =========================
   タップでHP移動
========================= */

function jumpToTappedHp(
  player,
  event
){

  if(player.isEvolving){

    return;

  }

  const point =
    getPointerPosition(
      player,
      event
    );

  const dx =
    point.x -
    CENTER_X;

  const dy =
    point.y -
    CENTER_Y;

  const distance =
    Math.sqrt(
      dx * dx +
      dy * dy
    );

  if(
    distance <
      DIAL_RADIUS - 45 ||
    distance >
      DIAL_RADIUS + 45
  ){

    return;

  }

  const angle =
    Math.atan2(
      dy,
      dx
    ) *
    180 /
    Math.PI;

  let clockwiseAngle =
    angle +
    90;

  if(
    clockwiseAngle <
    0
  ){

    clockwiseAngle +=
      360;

  }

  if(
    clockwiseAngle >=
    360
  ){

    clockwiseAngle -=
      360;

  }

  const stepAngle =
    getDegreesPerStep(
      player
    );

  let step =
    Math.round(
      clockwiseAngle /
      stepAngle
    );

  const totalSteps =
    player.maxHp /
    HP_STEP;

  if(
    step >=
    totalSteps
  ){

    step =
      0;

  }

  let nextHp =
    step *
    HP_STEP;

  if(
    step === 0
  ){

    nextHp =
      player.hp >=
      player.maxHp / 2
        ? player.maxHp
        : 0;

  }

  player.hp =
    nextHp;

  player.accumulatedAngle =
    0;

  player.previousPointerAngle =
    null;

  renderHP(player);

}

/* =========================
   ドラッグ操作
========================= */

function updateHpFromDrag(
  player,
  event
){

  if(player.isEvolving){

    return;

  }

  const currentAngle =
    getPointerAngle(
      player,
      event
    );

  if(
    player.previousPointerAngle ===
    null
  ){

    player.previousPointerAngle =
      currentAngle;

    return;

  }

  let difference =
    currentAngle -
    player.previousPointerAngle;

  if(
    difference >
    180
  ){

    difference -=
      360;

  }

  if(
    difference <
    -180
  ){

    difference +=
      360;

  }

  player.accumulatedAngle +=
    difference;

  const stepAngle =
    getDegreesPerStep(
      player
    );

  while(
    player.accumulatedAngle >=
    stepAngle
  ){

    if(
      player.hp <
      player.maxHp
    ){

      player.hp =
        Math.min(
          player.maxHp,
          player.hp +
          HP_STEP
        );

      renderHP(player);

    }

    player.accumulatedAngle -=
      stepAngle;

  }

  while(
    player.accumulatedAngle <=
    -stepAngle
  ){

    if(
      player.hp >
      MIN_HP
    ){

      player.hp =
        Math.max(
          MIN_HP,
          player.hp -
          HP_STEP
        );

      renderHP(player);

    }

    player.accumulatedAngle +=
      stepAngle;

  }

  if(
    player.hp ===
      player.maxHp &&
    player.accumulatedAngle >
      0
  ){

    player.accumulatedAngle =
      0;

  }

  if(
    player.hp ===
      MIN_HP &&
    player.accumulatedAngle <
      0
  ){

    player.accumulatedAngle =
      0;

  }

  player.previousPointerAngle =
    currentAngle;

}

/* =========================
   操作終了
========================= */

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

    jumpToTappedHp(
      player,
      event
    );

  }

  player.isDragging =
    false;

  player.pointerMoved =
    false;

  player.previousPointerAngle =
    null;

  player.accumulatedAngle =
    0;

  player.dialContainer
    .classList
    .remove(
      "dragging"
    );

  try{

    if(
      player.dialContainer
        .hasPointerCapture(
          event.pointerId
        )
    ){

      player.dialContainer
        .releasePointerCapture(
          event.pointerId
        );

    }

  }catch(error){}

}

/* =========================
   プレイヤー設定
========================= */

function setupPlayer(player){

  renderAll(player);

  player.select.addEventListener(
    "change",
    () => {

      if(player.isEvolving){

        return;

      }

      restoreOptionNames(
        player
      );

      player.selectedBasePokemon =
        player.select.value;

      player.evolutionStage =
        0;

      player.maxHp =
        BASE_MAX_HP;

      player.hp =
        BASE_MAX_HP;

      player.previousPointerAngle =
        null;

      player.accumulatedAngle =
        0;

      renderAll(player);

    }
  );

  player.reset.addEventListener(
    "click",
    () => {

      resetPlayer(
        player
      );

    }
  );

  player.evolutionButton.addEventListener(
    "click",
    () => {

      evolvePlayer(
        player
      );

    }
  );

  player.dialContainer.addEventListener(
    "pointerdown",
    event => {

      if(player.isEvolving){

        return;

      }

      event.preventDefault();

      player.isDragging =
        true;

      player.pointerMoved =
        false;

      player.pointerStartX =
        event.clientX;

      player.pointerStartY =
        event.clientY;

      player.previousPointerAngle =
        getPointerAngle(
          player,
          event
        );

      player.accumulatedAngle =
        0;

      player.dialContainer
        .classList
        .add(
          "dragging"
        );

      try{

        player.dialContainer
          .setPointerCapture(
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
        Math.sqrt(
          dx * dx +
          dy * dy
        );

      if(
        distance >=
        TAP_MOVE_THRESHOLD
      ){

        player.pointerMoved =
          true;

      }

      if(
        player.pointerMoved
      ){

        updateHpFromDrag(
          player,
          event
        );

      }

    },
    {
      passive:false
    }
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
    {
      passive:false
    }
  );

  player.dialContainer.addEventListener(
    "keydown",
    event => {

      if(player.isEvolving){

        return;

      }

      if(
        event.key ===
          "ArrowLeft" ||
        event.key ===
          "ArrowDown"
      ){

        event.preventDefault();

        player.hp =
          Math.max(
            MIN_HP,
            player.hp -
            HP_STEP
          );

        renderHP(player);

      }

      if(
        event.key ===
          "ArrowRight" ||
        event.key ===
          "ArrowUp"
      ){

        event.preventDefault();

        player.hp =
          Math.min(
            player.maxHp,
            player.hp +
            HP_STEP
          );

        renderHP(player);

      }

    }
  );

}

/* =========================
   進化
========================= */

function evolvePlayer(player){

  if(
    player.isEvolving ||
    !hasNextEvolution(player)
  ){

    return;

  }

  const line =
    getEvolutionLine(
      player
    );

  const nextStage =
    player.evolutionStage +
    1;

  const next =
    line[
      nextStage
    ];

  const damage =
    player.maxHp -
    player.hp;

  player.isEvolving =
    true;

  player.evolutionButton.disabled =
    true;

  player.evolutionButton
    .classList
    .remove(
      "show"
    );

  player.evolutionText.textContent =
    "EVOLUTION";

  player.evolutionOverlay
    .classList
    .add(
      "active"
    );

  player.digitalUnit
    .classList
    .add(
      "evolving"
    );

  window.setTimeout(
    () => {

      player.evolutionStage =
        nextStage;

      player.maxHp =
        next.maxHp;

      player.hp =
        Math.max(
          MIN_HP,
          player.maxHp -
          damage
        );

      updateSelectedOption(
        player
      );

      player.monitorName.textContent =
        next.name;

      createDialScale(
        player
      );

      renderHP(
        player
      );

      player.evolutionText.textContent =
        next.name;

    },
    650
  );

  window.setTimeout(
    () => {

      player.evolutionOverlay
        .classList
        .remove(
          "active"
        );

      player.digitalUnit
        .classList
        .remove(
          "evolving"
        );

      player.isEvolving =
        false;

      renderEvolutionButton(
        player
      );

    },
    1550
  );

}

/* =========================
   個別RESET
========================= */

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

  if(
    option &&
    selected
  ){

    option.textContent =
      selected;

  }

  player.selectedBasePokemon =
    selected;

  player.evolutionStage =
    0;

  player.maxHp =
    BASE_MAX_HP;

  player.hp =
    BASE_MAX_HP;

  player.previousPointerAngle =
    null;

  player.accumulatedAngle =
    0;

  renderAll(player);

}

/* =========================
   ALL RESET
========================= */

document
  .getElementById(
    "allResetBtn"
  )
  .addEventListener(
    "click",
    () => {

      players.forEach(
        player => {

          resetPlayer(
            player
          );

        }
      );

    }
  );

/* =========================
   スクロール防止
========================= */

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
  {
    passive:false
  }
);

/* =========================
   初期化
========================= */

updateAppHeight();

players.forEach(
  setupPlayer
);

</script>

</body>
</html>
