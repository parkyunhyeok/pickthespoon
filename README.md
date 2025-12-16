<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>LET's PICK THE SPOON</title>
  <style>
    :root { --border:#e5e7eb; --text:#111827; --muted:#6b7280; --warn:#b45309; --ok:#065f46; }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif; margin: 24px; color: var(--text); }
    .wrap { max-width: 1100px; margin: 0 auto; }
    h1 { font-size: 22px; margin: 0 0 12px; }
    .grid { display: grid; grid-template-columns: 1fr; gap: 14px; }
    @media (min-width: 980px){ .grid { grid-template-columns: 1.15fr 0.85fr; } }

    textarea {
      width: 100%; min-height: 200px; resize: vertical; padding: 12px;
      border: 1px solid var(--border); border-radius: 12px; font-size: 14px;
      line-height: 1.45;
    }
    .small { min-height: 120px; }

    .row { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
    .card { border: 1px solid var(--border); border-radius: 16px; padding: 14px; background: #fff; }
    label { font-size: 13px; color: var(--muted); display:block; margin-bottom:6px; }
    input[type="number"]{
      width: 90px; padding: 8px 10px; border: 1px solid var(--border); border-radius: 10px;
      font-size: 14px;
    }
    input[type="checkbox"]{ transform: translateY(1px); }
    button{
      padding: 10px 14px; border: 1px solid var(--border); border-radius: 12px;
      background:#111827; color:#fff; cursor:pointer; font-size:14px;
    }
    button.secondary{ background:#fff; color:#111827; }
    button:active{ transform: translateY(1px); }
    .hint { font-size: 13px; color: var(--muted); margin-top: 6px; }
    .status { font-size: 13px; margin-top: 8px; }
    .status.warn { color: var(--warn); }
    .status.ok { color: var(--ok); }

    .out { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 12px; margin-top:12px; }
    .group { border: 1px solid var(--border); border-radius: 16px; padding: 12px; background: #fafafa; }
    .group h3{ margin: 0 0 8px; font-size: 15px; display:flex; justify-content:space-between; gap:10px; }
    .pill {
      display: inline-block; padding: 6px 10px; margin: 4px 6px 0 0;
      border-radius: 999px; border: 1px solid var(--border); background:#fff; font-size: 13px;
    }
    .restBadge{
      font-size:12px; padding:4px 8px; border-radius:999px; border:1px solid var(--border); background:#fff;
      color: var(--warn);
    }
    .footer { margin-top: 10px; font-size: 12px; color: var(--muted); }
    .copyline { display:flex; gap:10px; align-items:center; flex-wrap:wrap; }
    .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace; }
    .toast { color: var(--ok); font-size: 13px; display:none; }
    .split { display:grid; grid-template-columns: 1fr; gap:12px; }
    @media (min-width: 980px){ .split { grid-template-columns: 1fr 1fr; } }
  </style>
</head>
<body>
  <div class="wrap">
    <h1>랜덤 조 편성기 (4인 1조 + 연속 휴식 방지 옵션)</h1>

    <div class="grid">
      <div class="card">
        <div class="split">
          <div>
            <label for="names">참여자 이름 (한 줄에 1명 / 쉼표·세미콜론·탭도 가능)</label>
            <textarea id="names" placeholder="예)&#10;김철수&#10;이영희&#10;박민수&#10;최지은"></textarea>
            <div class="hint">* 인원이 4로 나누어떨어지지 않으면 남는 1~3명은 ‘이번 경기 휴식자’가 됩니다.</div>
          </div>

          <div>
            <label for="prevRest">직전 경기 휴식자 (이번 경기엔 휴식 불가)</label>
            <textarea id="prevRest" class="small" placeholder="예)&#10;이영희&#10;최지은"></textarea>
            <div class="hint">* 여기에 입력된 사람은 이번 경기 결과에서 ‘휴식자’로 뽑히지 않도록 강제합니다.</div>
          </div>
        </div>

        <div class="row" style="margin-top:10px;">
          <div>
            <label for="size">조당 인원</label>
            <input id="size" type="number" min="1" value="4" disabled />
            <div class="hint">현재 옵션은 4인 고정(룰 기반).</div>
          </div>

          <div>
            <label for="seed">시드(선택)</label>
            <input id="seed" class="mono" type="number" placeholder="예: 1234" />
          </div>

          <div style="display:flex; gap:8px; align-items:center; margin-top:18px;">
            <input id="enforceNoRepeatRest" type="checkbox" checked />
            <label for="enforceNoRepeatRest" style="margin:0; color:var(--text);">직전 휴식자는 이번 경기 ‘휴식자’ 금지(강제)</label>
          </div>

          <button id="btnMake">조 편성</button>
          <button id="btnReset" class="secondary">초기화</button>
        </div>

        <div id="status" class="status"></div>
      </div>

      <div class="card">
        <div class="row" style="justify-content:space-between;">
          <div>
            <div style="font-weight:600; margin-bottom:4px;">결과</div>
            <div class="hint" id="summary">아직 조 편성을 하지 않았어요.</div>
          </div>
          <div class="copyline">
            <button id="btnCopy" class="secondary">결과 복사</button>
            <span id="toast" class="toast">복사 완료!</span>
          </div>
        </div>

        <div id="output" class="out"></div>
        <div class="footer">A, B, C... 조는 자동 생성됩니다. ‘휴식자’는 별도 박스로 표시됩니다.</div>
      </div>
    </div>
  </div>

  <script>
    // ---- Seeded RNG (mulberry32) ----
    function mulberry32(seed) {
      let t = seed >>> 0;
      return function() {
        t += 0x6D2B79F5;
        let r = Math.imul(t ^ (t >>> 15), 1 | t);
        r ^= r + Math.imul(r ^ (r >>> 7), 61 | r);
        return ((r ^ (r >>> 14)) >>> 0) / 4294967296;
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
      let n = index;
      let s = "";
      while (true) {
        s = String.fromCharCode(A + (n % 26)) + s;
        n = Math.floor(n / 26) - 1;
        if (n < 0) break;
      }
      return s;
    }

    /**
     * 핵심 로직:
     * - 4명씩 그룹 만들고 나머지는 rest(휴식자)
     * - enforce=true일 때 prevRest(직전 휴식자)는 rest에 들어가지 않도록 swap으로 보정
     * - 보정 불가능(휴식해야 하는 인원 수가 prevRest보다 작거나, rest 후보가 부족)하면 경고
     */
    function makeGroupsWithNoRepeatRest(names, prevRest, seedValue, enforce=true) {
      const groupSize = 4;
      const list = [...names];

      const rand = (seedValue !== null && seedValue !== "") ? mulberry32(Number(seedValue)) : Math.random;
      shuffle(list, rand);

      // 1) 일단 4명씩 잘라 그룹 + rest(남는 인원)
      const fullCount = Math.floor(list.length / groupSize) * groupSize;
      const players = list.slice(0, fullCount);
      const rest = list.slice(fullCount); // 0~3명

      const groups = [];
      for (let i = 0; i < players.length; i += groupSize) {
        groups.push(players.slice(i, i + groupSize));
      }

      if (!enforce || rest.length === 0 || prevRest.length === 0) {
        return { groups, rest, warning: "" };
      }

      // 2) prevRest가 rest 안에 들어갔는지 확인하고, 있으면 swap으로 빼내기
      const prevSet = new Set(prevRest);
      const restSet = new Set(rest);

      const conflicted = rest.filter(n => prevSet.has(n)); // 이번 rest에 포함된 '직전 휴식자'
      if (conflicted.length === 0) {
        return { groups, rest, warning: "" };
      }

      // rest로 갈 수 있는 사람(=prevRest가 아닌 사람)이 충분한지 체크
      // 이번 rest 인원 수 = r
      // rest에 들어갈 수 있는 후보 수 = 전체 - prevRestCount
      const r = rest.length;
      const canRestCount = names.length - prevRest.length;
      if (canRestCount < r) {
        return {
          groups,
          rest,
          warning: "⚠️ 직전 휴식자가 너무 많아 이번 경기에서 ‘연속 휴식 금지’를 만족시키며 휴식자를 뽑을 수 없습니다. (이번 휴식 인원 수 > 휴식 가능 후보 수)"
        };
      }

      // swap 후보: 그룹 안에 있으면서 prevRest가 아닌 사람
      const swapCandidates = [];
      groups.forEach((g, gi) => g.forEach((member, mi) => {
        if (!prevSet.has(member)) swapCandidates.push({ gi, mi, member });
      }));

      // rest에 들어갈 사람도 prevRest가 아니어야 하므로, conflicted 수만큼 swap 가능해야 함
      if (swapCandidates.length < conflicted.length) {
        return {
          groups,
          rest,
          warning: "⚠️ 스왑 후보가 부족해 ‘직전 휴식자 연속 휴식 금지’를 만족시키지 못했습니다."
        };
      }

      // 실제 swap 실행: conflicted(휴식에 들어간 직전 휴식자)를 그룹으로 밀어넣고,
      // 그룹의 non-prevRest 한 명을 rest로 빼기
      for (let k = 0; k < conflicted.length; k++) {
        const bad = conflicted[k];           // rest에 있으면 안 되는 사람
        const cand = swapCandidates[k];      // 그룹에서 rest로 가도 되는 사람

        // 그룹 내 교체
        groups[cand.gi][cand.mi] = bad;

        // rest 내 교체
        const idx = rest.indexOf(bad);
        if (idx !== -1) rest[idx] = cand.member;
      }

      return { groups, rest, warning: "" };
    }

    function render(groups, rest) {
      const out = document.getElementById("output");
      out.innerHTML = "";

      groups.forEach((members, idx) => {
        const box = document.createElement("div");
        box.className = "group";

        const h = document.createElement("h3");
        h.innerHTML = `<span>${groupLabel(idx)}조</span><span class="restBadge">${members.length}명</span>`;
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
        h.innerHTML = `<span>휴식자</span><span class="restBadge">${rest.length}명</span>`;
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
      const lines = groups.map((members, idx) => `${groupLabel(idx)}조: ${members.join(", ")}`);
      if (rest.length > 0) lines.push(`휴식자: ${rest.join(", ")}`);
      return lines.join("\n");
    }

    // ---- UI wiring ----
    const namesEl = document.getElementById("names");
    const prevRestEl = document.getElementById("prevRest");
    const seedEl = document.getElementById("seed");
    const enforceEl = document.getElementById("enforceNoRepeatRest");
    const summaryEl = document.getElementById("summary");
    const statusEl = document.getElementById("status");
    const toastEl = document.getElementById("toast");

    let latestText = "";

    document.getElementById("btnMake").addEventListener("click", () => {
      const names = parseNames(namesEl.value);
      const prevRest = parseNames(prevRestEl.value);
      const seedValue = seedEl.value;
      const enforce = enforceEl.checked;

      statusEl.textContent = "";
      statusEl.className = "status";

      if (names.length === 0) {
        summaryEl.textContent = "이름을 먼저 입력해줘.";
        document.getElementById("output").innerHTML = "";
        latestText = "";
        return;
      }

      // 직전 휴식자 명단 중 참여자 목록에 없는 이름은 무시(경고만)
      const nameSet = new Set(names);
      const filteredPrevRest = prevRest.filter(n => nameSet.has(n));
      const ignored = prevRest.filter(n => !nameSet.has(n));

      const { groups, rest, warning } = makeGroupsWithNoRepeatRest(names, filteredPrevRest, seedValue, enforce);
      render(groups, rest);

      latestText = toText(groups, rest);

      const r = names.length % 4;
      const restCount = r === 0 ? 0 : r;
      summaryEl.textContent = `총 ${names.length}명 → 4인조 ${Math.floor(names.length / 4)}개 + 휴식자 ${restCount}명`;

      if (ignored.length > 0) {
        statusEl.textContent = `참고: 직전 휴식자 중 참여자 목록에 없는 이름은 무시했어요 → ${ignored.join(", ")}`;
        statusEl.classList.add("warn");
      }

      if (warning) {
        statusEl.textContent = warning;
        statusEl.classList.add("warn");
      } else if (enforce && filteredPrevRest.length > 0) {
        statusEl.textContent = "✅ 직전 휴식자 연속 휴식 금지 룰을 적용했어요.";
        statusEl.classList.add("ok");
      }
    });

    document.getElementById("btnReset").addEventListener("click", () => {
      namesEl.value = "";
      prevRestEl.value = "";
      seedEl.value = "";
      enforceEl.checked = true;
      document.getElementById("output").innerHTML = "";
      summaryEl.textContent = "아직 조 편성을 하지 않았어요.";
      statusEl.textContent = "";
      statusEl.className = "status";
      latestText = "";
      toastEl.style.display = "none";
    });

    document.getElementById("btnCopy").addEventListener("click", async () => {
      if (!latestText) return;
      try {
        await navigator.clipboard.writeText(latestText);
        toastEl.style.display = "inline";
        setTimeout(() => (toastEl.style.display = "none"), 1200);
      } catch (e) {
        window.prompt("복사해서 사용하세요:", latestText);
      }
    });
  </script>
</body>
</html>
