<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>LET's PICK THE SPOOON</title>
  <style>
:root {
  --border:#dbeafe;
  --text:#082f6b;
  --muted:#64748b;
  --warn:#f59e0b;
  --ok:#22c55e;

  /* RKS x Samsung Blue */
  --blue-sky:#6EC6FF;
  --blue-samsung:#0B3C8A;
  --blue-deep:#082F6B;

  /* 포인트용 그라데이션 */
  --grad-blue: linear-gradient(135deg, #6EC6FF 0%, #0B3C8A 100%);
}
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
      width: 120px; padding: 8px 10px; border: 1px solid var(--border); border-radius: 10px;
      font-size: 14px;
    }
    input[type="checkbox"]{ transform: translateY(1px); }
    button{
      padding: 10px 14px; border: 1px solid var(--border); border-radius: 12px;
        background: var(--grad-blue); color:#ffffff; border: none; cursor:pointer; font-size:14px;
    }
    button.secondary{background:#ffffff; color: var(--blue-samsung); border:1px solid var(--blue-samsung); }
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
    .badge{
      font-size:12px; padding:4px 8px; border-radius:999px; border:1px solid var(--border); background:#fff;
      color: var(--muted);
    }
    .restBadge{ color: var(--warn); }
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
    <h1>RKS 팀 매치 메이커</h1>

    <div class="grid">
      <div class="card">
        <div class="split">
          <div>
            <label for="names" class="strong-label">참여자 이름(쉼표로 구분)</label>
            .strong-label { font-weight: 900; font-size: 15px; color: var(--text); }
            <textarea id="names" placeholder="예)&#10;강성준&#10;김늠름&#10;고원섭&#10;송준영"></textarea>
            <div class="hint">
              * 1경기 = 4명(복식 기준)으로 조 편성<br/>
            </div>
          </div>

          <div>
            <label for="prevRest">휴식자 입력</label>
            <textarea id="prevRest" class="small" placeholder="예)&#10;서한솔&#10;누리"></textarea>
            <div class="hint">* 휴식자는 이번 경기에는 ‘참여자’로 배정</div>
          </div>
        </div>

        <div class="row" style="margin-top:10px;">
          <div>
            <label for="courts">코트 수</label>
            <input id="courts" type="number" min="1" value="3" />
          </div>

          <div>
            <label for="seed">구분번호(필수X)</label>
            <input id="seed" class="mono" type="number" placeholder="예: 1234" />
          </div>

          <div style="display:flex; gap:8px; align-items:center; margin-top:18px;">
            <input id="enforceNoRepeatRest" type="checkbox" checked />
            <label for="enforceNoRepeatRest" style="margin:0; color:var(--text);">직전 휴식자는 이번 휴식자에서 제외(가능한 경우 강제)</label>
          </div>

          <button id="btnMake">팀 매칭</button>
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
      let n = index, s = "";
      while (true) {
        s = String.fromCharCode(A + (n % 26)) + s;
        n = Math.floor(n / 26) - 1;
        if (n < 0) break;
      }
      return s;
    }

    /**
     * 코트 수를 반영한 조 편성:
     * - groupSize=4 고정
     * - 이번 라운드 최대 참여 인원 = courts * 4
     * - 실제 참여 인원 = min(courts*4, floor(n/4)*4)  (항상 4의 배수로만 참여)
     * - 휴식자 = n - 실제 참여 인원  (5명 이상 가능)
     *
     * 연속 휴식 방지(enforce=true):
     * - 이번 휴식자(rest)에서 prevRest는 제외(가능한 경우 강제)
     * - 불가능한 경우(휴식해야 하는 인원이 너무 많아 prevRest 일부가 휴식할 수밖에 없음) 경고 표시
     */
    function makeRound(names, prevRest, courts, seedValue, enforce=true) {
      const groupSize = 4;
      const n = names.length;
      const maxPlayersByCourts = Math.max(0, courts) * groupSize;
      const maxPlayersByPeople = Math.floor(n / groupSize) * groupSize; // 4의 배수로만 참여
      const playCount = Math.min(maxPlayersByCourts, maxPlayersByPeople); // 이번 라운드 실제 참여 인원
      const restCount = n - playCount;

      const rand = (seedValue !== null && seedValue !== "") ? mulberry32(Number(seedValue)) : Math.random;

      const prevSet = new Set(prevRest);
      const eligibleRest = [];
      const ineligibleRest = [];
      for (const name of names) {
        (prevSet.has(name) ? ineligibleRest : eligibleRest).push(name);
      }

      // 섞어서 공정하게 뽑기
      shuffle(eligibleRest, rand);
      shuffle(ineligibleRest, rand);

      let warning = "";

      let rest = [];
      let players = [];

      if (!enforce || restCount <= 0 || prevRest.length === 0) {
        // 단순 랜덤: 전체 섞고 앞에서 playCount만 참여
        const all = [...names];
        shuffle(all, rand);
        players = all.slice(0, playCount);
        rest = all.slice(playCount);
      } else {
        // prevRest를 휴식에서 제외하려면 rest를 eligibleRest에서만 restCount만큼 뽑아야 함
        if (eligibleRest.length < restCount) {
          // 불가능: 휴식자가 너무 많아서 prevRest 일부는 휴식할 수밖에 없음
          warning = "⚠️ 이번 라운드 휴식 인원이 많아 ‘직전 휴식자 연속 휴식 금지’를 완전히 만족할 수 없습니다. (직전 휴식자 일부가 휴식에 포함될 수 있어요)";
          // 그래도 최대한 제외 시도: eligibleRest는 전부 휴식으로 보내고, 부족분은 ineligibleRest에서 채움
          rest = eligibleRest.slice(0);
          rest = rest.concat(ineligibleRest.slice(0, restCount - rest.length));
          // 남은 사람들로 참여자 구성
          const restSet = new Set(rest);
          players = names.filter(nm => !restSet.has(nm));
          shuffle(players, rand);
          players = players.slice(0, playCount);
        } else {
          // 가능: 휴식자는 eligibleRest에서만 선발
          rest = eligibleRest.slice(0, restCount);
          // 참여자는 (eligibleRest의 나머지 + ineligibleRest 전체)
          players = eligibleRest.slice(restCount).concat(ineligibleRest);
          shuffle(players, rand);
          players = players.slice(0, playCount);
        }
      }

      // 4명씩 그룹 만들기
      const groups = [];
      for (let i = 0; i < players.length; i += groupSize) {
        groups.push(players.slice(i, i + groupSize));
      }

      return { groups, rest, playCount, restCount, warning };
    }

    function render(groups, rest) {
      const out = document.getElementById("output");
      out.innerHTML = "";

      groups.forEach((members, idx) => {
        const box = document.createElement("div");
        box.className = "group";

        const h = document.createElement("h3");
        h.innerHTML = `<span>${groupLabel(idx)}조</span><span class="badge">${members.length}명</span>`;
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
      const lines = groups.map((members, idx) => `${groupLabel(idx)}조: ${members.join(", ")}`);
      if (rest.length > 0) lines.push(`휴식자: ${rest.join(", ")}`);
      return lines.join("\n");
    }

    // ---- UI wiring ----
    const namesEl = document.getElementById("names");
    const prevRestEl = document.getElementById("prevRest");
    const courtsEl = document.getElementById("courts");
    const seedEl = document.getElementById("seed");
    const enforceEl = document.getElementById("enforceNoRepeatRest");
    const summaryEl = document.getElementById("summary");
    const statusEl = document.getElementById("status");
    const toastEl = document.getElementById("toast");

    let latestText = "";

    document.getElementById("btnMake").addEventListener("click", () => {
      const names = parseNames(namesEl.value);
      const prevRestRaw = parseNames(prevRestEl.value);
      const courts = Math.max(1, Number(courtsEl.value || 1));
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

      // 직전 휴식자 중 참여자 목록에 없는 이름은 무시(경고만)
      const nameSet = new Set(names);
      const prevRest = prevRestRaw.filter(n => nameSet.has(n));
      const ignored = prevRestRaw.filter(n => !nameSet.has(n));

      const { groups, rest, playCount, restCount, warning } =
        makeRound(names, prevRest, courts, seedValue, enforce);

      render(groups, rest);
      latestText = toText(groups, rest);

      summaryEl.textContent =
        `총 ${names.length}명 / 코트 ${courts}개 → 참여 ${playCount}명(4인조 ${groups.length}개), 휴식 ${restCount}명`;

      if (ignored.length > 0) {
        statusEl.textContent = `참고: 직전 휴식자 중 참여자 목록에 없는 이름은 무시했어요 → ${ignored.join(", ")}`;
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

    document.getElementById("btnReset").addEventListener("click", () => {
      namesEl.value = "";
      prevRestEl.value = "";
      courtsEl.value = 3;
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
