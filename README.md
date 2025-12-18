<html lang="ko" translate="no">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <meta name="google" content="notranslate">
  <title>LET's PICK THE SPOOON</title>

  <style>
    :root{
      --blue-sky:#6EC6FF;
      --blue-samsung:#0B3C8A;

      --page-bg:#f3f4f6;
      --top-grad: linear-gradient(180deg, #f3f4f6 0%, #e5e7eb 100%);
      --grad-blue: linear-gradient(135deg, #6EC6FF 0%, #0B3C8A 100%);

      --border-soft: rgba(11,60,138,0.12);
      --text:#0b1220;
      --muted:#5b6475;

      --warn:#f59e0b;
      --ok:#22c55e;
      --danger:#ef4444;

      --card-bg: rgba(255,255,255,0.75);
      --card-bg-strong: rgba(255,255,255,0.88);
    }

    body{
      font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
      margin: 0;
      padding: 24px;
      color: var(--text);
      background: var(--page-bg);
    }

    body::before{
      content:"LET'S PLAY RKS";
      position: fixed;
      top: 0; left: 0;
      width: 100%; height: 330px;
      background: var(--top-grad);
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 56px;
      font-weight: 900;
      letter-spacing: 10px;
      font-style: italic;
      transform: rotate(-6deg);
      color: rgba(11, 80, 138, 0.12);
      text-transform: uppercase;
      pointer-events: none;
      user-select: none;
      z-index: 0;
    }

    .wrap{
      max-width: 1100px;
      margin: 0 auto;
      padding-top: 10px;
      position: relative;
      z-index: 1;
    }

    h1{
      margin: 0 0 14px;
      font-size: 26px;
      font-weight: 900;
      letter-spacing: 0.2px;
      color: var(--blue-samsung);
    }

    .grid{ display: grid; grid-template-columns: 1fr; gap: 14px; }
    @media (min-width: 980px){ .grid{ grid-template-columns: 1.15fr 0.85fr; } }

    .split{ display:grid; grid-template-columns: 1fr; gap:12px; }
    @media (min-width: 980px){ .split { grid-template-columns: 1fr 1fr; } }

    .row{ display:flex; gap:10px; flex-wrap:wrap; align-items:center; }

    .card{
      border: 1px solid rgba(255,255,255,0.55);
      border-radius: 18px;
      padding: 16px;
      background: var(--card-bg);
      box-shadow: 0 16px 40px rgba(0,0,0,0.12);
      backdrop-filter: blur(10px);
      -webkit-backdrop-filter: blur(10px);
    }

    textarea{
      width: 100%;
      min-height: 200px;
      resize: vertical;
      padding: 12px;
      border: 1px solid var(--border-soft);
      border-radius: 12px;
      font-size: 14px;
      line-height: 1.45;
      background: var(--card-bg-strong);
      color: var(--text);
      outline: none;
      box-sizing: border-box;
      transition: border-color .15s ease, box-shadow .15s ease;
    }
    textarea::placeholder{ color: rgba(11,18,32,0.45); }
    textarea:focus{
      border-color: rgba(11,60,138,0.35);
      box-shadow: 0 0 0 3px rgba(110,198,255,0.22);
    }
    .small{ min-height: 120px; }

    /* ✅ 중복 입력 시 textarea 빨간 강조 */
    textarea.dup{
      border-color: rgba(239,68,68,0.75);
      box-shadow: 0 0 0 3px rgba(239,68,68,0.18);
    }
    .dupWarn{
      color: rgba(220,38,38,0.95);
      font-weight: 800;
    }

    label{
      font-size: 13px;
      color: var(--muted);
      display:block;
      margin-bottom:6px;
    }

    .label-required{
      font-weight: 900;
      font-size: 15px;
      color: #0b1220;
    }
    .label-required::after{
      content:" (필수)";
      font-weight: 800;
      font-size: 12px;
      color: var(--blue-samsung);
      margin-left: 6px;
    }

    .label-strong{
      font-weight: 800;
      font-size: 14px;
      color: #0b1220;
    }

    input[type="number"]{
      width: 120px;
      padding: 8px 10px;
      border: 1px solid var(--border-soft);
      border-radius: 10px;
      font-size: 14px;
      background: var(--card-bg-strong);
      color: var(--text);
      outline: none;
      box-sizing: border-box;
    }
    input[type="number"]:focus{
      border-color: rgba(11,60,138,0.35);
      box-shadow: 0 0 0 3px rgba(110,198,255,0.22);
    }

    input[type="checkbox"]{ transform: translateY(1px); }

    button{
      padding: 10px 14px;
      border-radius: 12px;
      background: var(--grad-blue);
      color:#ffffff;
      border: none;
      cursor:pointer;
      font-size:14px;
      box-shadow: 0 10px 24px rgba(11,60,138,0.20);
    }
    button:hover{ filter: brightness(1.03); }
    button:active{ transform: translateY(1px); }

    button.secondary{
      background: rgba(255,255,255,0.9);
      color: var(--blue-samsung);
      border: 1px solid rgba(11,60,138,0.25);
      box-shadow: none;
    }
    button:disabled{
      opacity: .55;
      cursor: not-allowed;
      filter: grayscale(0.2);
    }

    .hint{ font-size: 13px; color: rgba(11,18,32,0.65); margin-top: 6px; }
    .status{ font-size: 13px; margin-top: 8px; }
    .status.warn{ color: var(--warn); }
    .status.ok{ color: var(--ok); }

    .out{
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 12px;
      margin-top: 12px;
    }
    .group{
      border: 1px solid rgba(11,60,138,0.12);
      border-radius: 16px;
      padding: 12px;
      background: rgba(255,255,255,0.86);
      box-shadow: 0 10px 26px rgba(0,0,0,0.06);
    }
    .group h3{
      margin: 0 0 8px;
      font-size: 15px;
      display:flex;
      justify-content:space-between;
      gap:10px;
      color: rgba(11,18,32,0.90);
    }
    .pill{
      display:inline-block;
      padding: 6px 10px;
      margin: 4px 6px 0 0;
      border-radius: 999px;
      border: 1px solid rgba(11,60,138,0.14);
      background: rgba(255,255,255,0.95);
      font-size: 13px;
      color: rgba(11,18,32,0.92);
    }
    .badge{
      font-size: 12px;
      padding: 4px 8px;
      border-radius: 999px;
      border: 1px solid rgba(11,60,138,0.14);
      background: rgba(255,255,255,0.95);
      color: rgba(11,18,32,0.68);
    }
    .restBadge{ color: var(--warn); }

    .footer{ margin-top: 10px; font-size: 12px; color: rgba(11,18,32,0.62); }
    .copyline{ display:flex; gap:10px; align-items:center; flex-wrap:wrap; }
    .mono{ font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace; }
    .toast{ color: var(--ok); font-size: 13px; display:none; }

    .controls{
      display:flex;
      flex-direction: column;
      gap: 10px;
      margin-top: 12px;
    }
    .checkline{
      display:flex;
      align-items:center;
      gap:10px;
    }
    .checkline label{
      margin: 0;
      display: inline;
      color: rgba(11,18,32,0.85);
      font-weight: 700;
    }

    .btnbar{
      display:flex;
      gap: 10px;
      margin-top: 14px;
      flex-wrap: nowrap;
    }
    .btnbar button{
      flex: 1;
      white-space: nowrap;
      min-width: 0;
    }
    @media (max-width: 360px){
      .btnbar button{ font-size: 13px; padding: 10px 12px; }
    }

    /* gap fallback */
    .checkline > * + * { margin-left: 10px; }
    .btnbar > * + * { margin-left: 10px; }
    .copyline > * + * { margin-left: 10px; }
  </style>
</head>

<body>
  <div class="wrap">
    <h1>RKS 팀 매치 메이커</h1>

    <div class="grid">
      <div class="card">
        <div class="split">
          <div>
            <label for="names" class="label-required">참여자 이름(쉼표로 구분)</label>
            <textarea id="names" placeholder="예)성준, 늠름, 원섭, 준영"></textarea>

            <div class="hint" id="nameCount">현재 0명 입력됨 (총 입력 0)</div>
            <div class="hint" id="dupHint"></div>

            <div class="hint">* 코트 기준으로 조가 생성됩니다. (복식=4명, 단식=2명)</div>
          </div>

          <div>
            <label for="prevRest" class="label-strong">휴식자 입력</label>
            <textarea id="prevRest" class="small" placeholder="예)한솔, 누리"></textarea>
            <div class="hint">* 휴식자는 이번 경기에는 ‘참여자’로 배정</div>
          </div>
        </div>

        <div class="row" style="margin-top:10px;">
          <div>
            <label for="courts" class="label-required">코트 수</label>
            <input id="courts" type="number" min="1" value="3" />
          </div>

          <div>
            <label for="seed" class="label-strong">구분번호</label>
            <input id="seed" class="mono" type="number" placeholder="예: 1234" />
          </div>
        </div>

        <div class="controls">
          <div class="checkline">
            <input id="enforceNoRepeatRest" type="checkbox" checked />
            <label for="enforceNoRepeatRest">휴식자 경기에 배정하기</label>
          </div>

          <div class="checkline">
            <input id="allowSingles" type="checkbox" />
            <label for="allowSingles">단식경기가 있다면 체크</label>
          </div>
        </div>

        <div class="btnbar">
          <button id="btnMake" type="button">팀 매칭</button>
          <button id="btnReset" type="button" class="secondary">초기화</button>
        </div>

        <div id="status" class="status"></div>
      </div>

      <div class="card" id="resultCard">
        <div class="row" style="justify-content:space-between;">
          <div>
            <div style="font-weight:800; margin-bottom:4px; color: rgba(11,18,32,0.90);">결과</div>
            <div class="hint" id="summary">아직 조 편성을 하지 않았어요.</div>
          </div>
          <div class="copyline">
            <button id="btnCopy" type="button" class="secondary">결과 복사</button>
            <span id="toast" class="toast">복사 완료!</span>
          </div>
        </div>

        <div id="output" class="out"></div>
        <div class="footer">A, B, C... 조는 자동 생성됩니다. ‘휴식자’는 별도 박스로 표시됩니다.</div>
      </div>
    </div>
  </div>

  <script>
    const $ = (id) => document.getElementById(id);

    function mulberry32(seed) {
      let t = seed >>> 0;
      return function() {
        t += 0x6D2B79F5;
        let r = Math.imul(t ^ (t >>> 15), 1 | t);
        r ^= r + Math.imul(r ^ (t >>> 7), 61 | r);
        return ((r ^ (t >>> 14)) >>> 0) / 4294967296;
      };
    }

    function parseNames(raw) {
      return raw
        .split(/[\n,;\t]+/g)
        .map(s => s.trim())
        .filter(Boolean);
    }

    function shuffle(arr, rand=Math.random) {
      for (let i = arr.length - 1; i > 0; i--) {
        const j = Math.floor(rand() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
      return arr;
    }

    function groupLabel(index) {
      const A = "A".charCodeAt(0);
      let n = index, s = "";
      while (true) {
        s = String.fromCharCode(A + (n % 26)) + s;
        n = Math.floor(n / 26) - 1;
        if (n < 0) break;
      }
      return s;
    }

    function makeRound(names, prevRest, courts, seedValue, enforce=true, allowSingles=false) {
      const n = names.length;
      const rand = (seedValue !== null && seedValue !== "") ? mulberry32(Number(seedValue)) : Math.random;

      let playCount;

      if (!allowSingles) {
        const maxPlayersByCourts = courts * 4;
        const maxPlayersByPeople = Math.floor(n / 4) * 4;
        playCount = Math.min(maxPlayersByCourts, maxPlayersByPeople);
      } else {
        const maxPlayers = courts * 4;
        const minPlayers = courts * 2;

        const target = Math.min(n, maxPlayers);
        const evenTarget = target - (target % 2);

        playCount = Math.max(minPlayers, evenTarget);
        playCount = Math.min(playCount, maxPlayers, n);
        playCount = playCount - (playCount % 2);
      }

      const restCount = n - playCount;

      const prevSet = new Set(prevRest);
      const eligibleRest = [];
      const ineligibleRest = [];

      for (const name of names) (prevSet.has(name) ? ineligibleRest : eligibleRest).push(name);

      shuffle(eligibleRest, rand);
      shuffle(ineligibleRest, rand);

      let warning = "";
      let rest = [];
      let players = [];

      if (!enforce || restCount <= 0 || prevRest.length === 0) {
        const all = [...names];
        shuffle(all, rand);
        players = all.slice(0, playCount);
        rest = all.slice(playCount);
      } else {
        if (eligibleRest.length < restCount) {
          warning = "⚠️ 이번 라운드 휴식 인원이 많아 ‘직전 휴식자 연속 휴식 금지’를 완전히 만족할 수 없습니다.";
          rest = eligibleRest.slice(0);
          rest = rest.concat(ineligibleRest.slice(0, restCount - rest.length));

          const restSet = new Set(rest);
          players = names.filter(nm => !restSet.has(nm));
          shuffle(players, rand);
          players = players.slice(0, playCount);
        } else {
          rest = eligibleRest.slice(0, restCount);
          players = eligibleRest.slice(restCount).concat(ineligibleRest);
          shuffle(players, rand);
          players = players.slice(0, playCount);
        }
      }

      const groups = [];

      if (!allowSingles) {
        for (let i = 0; i < players.length; i += 4) groups.push(players.slice(i, i + 4));
      } else {
        const fours = players.length - (players.length % 4);
        for (let i = 0; i < fours; i += 4) groups.push(players.slice(i, i + 4));
        const remaining = players.slice(fours);
        if (remaining.length === 2) groups.push(remaining);
      }

      const doublesCourts = groups.filter(g => g.length === 4).length;
      const singlesCourts = groups.filter(g => g.length === 2).length;

      return { groups, rest, playCount, restCount, warning, doublesCourts, singlesCourts };
    }

    function render(groups, rest) {
      const out = $("output");
      out.innerHTML = "";

      groups.forEach((members, idx) => {
        const box = document.createElement("div");
        box.className = "group";

        const type = members.length === 2 ? "단식" : "복식";
        const h = document.createElement("h3");
        h.innerHTML = `<span>${groupLabel(idx)}조 (${type})</span><span class="badge">${members.length}명</span>`;
        box.appendChild(h);

        members.forEach(name => {
          const p = document.createElement("span");
          p.className = "pill";
          p.textContent = name;
          box.appendChild(p);
        });

        out.appendChild(box);
      });

      if (rest.length > 0) {
        const box = document.createElement("div");
        box.className = "group";

        const h = document.createElement("h3");
        h.innerHTML = `<span>휴식자</span><span class="badge restBadge">${rest.length}명</span>`;
        box.appendChild(h);

        rest.forEach(name => {
          const p = document.createElement("span");
          p.className = "pill";
          p.textContent = name;
          box.appendChild(p);
        });

        out.appendChild(box);
      }
    }

    function toText(groups, rest) {
      const lines = groups.map((members, idx) => {
        const type = members.length === 2 ? "단식" : "복식";
        return `${groupLabel(idx)}조(${type}): ${members.join(", ")}`;
      });
      if (rest.length > 0) lines.push(`휴식자: ${rest.join(", ")}`);
      return lines.join("\n");
    }

    function legacyCopy(text){
      const ta = document.createElement("textarea");
      ta.value = text;
      ta.setAttribute("readonly", "");
      ta.style.position = "fixed";
      ta.style.top = "-9999px";
      ta.style.left = "-9999px";
      document.body.appendChild(ta);

      ta.select();
      ta.setSelectionRange(0, ta.value.length);
      let ok = false;
      try { ok = document.execCommand("copy"); } catch (e) {}
      document.body.removeChild(ta);
      return ok;
    }

    const namesEl = $("names");
    const prevRestEl = $("prevRest");
    const courtsEl = $("courts");
    const seedEl = $("seed");
    const enforceEl = $("enforceNoRepeatRest");
    const allowSinglesEl = $("allowSingles");
    const summaryEl = $("summary");
    const statusEl = $("status");
    const toastEl = $("toast");
    const resultCardEl = $("resultCard");

    // ✅ 실시간 카운터 + 중복 표시
    const nameCountEl = $("nameCount");
    const dupHintEl = $("dupHint");
    const btnMakeEl = $("btnMake");

    function getNameStats(raw){
      const all = parseNames(raw);
      const map = new Map();
      for (const nm of all) map.set(nm, (map.get(nm) || 0) + 1);

      const duplicates = [];
      for (const [nm, cnt] of map.entries()) {
        if (cnt >= 2) duplicates.push(nm); // ✅ 완전 동일 문자열만 중복
      }

      return { all, uniqCount: map.size, duplicates };
    }

    function updateNameUI(){
      const { all, uniqCount, duplicates } = getNameStats(namesEl.value);

      nameCountEl.textContent = `현재 ${uniqCount}명 입력됨 (총 입력 ${all.length})`;

      if (duplicates.length > 0) {
        namesEl.classList.add("dup");
        dupHintEl.classList.add("dupWarn");
        dupHintEl.textContent = `중복 입력: ${duplicates.join(", ")}`;

        // ✅ 중복이면 팀 매칭 막기(요청했던 "보완" 포함)
        btnMakeEl.disabled = true;
        statusEl.textContent = "⚠️ 중복된 이름이 있어요. 중복을 제거하면 팀 매칭이 가능합니다.";
        statusEl.className = "status warn";
      } else {
        namesEl.classList.remove("dup");
        dupHintEl.classList.remove("dupWarn");
        dupHintEl.textContent = "";

        btnMakeEl.disabled = false;
        // status는 여기서 건드리지 않음(팀매칭 결과 메시지 유지)
      }
    }

    namesEl.addEventListener("input", updateNameUI);
    updateNameUI();

    let latestText = "";

    $("btnMake").addEventListener("click", () => {
      // ✅ 클릭 시에도 한번 더 확인(안전)
      const { duplicates } = getNameStats(namesEl.value);
      if (duplicates.length > 0) return;

      const names = parseNames(namesEl.value);
      const prevRestRaw = parseNames(prevRestEl.value);
      const courts = Math.max(1, Number(courtsEl.value || 1));
      const seedValue = seedEl.value;
      const enforce = enforceEl.checked;
      const allowSingles = allowSinglesEl.checked;

      statusEl.textContent = "";
      statusEl.className = "status";

      if (names.length === 0) {
        summaryEl.textContent = "이름을 먼저 입력해줘.";
        $("output").innerHTML = "";
        latestText = "";
        return;
      }

      const nameSet = new Set(names);
      const prevRest = prevRestRaw.filter(n => nameSet.has(n));
      const ignored = prevRestRaw.filter(n => !nameSet.has(n));

      const { groups, rest, playCount, restCount, warning, doublesCourts, singlesCourts } =
        makeRound(names, prevRest, courts, seedValue, enforce, allowSingles);

      render(groups, rest);
      latestText = toText(groups, rest);

      requestAnimationFrame(() => {
        if (resultCardEl && typeof resultCardEl.scrollIntoView === "function") {
          resultCardEl.scrollIntoView({ behavior: "smooth", block: "start" });
        } else {
          window.scrollTo({ top: document.body.scrollHeight, behavior: "smooth" });
        }
      });

      const courtInfo = allowSingles
        ? ` / 복식 ${doublesCourts}코트 + 단식 ${singlesCourts}코트`
        : "";

      summaryEl.textContent =
        `총 ${names.length}명 / 코트 ${courts}개${courtInfo} → 참여 ${playCount}명, 휴식 ${restCount}명`;

      if (ignored.length > 0) {
        statusEl.textContent = `참고: 휴식자 입력 중 참여자 목록에 없는 이름은 무시했어요 → ${ignored.join(", ")}`;
        statusEl.classList.add("warn");
      }

      if (warning) {
        statusEl.textContent = warning;
        statusEl.classList.add("warn");
      } else if (enforce && prevRest.length > 0 && restCount > 0) {
        statusEl.textContent = "✅ 직전 휴식자 연속 휴식 금지 룰을 적용했어요.";
        statusEl.classList.add("ok");
      }
    });

    $("btnReset").addEventListener("click", () => {
      namesEl.value = "";
      prevRestEl.value = "";
      courtsEl.value = 3;
      seedEl.value = "";
      enforceEl.checked = true;
      allowSinglesEl.checked = false;

      $("output").innerHTML = "";
      summaryEl.textContent = "아직 조 편성을 하지 않았어요.";
      statusEl.textContent = "";
      statusEl.className = "status";
      latestText = "";
      toastEl.style.display = "none";

      updateNameUI();
    });

    $("btnCopy").addEventListener("click", async () => {
      if (!latestText) return;

      try {
        if (navigator.clipboard && window.isSecureContext) {
          await navigator.clipboard.writeText(latestText);
          toastEl.style.display = "inline";
          setTimeout(() => (toastEl.style.display = "none"), 1200);
          return;
        }
      } catch (e) {}

      const ok = legacyCopy(latestText);
      if (ok) {
        toastEl.style.display = "inline";
        setTimeout(() => (toastEl.style.display = "none"), 1200);
        return;
      }

      window.prompt("복사해서 사용하세요:", latestText);
    });
  </script>
</body>
</html>
```
