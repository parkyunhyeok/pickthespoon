<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>LET's PICK THE SPOOON</title>
  <style>
:root {
  --border: rgba(255,255,255,0.18);

  /* text */
  --text: #eaf2ff;
  --muted: rgba(234,242,255,0.78);

  /* status */
  --warn: #f59e0b;
  --ok: #22c55e;

  /* RKS x Samsung Blue */
  --blue-sky:#6EC6FF;
  --blue-samsung:#0B3C8A;
  --blue-dark:#081A33;

  /* 포인트용 그라데이션 */
  --grad-blue: linear-gradient(135deg, #6EC6FF 0%, #0B3C8A 100%);

  /* 배경 그라데이션 */
  --bg-grad: linear-gradient(180deg, #0B3C8A 0%, #081A33 100%);

  /* 카드 */
  --card-bg: rgba(255, 255, 255, 0.08);
  --card-bg-strong: rgba(255, 255, 255, 0.10);
}

body {
  font-family: system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
  margin: 24px;
  color: var(--text);
  background: var(--bg-grad);
}

.wrap { max-width: 1100px; margin: 0 auto; }

h1 {
  font-size: 26px;
  margin: 0 0 14px;
  background: var(--grad-blue);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  font-weight: 900;
  letter-spacing: 0.2px;
}

.grid { display: grid; grid-template-columns: 1fr; gap: 14px; }
@media (min-width: 980px){ .grid { grid-template-columns: 1.15fr 0.85fr; } }

textarea {
  width: 100%;
  min-height: 200px;
  resize: vertical;
  padding: 12px;
  border: 1px solid var(--border);
  border-radius: 12px;
  font-size: 14px;
  line-height: 1.45;
  background: rgba(255,255,255,0.10);
  color: var(--text);
  outline: none;
}
textarea::placeholder{ color: rgba(234,242,255,0.55); }
textarea:focus{
  border-color: rgba(110,198,255,0.60);
  box-shadow: 0 0 0 3px rgba(110,198,255,0.18);
}

.small { min-height: 120px; }

.row { display: flex; gap: 10px; flex-wrap: wrap; align-items: center; }

/* ✅ 앱 느낌 카드 */
.card {
  border: 1px solid var(--border);
  border-radius: 18px;
  padding: 16px;
  background: var(--card-bg);
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow: 0 16px 40px rgba(0,0,0,0.28);
}

/* 기본 label */
label {
  font-size: 13px;
  color: var(--muted);
  display:block;
  margin-bottom:6px;
}

/* ✅ 필수 라벨 강조 */
.label-required{
  font-weight: 900;
  font-size: 15px;
  color: #ffffff;
}
.label-required::after{
  content: " (필수)";
  font-weight: 800;
  font-size: 12px;
  color: rgba(110,198,255,0.95);
  margin-left: 6px;
}

input[type="number"]{
  width: 120px;
  padding: 8px 10px;
  border: 1px solid var(--border);
  border-radius: 10px;
  font-size: 14px;
  background: rgba(255,255,255,0.10);
  color: var(--text);
  outline: none;
}
input[type="number"]:focus{
  border-color: rgba(110,198,255,0.60);
  box-shadow: 0 0 0 3px rgba(110,198,255,0.18);
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
  box-shadow: 0 10px 24px rgba(11,60,138,0.35);
}
button:hover{ filter: brightness(1.03); }
button:active{ transform: translateY(1px); }

button.secondary{
  background: rgba(255,255,255,0.10);
  color: rgba(234,242,255,0.92);
  border: 1px solid rgba(255,255,255,0.22);
  box-shadow: none;
}

.hint { font-size: 13px; color: rgba(234,242,255,0.70); margin-top: 6px; }
.status { font-size: 13px; margin-top: 8px; }
.status.warn { color: var(--warn); }
.status.ok { color: var(--ok); }

.out { display: grid; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); gap: 12px; margin-top:12px; }

/* 결과 그룹 카드도 앱 느낌으로 */
.group {
  border: 1px solid var(--border);
  border-radius: 16px;
  padding: 12px;
  background: rgba(255,255,255,0.06);
}

.group h3{
  margin: 0 0 8px;
  font-size: 15px;
  display:flex;
  justify-content:space-between;
  gap:10px;
  color: rgba(234,242,255,0.92);
}

.pill {
  display: inline-block;
  padding: 6px 10px;
  margin: 4px 6px 0 0;
  border-radius: 999px;
  border: 1px solid rgba(255,255,255,0.18);
  background: rgba(255,255,255,0.10);
  font-size: 13px;
  color: rgba(234,242,255,0.95);
}

.badge{
  font-size:12px;
  padding:4px 8px;
  border-radius:999px;
  border:1px solid rgba(255,255,255,0.18);
  background: rgba(255,255,255,0.10);
  color: rgba(234,242,255,0.80);
}
.restBadge{ color: var(--warn); }

.footer { margin-top: 10px; font-size: 12px; color: rgba(234,242,255,0.65); }
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
            <!-- ✅ 필수 라벨: class 추가 -->
            <label for="names" class="label-required">참여자 이름(쉼표로 구분)</label>
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
            <!-- ✅ 필수 라벨: class 추가 -->
            <label for="courts" class="label-required">코트 수</label>
            <input id="courts" type="number" min="1" value="3" />
          </div>

          <div>
            <label for="seed">구분번호(필수X)</label>
            <input id="seed" class="mono" type="number" placeholder="예: 1234" />
          </div>

          <div style="display:flex; gap:8px; align-items:center; margin-top:18px;">
            <input id="enforceNoRepeatRest" type="checkbox" checked />
            <label for="enforceNoRepeatRest" style="margin:0; color:rgba(234,242,255,0.92); font-weight:700;">
              직전 휴식자는 이번 휴식자에서 제외(가능한 경우 강제)
            </label>
          </div>

          <button id="btnMake">팀 매칭</button>
          <button id="btnReset" class="secondary">초기화</button>
        </div>

        <div id="status" class="status"></div>
      </div>

      <div class="card">
        <div class="row" style="justify-content:space-between;">
          <div>
            <div style="font-weight:800; margin-bottom:4px; color: rgba(234,242,255,0.92);">결과</div>
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

  <!-- 아래 JS는 네가 준 것 그대로 붙여넣으면 됨 -->
  <script>
    // (중략) — 네 기존 JS 그대로 유지
  </script>
</body>
</html>
