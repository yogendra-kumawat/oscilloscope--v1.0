<img width="746" height="360" alt="image" src="https://github.com/user-attachments/assets/482b9a10-772b-4144-b45c-be53a566b9e6" />
<img width="544" height="317" alt="image" src="https://github.com/user-attachments/assets/dcc41ac5-b95d-4321-bfda-617e66328937" />
<img width="466" height="341" alt="image" src="https://github.com/user-attachments/assets/5cd9beef-68f7-453a-b8dc-805cf51b11f8" />
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Oscilloscope v1.0 — README</title>
  <link href="https://fonts.googleapis.com/css2?family=Share+Tech+Mono&family=Barlow:wght@300;400;600;700&display=swap" rel="stylesheet"/>
  <style>
    :root {
      --bg:        #0b0f14;
      --surface:   #111820;
      --border:    #1e2d3d;
      --accent:    #00e5ff;
      --accent2:   #ff6b35;
      --green:     #39ff14;
      --text:      #c8d8e8;
      --muted:     #5a7a9a;
      --mono:      'Share Tech Mono', monospace;
      --sans:      'Barlow', sans-serif;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--sans);
      font-weight: 300;
      line-height: 1.7;
      min-height: 100vh;
    }

    /* ── scan-line overlay ── */
    body::before {
      content: '';
      position: fixed; inset: 0;
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0,229,255,.018) 2px,
        rgba(0,229,255,.018) 4px
      );
      pointer-events: none;
      z-index: 999;
    }

    /* ── header ── */
    header {
      position: relative;
      padding: 56px 40px 40px;
      border-bottom: 1px solid var(--border);
      overflow: hidden;
    }
    header::after {
      content: '';
      position: absolute;
      bottom: 0; left: 0; right: 0;
      height: 2px;
      background: linear-gradient(90deg, transparent, var(--accent), var(--accent2), transparent);
    }

    .badge-row {
      display: flex; gap: 8px; flex-wrap: wrap;
      margin-bottom: 20px;
    }
    .badge {
      font-family: var(--mono);
      font-size: 11px;
      padding: 3px 10px;
      border-radius: 2px;
      border: 1px solid;
      letter-spacing: .05em;
    }
    .badge-version  { color: var(--accent);  border-color: var(--accent);  background: rgba(0,229,255,.07); }
    .badge-hw       { color: var(--accent2); border-color: var(--accent2); background: rgba(255,107,53,.07); }
    .badge-status   { color: var(--green);   border-color: var(--green);   background: rgba(57,255,20,.07); }

    h1 {
      font-family: var(--mono);
      font-size: clamp(26px, 4vw, 42px);
      letter-spacing: .04em;
      color: #fff;
      margin-bottom: 10px;
    }
    h1 span { color: var(--accent); }

    .subtitle {
      font-size: 15px;
      color: var(--muted);
      max-width: 560px;
    }

    /* ── waveform SVG decoration ── */
    .wave-deco {
      position: absolute;
      right: 40px; top: 50%;
      transform: translateY(-50%);
      opacity: .18;
    }

    /* ── main layout ── */
    main {
      max-width: 900px;
      margin: 0 auto;
      padding: 48px 40px;
      display: flex;
      flex-direction: column;
      gap: 48px;
    }

    /* ── section ── */
    section { display: flex; flex-direction: column; gap: 20px; }

    h2 {
      font-family: var(--mono);
      font-size: 13px;
      letter-spacing: .15em;
      text-transform: uppercase;
      color: var(--accent);
      display: flex;
      align-items: center;
      gap: 10px;
    }
    h2::before {
      content: '//';
      color: var(--accent2);
    }
    h2::after {
      content: '';
      flex: 1;
      height: 1px;
      background: var(--border);
    }

    h3 {
      font-family: var(--sans);
      font-weight: 600;
      font-size: 17px;
      color: #e0eaf4;
      margin-bottom: 6px;
    }

    p { font-size: 15px; color: var(--text); max-width: 720px; }

    /* ── overview card grid ── */
    .card-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
      gap: 16px;
    }
    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-top: 2px solid var(--accent);
      padding: 22px 20px;
      border-radius: 4px;
      transition: border-color .2s, transform .2s;
    }
    .card:hover { border-top-color: var(--accent2); transform: translateY(-3px); }
    .card-icon { font-size: 22px; margin-bottom: 10px; }
    .card-title { font-weight: 600; font-size: 14px; color: #fff; margin-bottom: 6px; }
    .card-desc  { font-size: 13px; color: var(--muted); line-height: 1.5; }

    /* ── block table ── */
    .block-table {
      width: 100%;
      border-collapse: collapse;
      font-size: 14px;
    }
    .block-table th {
      font-family: var(--mono);
      font-size: 11px;
      letter-spacing: .12em;
      text-transform: uppercase;
      color: var(--muted);
      padding: 10px 16px;
      text-align: left;
      border-bottom: 1px solid var(--border);
    }
    .block-table td {
      padding: 12px 16px;
      border-bottom: 1px solid rgba(30,45,61,.6);
      vertical-align: top;
    }
    .block-table tr:hover td { background: rgba(0,229,255,.04); }
    .block-table .tag {
      font-family: var(--mono);
      font-size: 12px;
      padding: 2px 8px;
      border-radius: 2px;
      background: rgba(0,229,255,.1);
      color: var(--accent);
      white-space: nowrap;
    }
    .block-table .tag.orange {
      background: rgba(255,107,53,.1);
      color: var(--accent2);
    }
    .block-table .tag.green {
      background: rgba(57,255,20,.1);
      color: var(--green);
    }

    /* ── specs list ── */
    .specs {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(260px, 1fr));
      gap: 0;
      border: 1px solid var(--border);
      border-radius: 4px;
      overflow: hidden;
    }
    .spec-item {
      padding: 14px 18px;
      border-right: 1px solid var(--border);
      border-bottom: 1px solid var(--border);
      display: flex;
      flex-direction: column;
      gap: 4px;
    }
    .spec-label { font-family: var(--mono); font-size: 11px; color: var(--muted); letter-spacing: .1em; text-transform: uppercase; }
    .spec-value { font-size: 15px; font-weight: 600; color: #fff; }

    /* ── block diagram ── */
    .block-diagram {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 6px;
      padding: 32px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 0;
      flex-wrap: wrap;
      overflow-x: auto;
    }
    .blk {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 6px;
    }
    .blk-box {
      background: #0e1820;
      border: 1px solid var(--border);
      border-radius: 4px;
      padding: 14px 20px;
      font-family: var(--mono);
      font-size: 12px;
      text-align: center;
      white-space: nowrap;
      position: relative;
      transition: border-color .2s;
    }
    .blk-box:hover { border-color: var(--accent); }
    .blk-box.hi { border-top: 2px solid var(--accent2); }
    .blk-box.primary { border-top: 2px solid var(--accent); }
    .blk-box.ctrl  { border-top: 2px solid var(--green); }
    .blk-label { font-size: 11px; color: var(--muted); }

    .arrow {
      display: flex;
      align-items: center;
      padding: 0 6px;
      color: var(--accent);
      font-size: 18px;
      padding-bottom: 20px; /* align with box vertically */
    }

    /* ── code / note block ── */
    .note {
      background: var(--surface);
      border-left: 3px solid var(--accent2);
      padding: 16px 20px;
      border-radius: 0 4px 4px 0;
      font-size: 14px;
      color: var(--text);
    }
    .note strong { color: var(--accent2); }

    /* ── footer ── */
    footer {
      text-align: center;
      padding: 32px 40px;
      border-top: 1px solid var(--border);
      font-family: var(--mono);
      font-size: 12px;
      color: var(--muted);
    }
    footer a { color: var(--accent); text-decoration: none; }
    footer a:hover { text-decoration: underline; }

    @media (max-width: 600px) {
      header, main { padding: 32px 20px; }
      .wave-deco { display: none; }
      .block-diagram { gap: 4px; }
      .arrow { padding-bottom: 0; }
    }
  </style>
</head>
<body>

<!-- ═══════════════════════════════════════════════ HEADER -->
<header>
  <div class="badge-row">
    <span class="badge badge-version">v1.0</span>
    <span class="badge badge-hw">Hardware</span>
    <span class="badge badge-status">&#9679; Active</span>
  </div>

  <h1>Oscilloscope <span>v1.0</span></h1>
  <p class="subtitle">
    Power Management &amp; Control Board — Battery charging, regulated power supply
    for MCU, and user control interface for a portable digital oscilloscope.
  </p>

  <!-- decorative waveform -->
  <svg class="wave-deco" width="260" height="80" viewBox="0 0 260 80"
       fill="none" xmlns="http://www.w3.org/2000/svg">
    <polyline points="0,40 30,40 30,10 60,10 60,70 90,70 90,10 120,10 120,70 150,70 150,40 180,40 180,20 210,20 210,60 240,60 240,40 260,40"
      stroke="#00e5ff" stroke-width="2" fill="none" stroke-linejoin="round"/>
  </svg>
</header>

<!-- ═══════════════════════════════════════════════ MAIN -->
<main>

  <!-- Overview -->
  <section>
    <h2>Overview</h2>
    <p>
      This board forms the <strong>power backbone</strong> of the Oscilloscope v1.0 project.
      It handles safe Li-ion battery charging from an external source, provides a clean
      regulated voltage rail to the microcontroller, and exposes control switches and
      regulation circuitry so the user can operate the device reliably in the field.
    </p>

    <div class="card-grid">
      <div class="card">
        <div class="card-icon">🔋</div>
        <div class="card-title">Battery Power Supply</div>
        <div class="card-desc">Li-ion / LiPo cell with dedicated charging IC for safe CC/CV charging.</div>
      </div>
      <div class="card">
        <div class="card-icon">⚡</div>
        <div class="card-title">Voltage Regulator</div>
        <div class="card-desc">LDO / switching regulator steps battery voltage down to a stable MCU rail.</div>
      </div>
      <div class="card">
        <div class="card-icon">🎛️</div>
        <div class="card-title">Control Switches</div>
        <div class="card-desc">Power on/off and mode-selection switches routed to the MCU and regulator enable pin.</div>
      </div>
      <div class="card">
        <div class="card-icon">🖥️</div>
        <div class="card-title">MCU Interface</div>
        <div class="card-desc">Clean power header and GPIO lines connecting to the oscilloscope microcontroller.</div>
      </div>
    </div>
  </section>

  <!-- Block Diagram -->
  <section>
    <h2>Block Diagram</h2>
    <div class="block-diagram">

      <div class="blk">
        <div class="blk-box hi">USB / DC<br>Input</div>
        <div class="blk-label">5 V in</div>
      </div>

      <div class="arrow">→</div>

      <div class="blk">
        <div class="blk-box hi">Battery<br>Charger IC</div>
        <div class="blk-label">CC/CV charge</div>
      </div>

      <div class="arrow">→</div>

      <div class="blk">
        <div class="blk-box hi">Li-ion / LiPo<br>Battery</div>
        <div class="blk-label">3.7 V cell</div>
      </div>

      <div class="arrow">→</div>

      <div class="blk">
        <div class="blk-box primary">Voltage<br>Regulator</div>
        <div class="blk-label">3.3 V / 5 V rail</div>
      </div>

      <div class="arrow">→</div>

      <div class="blk">
        <div class="blk-box primary">Microcontroller<br>(MCU)</div>
        <div class="blk-label">Oscilloscope core</div>
      </div>

    </div>
    <div class="note">
      <strong>Note:</strong> Control switches interface between the user and both the
      regulator enable pin and the MCU, allowing safe power-on sequencing and mode selection.
    </div>
  </section>

  <!-- Components -->
  <section>
    <h2>Key Components</h2>
    <table class="block-table">
      <thead>
        <tr>
          <th>Module</th>
          <th>Function</th>
          <th>Interface</th>
          <th>Type</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td><strong>Battery Charging IC</strong></td>
          <td>Manages CC/CV Li-ion charging; over-voltage &amp; over-current protection</td>
          <td>USB / DC jack input</td>
          <td><span class="tag orange">Power</span></td>
        </tr>
        <tr>
          <td><strong>Li-ion / LiPo Cell</strong></td>
          <td>Main energy storage; supplies power when no external source is present</td>
          <td>JST battery connector</td>
          <td><span class="tag orange">Power</span></td>
        </tr>
        <tr>
          <td><strong>Voltage Regulator</strong></td>
          <td>Converts battery voltage to stable 3.3 V or 5 V for MCU and peripherals</td>
          <td>EN pin (switch-controlled)</td>
          <td><span class="tag">Regulation</span></td>
        </tr>
        <tr>
          <td><strong>Power Switch</strong></td>
          <td>Hard on/off; also drives regulator enable to cut standby draw</td>
          <td>Mechanical toggle / tactile</td>
          <td><span class="tag green">Control</span></td>
        </tr>
        <tr>
          <td><strong>Mode / Reset Switch</strong></td>
          <td>User interaction for mode selection; feeds GPIO on MCU</td>
          <td>MCU GPIO</td>
          <td><span class="tag green">Control</span></td>
        </tr>
        <tr>
          <td><strong>Decoupling Capacitors</strong></td>
          <td>Filters noise on power rails; stabilises regulator output</td>
          <td>Inline on VCC / GND</td>
          <td><span class="tag">Passive</span></td>
        </tr>
      </tbody>
    </table>
  </section>

  <!-- Electrical Specs -->
  <section>
    <h2>Electrical Specifications</h2>
    <div class="specs">
      <div class="spec-item">
        <span class="spec-label">Input Voltage</span>
        <span class="spec-value">5 V (USB / DC)</span>
      </div>
      <div class="spec-item">
        <span class="spec-label">Battery Type</span>
        <span class="spec-value">Li-ion / LiPo</span>
      </div>
      <div class="spec-item">
        <span class="spec-label">Battery Voltage</span>
        <span class="spec-value">3.7 V nominal</span>
      </div>
      <div class="spec-item">
        <span class="spec-label">Regulated Output</span>
        <span class="spec-value">3.3 V / 5 V</span>
      </div>
      <div class="spec-item">
        <span class="spec-label">Max Charge Current</span>
        <span class="spec-value">500 mA (configurable)</span>
      </div>
      <div class="spec-item">
        <span class="spec-label">Standby Quiescent</span>
        <span class="spec-value">&lt; 1 µA (regulator off)</span>
      </div>
    </div>
  </section>

  <!-- How it Works -->
  <section>
    <h2>How It Works</h2>

    <div>
      <h3>1 — Battery Charging</h3>
      <p>
        When a USB or DC supply is connected, the charging IC enters constant-current mode
        to bulk-charge the Li-ion cell, then transitions to constant-voltage mode near full
        capacity. Built-in protection circuitry prevents overcharge, over-discharge, and
        short-circuit conditions.
      </p>
    </div>

    <div>
      <h3>2 — Regulated Power to MCU</h3>
      <p>
        The battery output feeds a low-dropout (LDO) or switching voltage regulator.
        The regulator's enable pin is tied to the power switch, so turning off the switch
        immediately cuts the MCU rail and minimises battery drain. Decoupling capacitors
        on the output suppress switching noise and ensure a clean supply.
      </p>
    </div>

    <div>
      <h3>3 — Control Switches</h3>
      <p>
        A main power toggle controls the regulator enable pin for hard on/off.
        Additional tactile or slide switches route directly to MCU GPIO lines,
        allowing the firmware to detect user input for mode changes, triggering,
        or calibration without requiring external pull-up resistors (the MCU's
        internal pull-ups are used).
      </p>
    </div>
  </section>

  <!-- Getting Started -->
  <section>
    <h2>Getting Started</h2>
    <div class="note">
      <strong>Step 1 —</strong> Connect a charged Li-ion / LiPo cell to the JST battery connector,
      observing polarity markings on the PCB silkscreen.
    </div>
    <div class="note" style="border-color: var(--accent);">
      <strong>Step 2 —</strong> (Optional) Connect a 5 V USB or DC supply to charge the battery
      via the onboard charging IC. The charge LED will illuminate during active charging.
    </div>
    <div class="note" style="border-color: var(--green);">
      <strong>Step 3 —</strong> Flip the power switch to <em>ON</em>. The voltage regulator
      enables and the MCU powers up, initiating the oscilloscope firmware.
    </div>
    <div class="note" style="border-color: var(--muted);">
      <strong>Step 4 —</strong> Use the mode/control switches to navigate the oscilloscope
      interface as described in the firmware documentation.
    </div>
  </section>

  <!-- Repository -->
  <section>
    <h2>Repository</h2>
    <p>
      Source files, schematics, and PCB layout are available at:<br/>
      <a href="https://github.com/yogendra-kumawat/oscilloscope--v1.0"
         style="color: var(--accent); font-family: var(--mono); font-size: 14px;"
         target="_blank">
        github.com/yogendra-kumawat/oscilloscope--v1.0
      </a>
    </p>
  </section>

</main>

<!-- ═══════════════════════════════════════════════ FOOTER -->
<footer>
  <p>
    Oscilloscope v1.0 &nbsp;·&nbsp;
    by <a href="https://github.com/yogendra-kumawat" target="_blank">yogendra-kumawat</a>
    &nbsp;·&nbsp; Hardware Project &nbsp;·&nbsp; Open Source
  </p>
</footer>

</body>
</html>
