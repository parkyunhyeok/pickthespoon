# pickthespoon
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>랜덤 조 편성기</title>
  <style>
    :root { --border:#e5e7eb; --text:#111827; --muted:#6b7280; }
    body { font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif; margin: 24px; color: var(--text); }
    .wrap { max-width: 980px; margin: 0 auto; }
    h1 { font-size: 22px; margin: 0 0 12px; }
    .grid { display: grid; grid-template-columns: 1fr; gap: 14px; }
    @media (min-width: 900px){ .grid { grid-template-columns: 1fr 1fr; } }
    textarea {
      width: 100%; min-height: 240px; resize: vertical; padding: 12px;
      border: 1px solid var(--border); border-radius: 12px; font-size: 14px;
      line-height: 1.45;
    }
    .row { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }
    .card {
      border: 1px solid var(--border); border-radius: 16px; padding: 14px; background: #fff;
    }
    label { font-size: 13px; color: var(--muted); }
    input[type="number"]{
      width: 90px; padding: 8px 10px; border: 1px solid var(--border); border-radius: 10px;
      font-size: 14px;
    }
    button{
      padding: 10px 14px; border: 1px solid var(--border); border-radius: 12px;
      background:#111827; color:#fff; cursor:pointer; font-size:14px;
    }
    button.secondary{ background:#fff; color:#111827; }
    button:active{ transform: translateY(1px); }
    .hint { font-size: 13px; color: var(--muted); margin-top: 6px; }
    .out { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 12px; }
    .group {
      border: 1px solid var(--border); border-radius: 16px; padding: 12px;
      background: #fafafa;
    }
    .group h3{ margin: 0 0 8px; font-size: 15px; }
    .pill {
      display: inline-block; padding: 6px 10px; margin: 4px 6px 0 0;
      border-radius: 999px; border: 1px solid var(--border); background:#fff; font-size: 13px;
    }
    .footer { margin-top: 12px; font-size: 12px; color: var(--muted); }
    .copyline { display:flex; gap:10px; align-items:center; flex-wrap:wrap; }
    .mono { font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", monospace; }
    .toast { color: #065f46; font-size: 13px; display:none; }
  </style>
</head>
<body>
  <div class="wrap">
    <h1>랜덤 조 편성기</h1>

    <div class="grid">
      <div class="card">
        <label for="names">이름 입력 (한 줄에 1명 / 쉼표로 구분해도 됨)</label>
        <textarea id="names" placeholder="예)&#10;김철수&#10;이영희&#10;박민수&#10;최지은"></textarea>

        <div class="row" style="margin-top:10px;">
          <div>
            <label for="size">조당 인원</label><br/>
            <input id="size" type="number" min="1" value="4" />
          </div>
          <div>
            <label for="seed">시드(선택)</label><br/>
            <input id="seed" class="mono" type="number" placeholder="예: 1234" />
          </div>

          <button id="btnMake">조 편성</button>
          <button id="btnReset" class="secondary">초기화</button>
        </div>

        <div class="hint">
          * 중복 이름은 자동으로 제거하지 않아요(필요하면 옵션 추가해줄게).<br/>
          * 시드를 넣으면 같은 입력에서 같은 결과를 재현할 수 있어요.
        </div>
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

        <div id="output" class="out" style="margin-top:12px;"></div>
        <div class="footer">A, B, C... 순으로 자동 생성됩니다(필요시 더 늘어남).</div>
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
      // 줄바꿈 / 쉼표 / 탭 모두 허용
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
      // A(0)~Z(25), AA(26)~AZ, BA...
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

    function makeGroups(names, groupSize, seedValue) {
      const list = [...names];
      const rand = (seedValue !== null && seedValue !== "") ? mulberry32(Number(seedValue)) : Math.random;
      shuffle(list, rand);

      const groups = [];
      for (let i = 0; i < list.length; i += groupSize) {
        groups.push(list.slice(i, i + groupSize));
      }
      return groups;
    }

    function render(groups) {
      const out = document.getElementById("output");
      out.innerHTML = "";

      groups.forEach((members, idx) => {
        const box = document.createElement("div");
        box.className = "group";

        const h = document.createElement("h3");
        h.textContent = `${groupLabel(idx)}조 (${members.length}명)`;
        box.appendChild(h);

        members.forEach(name => {
          const p = document.createElement("span");
          p.className = "pill";
          p.textContent = name;
          box.appendChild(p);
        });

        out.appendChild(box);
      });
    }

    function toText(groups) {
      return groups
        .map((members, idx) => `${groupLabel(idx)}조: ${members.join(", ")}`)
        .join("\n");
    }

    // ---- UI wiring ----
    const namesEl = document.getElementById("names");
    const sizeEl = document.getElementById("size");
    const seedEl = document.getElementById("seed");
    const summaryEl = document.getElementById("summary");
    const toastEl = document.getElementById("toast");

    let latestText = "";

    document.getElementById("btnMake").addEventListener("click", () => {
      const names = parseNames(namesEl.value);
      const groupSize = Math.max(1, Number(sizeEl.value || 4));
      const seedValue = seedEl.value;

      if (names.length === 0) {
        summaryEl.textContent = "이름을 먼저 입력해줘.";
        document.getElementById("output").innerHTML = "";
        latestText = "";
        return;
      }

      const groups = makeGroups(names, groupSize, seedValue);
      render(groups);

      latestText = toText(groups);
      const fullGroups = Math.floor(names.length / groupSize);
      const lastSize = names.length % groupSize;

      summaryEl.textContent =
        `총 ${names.length}명 → ${groups.length}개 조 (조당 ${groupSize}명, 마지막 조 ${lastSize === 0 ? groupSize : lastSize}명)`;
    });

    document.getElementById("btnReset").addEventListener("click", () => {
      namesEl.value = "";
      seedEl.value = "";
      sizeEl.value = 4;
      document.getElementById("output").innerHTML = "";
      summaryEl.textContent = "아직 조 편성을 하지 않았어요.";
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
        // clipboard API가 막힌 환경이면 prompt로 대체
        window.prompt("복사해서 사용하세요:", latestText);
      }
    });
  </script>
</body>
</html>
