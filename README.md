<!doctype html>
<html lang="zh-Hant">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>繁中自動朗讀</title>
  <style>
    body{
      font-family: system-ui, -apple-system, "PingFang TC", "Noto Sans TC", sans-serif;
      max-width: 920px;
      margin: 32px auto;
      padding: 0 16px;
      line-height: 1.6;
    }
    h1{font-size: 28px; margin: 0 0 12px}
    textarea{
      width: 100%;
      min-height: 240px;
      padding: 14px;
      border: 1px solid #ccc;
      border-radius: 14px;
      font-size: 16px;
      box-sizing: border-box;
    }
    .row{
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      align-items: center;
      margin: 12px 0;
    }
    button, input{
      padding: 10px 12px;
      border-radius: 14px;
      border: 1px solid #ccc;
      font-size: 14px;
      cursor: pointer;
      background: white;
    }
    label{font-size: 14px; color:#333}
    .hint{color:#666;font-size:13px;margin:8px 0 0}
  </style>
</head>

<body>
  <h1>繁體中文文字朗讀</h1>
  <p class="hint">把文字貼上後按「開始朗讀」。如果第一次沒聲音，先按一次「停止」再按「開始朗讀」。</p>

  <textarea id="text" placeholder="把你要朗讀的文字貼在這裡…"></textarea>

  <div class="row">
    <button id="speak">開始朗讀</button>
    <button id="pause">暫停</button>
    <button id="resume">繼續</button>
    <button id="stop">停止</button>

    <label>語速
      <input id="rate" type="range" min="0.5" max="2" step="0.1" value="1">
    </label>
    <label>音量
      <input id="volume" type="range" min="0" max="1" step="0.1" value="1">
    </label>
  </div>

  <script>
    const textEl = document.getElementById('text');
    const rateEl = document.getElementById('rate');
    const volumeEl = document.getElementById('volume');

    let utter = null;

    function buildUtterance(text){
      const u = new SpeechSynthesisUtterance(text);
      // 目標：繁體中文 / 台灣
      u.lang = "zh-TW";
      u.rate = Number(rateEl.value);
      u.volume = Number(volumeEl.value);
      return u;
    }

    document.getElementById('speak').addEventListener('click', () => {
      const text = textEl.value.trim();
      if (!text) return;

      // 先停止任何正在朗讀的內容，避免疊音/卡住
      window.speechSynthesis.cancel();

      utter = buildUtterance(text);
      window.speechSynthesis.speak(utter);
    });

    document.getElementById('pause').addEventListener('click', () => {
      window.speechSynthesis.pause();
    });

    document.getElementById('resume').addEventListener('click', () => {
      window.speechSynthesis.resume();
    });

    document.getElementById('stop').addEventListener('click', () => {
      window.speechSynthesis.cancel();
    });
  </script>
</body>
</html>
