<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Daily Countdown</title>
  <style>
    :root { color-scheme: light dark; }
    body {
      margin: 0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      display: grid;
      place-items: center;
      min-height: 100vh;
      padding: 16px;
      background: transparent;
    }
    .card {
      width: min(720px, 100%);
      border: 1px solid rgba(127,127,127,.25);
      border-radius: 16px;
      padding: 18px 18px 14px;
      background: rgba(127,127,127,.06);
      backdrop-filter: blur(6px);
    }
    .top {
      display: flex;
      justify-content: space-between;
      gap: 12px;
      align-items: baseline;
      flex-wrap: wrap;
    }
    .title { font-size: 14px; opacity: .8; letter-spacing: .2px; }
    .meta { font-size: 12px; opacity: .7; }
    .time {
      margin-top: 10px;
      font-size: clamp(36px, 7vw, 64px);
      font-weight: 700;
      font-variant-numeric: tabular-nums;
      letter-spacing: .6px;
    }
    .sub {
      margin-top: 8px;
      font-size: 13px;
      opacity: .75;
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
      align-items: center;
    }
    input {
      border-radius: 10px;
      border: 1px solid rgba(127,127,127,.35);
      padding: 6px 10px;
      background: transparent;
      color: inherit;
      outline: none;
    }
    button {
      border-radius: 10px;
      border: 1px solid rgba(127,127,127,.35);
      padding: 6px 10px;
      background: rgba(127,127,127,.10);
      color: inherit;
      cursor: pointer;
    }
    button:hover { background: rgba(127,127,127,.16); }
  </style>
</head>
<body>
  <div class="card">
    <div class="top">
      <div class="title" id="headline">Time left today</div>
      <div class="meta" id="today"></div>
    </div>

    <div class="time" id="countdown">--:--:--</div>

    <div class="sub">
      <span>Target:</span>
      <input id="target" type="time" step="60" />
      <button id="apply">Apply</button>
      <button id="reset">Reset (23:59)</button>
      <span id="note"></span>
    </div>
  </div>

  <script>
    const $ = (id) => document.getElementById(id);

    // Save target time in localStorage so it persists inside the embed
    const KEY = "dailyCountdownTarget";

    function pad(n) { return String(n).padStart(2, "0"); }

    function formatDate(d) {
      const opts = { weekday: "long", year: "numeric", month: "short", day: "2-digit" };
      return d.toLocaleDateString(undefined, opts);
    }

    function getTargetDate(now, hh, mm) {
      const t = new Date(now);
      t.setHours(hh, mm, 0, 0);
      // If target already passed, count down to tomorrow's target
      if (t.getTime() <= now.getTime()) t.setDate(t.getDate() + 1);
      return t;
    }

    function setNote(text="") { $("note").textContent = text; }

    function readTarget() {
      const saved = localStorage.getItem(KEY);
      if (saved) return saved;          // "HH:MM"
      return "23:59";                   // default end-of-day
    }

    function writeTarget(value) {
      localStorage.setItem(KEY, value);
    }

    function tick() {
      const now = new Date();
      $("today").textContent = formatDate(now);

      const targetStr = readTarget();
      const [hh, mm] = targetStr.split(":").map(Number);

      const target = getTargetDate(now, hh, mm);
      const diffMs = target.getTime() - now.getTime();

      const totalSec = Math.max(0, Math.floor(diffMs / 1000));
      const h = Math.floor(totalSec / 3600);
      const m = Math.floor((totalSec % 3600) / 60);
      const s = totalSec % 60;

      $("countdown").textContent = `${pad(h)}:${pad(m)}:${pad(s)}`;
      $("headline").textContent = `Time left until ${targetStr}`;

      // Small hint for “tomorrow rollover”
      if (target.getDate() !== now.getDate()) setNote("Target is tomorrow (today already passed).");
      else setNote("");
    }

    // UI wiring
    function applyFromInput() {
      const val = $("target").value;
      if (!val) return;
      writeTarget(val);
      tick();
    }

    $("apply").addEventListener("click", applyFromInput);

    $("reset").addEventListener("click", () => {
      writeTarget("23:59");
      $("target").value = "23:59";
      tick();
    });

    // Init input to saved target
    const initial = readTarget();
    $("target").value = initial;

    tick();
    setInterval(tick, 1000);
  </script>
</body>
</html>
