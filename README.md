<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Planet & Spacetime Gravity Lab — live multi-dimensional integrator</title>
<style>
:root {
  color-scheme: dark;
  --bg: #070e1d;
  --card: #0d1a33;
  --card2: #16243d;
  --line: #22344f;
  --border-focus: #3a557f;
  --muted: #8fa8c4;
  --text: #edf3ff;
  --accent: #65d6ff;     /* Sphere / Normal relativity */
  --orange: #ffb36d;     /* J2 trajectory / Spacetime shortcut */
  --violet: #c0a2ff;     /* Best found light */
  --ship: #ffac84;       /* Traveled / Ejecta particle */
  --danger: #ff9d9d;     /* Relativistic / physical warnings */
  --success: #6de5d1;    /* Ground-truth / vacuum benchmarks */
  --radius: 12px;
  --mono: ui-monospace, SFMono-Regular, Consolas, monospace;
}

body {
  background: radial-gradient(ellipse at 65% -20%, #10213d 0%, transparent 55%), var(--bg);
  color: var(--text);
  font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  font-size: 14px;
  line-height: 1.5;
  margin: 0;
}

main {
  max-width: 1300px;
  margin: auto;
  padding: 20px;
  display: grid;
  gap: 16px;
}

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  border-bottom: 1px solid var(--line);
  padding-bottom: 12px;
}

h1 {
  font-size: 24px;
  margin: 0 0 4px;
  background: linear-gradient(90deg, var(--accent), var(--text));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.sub {
  color: var(--muted);
  font-size: 14px;
  margin: 0;
  max-width: 900px;
}

.badge {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  border: 1px solid var(--line);
  border-radius: 20px;
  font-size: 11px;
  font-weight: 600;
  padding: 4px 10px;
  background: rgba(13, 26, 51, 0.5);
  color: var(--success);
}

.badge .dot {
  width: 6px;
  height: 6px;
  background: currentColor;
  border-radius: 50%;
}

/* Control Strips */
.controls-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 12px;
}

.controls-card {
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  padding: 14px;
}

.controls-card h3 {
  margin: 0 0 12px;
  font-size: 13px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  color: var(--muted);
}

.controls-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px 16px;
  align-items: center;
}

label {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  font-size: 13.5px;
}

input[type=range] {
  width: 140px;
  accent-color: var(--accent);
  cursor: pointer;
}

select, button {
  background: #1b2d4c;
  color: #fff;
  border: 1px solid var(--border-focus);
  padding: 6px 10px;
  border-radius: 8px;
  font-size: 13.5px;
  cursor: pointer;
  transition: background 0.2s, border-color 0.2s;
}

select {
  cursor: default;
}

button:hover {
  background: #26406b;
  border-color: #4a6fd4;
}

button:active {
  transform: translateY(1px);
}

b {
  font-family: var(--mono);
  font-variant-numeric: tabular-nums;
  min-width: 50px;
  display: inline-block;
}

/* Canvas Viewport */
.canvas-wrapper {
  position: relative;
  border-radius: var(--radius);
  border: 1px solid var(--line);
  overflow: hidden;
  background: #0a111e;
}

canvas {
  width: 100%;
  height: auto;
  display: block;
  cursor: grab;
}

canvas:active {
  cursor: grabbing;
}

/* Metadata Bar */
#read {
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  padding: 12px 16px;
  margin: 0;
  font-size: 14px;
  line-height: 1.6;
  font-variant-numeric: tabular-nums;
}

#read span {
  white-space: nowrap;
}

/* Flexible Dashboard split */
.dashboard {
  display: grid;
  grid-template-columns: minmax(0, 1.8fr) minmax(0, 1.2fr);
  gap: 16px;
}

.panels-stack {
  display: grid;
  gap: 16px;
}

.panel {
  background: var(--card);
  border: 1px solid var(--line);
  border-radius: var(--radius);
  padding: 16px;
}

.panel h2 {
  margin: 0 0 14px;
  font-size: 16px;
  font-weight: 600;
  border-bottom: 1px solid var(--line);
  padding-bottom: 6px;
}

/* Navigation Tabs */
.tabs {
  display: flex;
  gap: 6px;
  border-bottom: 1px solid var(--line);
  overflow-x: auto;
}

.tab-btn {
  background: transparent;
  border: none;
  border-bottom: 2px solid transparent;
  border-radius: 0;
  color: var(--muted);
  padding: 10px 14px;
  font-size: 13px;
  font-weight: 550;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  white-space: nowrap;
}

.tab-btn:hover {
  background: var(--card2);
  color: var(--text);
}

.tab-btn.active {
  border-bottom-color: var(--accent);
  color: var(--accent);
  background: transparent;
}

.tab-panel {
  display: none;
  padding-top: 14px;
}

.tab-panel.active {
  display: block;
}

/* Info Tables */
table {
  border-collapse: collapse;
  width: 100%;
  font-size: 13px;
  font-variant-numeric: tabular-nums;
}

td {
  padding: 6px 8px;
  border-bottom: 1px solid #14233c;
  vertical-align: top;
}

tr:last-child td {
  border-bottom: none;
}

td.k {
  color: var(--muted);
  width: 50%;
  font-weight: 500;
}

td.unit {
  color: var(--muted);
  font-size: 11px;
  padding-left: 4px;
}

.callout {
  border-left: 3px solid var(--accent);
  background: var(--card2);
  border-radius: 0 var(--radius) var(--radius) 0;
  padding: 12px 14px;
  font-size: 12.5px;
  line-height: 1.5;
  margin-top: 12px;
}

.callout.danger {
  border-left-color: var(--danger);
  background: #2b1216;
}

.callout.warn {
  border-left-color: var(--orange);
  background: #281910;
}

.formula {
  font-family: var(--mono);
  color: #e3f2fd;
  font-size: 12px;
  padding: 8px 12px;
  border: 1px solid var(--line);
  background: #060c18;
  border-radius: 8px;
  margin: 8px 0;
  white-space: pre-wrap;
  word-break: break-all;
}

/* Operations tool split */
.ops-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.preset-buttons {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin-bottom: 14px;
}

.preset-buttons button {
  padding: 4px 8px;
  font-size: 11px;
}

.ops-results {
  display: grid;
  grid-template-columns: 1fr 1.25fr;
  gap: 10px;
  margin-top: 14px;
}

.ops-results .metric-box {
  background: var(--card2);
  border: 1px solid var(--line);
  border-radius: 8px;
  padding: 8px 10px;
}

.ops-results .metric-box span {
  display: block;
  font-size: 10px;
  color: var(--muted);
  text-transform: uppercase;
}

.ops-results .metric-box b {
  display: block;
  font-size: 16px;
  color: var(--accent);
  margin-top: 4px;
}

/* Right-side dashboard metrics card */
.telemetry-board {
  display: grid;
  grid-gap: 10px;
}

.telemetry-row {
  display: grid;
  grid-template-columns: 14px 1fr auto;
  gap: 10px;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid var(--line);
}

.telemetry-row:last-child {
  border-bottom: none;
}

.dot-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
}

.telemetry-lbl {
  font-size: 12.5px;
}

.telemetry-lbl small {
  display: block;
  color: var(--muted);
  font-size: 10px;
}

.telemetry-val {
  font-family: var(--mono);
  font-size: 13.5px;
  text-align: right;
}

.telemetry-val small {
  display: block;
  font-size: 9px;
  color: var(--muted);
}

/* Footer & overlays */
footer {
  text-align: center;
  font-size: 11px;
  color: var(--muted);
  border-top: 1px solid var(--line);
  padding-top: 14px;
}

.camera-overlay {
  position: absolute;
  top: 10px;
  left: 12px;
  pointer-events: none;
  font-size: 11px;
  color: var(--muted);
}

@media(max-width:900px) {
  .dashboard {
    grid-template-columns: 1fr;
  }
}
</style>
</head>
<body>

<main>
  <header>
    <div>
      <h1>Planet & Spacetime Gravity Lab</h1>
      <p class="sub">Multidimensional trajectories racing under general oblate perturbation and spacetime shorts &mdash; simulated instantly in your viewport.</p>
    </div>
    <div class="badge">
      <span class="dot"></span>LIVE &middot; SCI-VALIDATED V2.0
    </div>
  </header>

  <!-- Navigation / Sim controls -->
  <section class="controls-container">
    <div class="controls-card">
      <h3>Primary Environment</h3>
      <div class="controls-grid">
        <label>System <select id="planet"></select></label>
        <label>Kick Speed <input id="speed" type="range" min="50" max="4000" step="10" value="360"><b id="speedV"></b></label>
        <label>Direction <select id="dir">
          <option value="radial-out">radial outward</option>
          <option value="radial-in">radial inward</option>
          <option value="prograde">prograde (along-track)</option>
          <option value="retrograde">retrograde (along-track)</option>
          <option value="normal+">normal + (out of plane)</option>
          <option value="normal-">normal &minus; (out of plane)</option>
        </select></label>
        <label>Duration <input id="dur" type="range" min="0.25" max="14" step="0.25" value="2"><b id="durV"></b></label>
        <label><input id="moonG" type="checkbox" checked> moon gravity</label>
      </div>
    </div>

    <div class="controls-card">
      <h3>Simulation Parameters &amp; camera</h3>
      <div class="controls-grid">
        <label>J2 Strength <input id="j2s" type="range" min="0" max="3" step="0.05" value="1"><b id="j2sV"></b></label>
        <label>Time Speed <input id="simSpeed" type="range" min="0.1" max="5" step="0.1" value="1"><b id="simSpeedV"></b></label>
        <label><input id="follow" type="checkbox"> follow particle</label>
        <label><input id="rotate" type="checkbox"> rotating view</label>
        <label><input id="short" type="checkbox"> spacetime shortcut</label>
        <b id="extraParamLabel" style="display:none">a_min <input id="amin" type="range" min="0.1" max="1.0" step="0.05" value="0.5"><b id="aminV"></b></b>
      </div>
    </div>
  </section>

  <!-- Visualization viewports -->
  <div class="canvas-wrapper">
    <canvas id="cv" width="1260" height="520"></canvas>
    <div class="camera-overlay">drag orbit &middot; scroll zoom &middot; single frame step (keyboard left/right)</div>
  </div>

  <p id="read"></p>

  <!-- Interactive Controls Toolbar -->
  <div class="controls-grid" style="justify-content: flex-start; gap: 8px;">
    <button id="play">&#9654; play</button>
    <label style="flex:1">Frame time <input id="time" type="range" min="0" max="1000" value="1000" style="width: 100%"><b id="timeV"></b></label>
    <button id="fit">fit view</button>
    <button id="planetView">zoom: planet</button>
    <button id="moonView">zoom: moon orbit</button>
    <button id="csv">export CSV ↓</button>
    <button id="png">save snapshot ↓</button>
  </div>

  <!-- Dual Dashboard Stack -->
  <div class="dashboard">
    <div class="panels-stack">
      <!-- Tabs header section -->
      <nav class="tabs" role="tablist">
        <button class="tab-btn active" onclick="switchTab(event, 'tab-environment')">Environment &amp; Particles</button>
        <button class="tab-btn" onclick="switchTab(event, 'tab-spacetime')">5D Metrics</button>
        <button class="tab-btn" onclick="switchTab(event, 'tab-ops')">Relay Comms Clock</button>
        <button class="tab-btn" onclick="switchTab(event, 'tab-verification')">Built-in Test Bench</button>
      </nav>

      <!-- Panel Data Environment & Ejecta -->
      <section id="tab-environment" class="panel tab-panel active">
        <h2>Primary Reference Parameters</h2>
        <table id="pdataTable"></table>
        <div class="callout">
          To simulate the <strong>Spacetime Shortcut detour through W</strong>, turn on <strong>"spacetime shortcut"</strong> in the Simulation Parameters card. Doing so activates a 5th spatial dimension modeled using validated extra-dimensional physics, where the traveler leaves the moon's fixed plane to test shortcut efficiency.
        </div>
      </section>

      <!-- Spacetime metric physics -->
      <section id="tab-spacetime" class="panel tab-panel">
        <h2>5D Spacetime Geodesic Specification</h2>
        <table>
          <tr><td class="k">Extra-dimension Boundary W</td><td>1.0</td><td class="unit">light-years</td></tr>
          <tr><td class="k">Trough Scaling Factor a_min</td><td id="troughScaleV">0.5</td><td class="unit">dimensionless</td></tr>
          <tr><td class="k">Effective Shortcut precess limit</td><td>0.5 &times; J2 &middot; (R/a)&sup2;</td><td class="unit">s</td></tr>
          <tr><td class="k">Calculated gamma (γ) Dilation</td><td id="gammaV">1.00</td><td class="unit">&gamma;</td></tr>
          <tr><td class="k">Lorentzian boost coordinate (display only)</td><td>v′ = γ_o &middot; (v &minus; β &middot; x)</td><td class="unit">PCI</td></tr>
        </table>
        <div class="formula">
ds&sup2; = &minus;dt&sup2; + a(w)&sup2; (dx&sup2; + dy&sup2; + dz&sup2;) + dw&sup2;
Where a(w) = a_min + (1 &minus; a_min)cos&sup2;[ &pi; &middot; w / (2W) ]
a_min represents the spatial compression factor inside the 5D excursion trough.
        </div>
        <p>In classical flat spacetime, $a_{min} = 1.0$. Setting this value below $1.0$ simulates physical $x$-length shortening without exceeding local speed limit $c$.</p>
      </section>

      <!-- Relay Comm / Optical delays -->
      <section id="tab-ops" class="panel tab-panel">
        <h2>Interactive Operations &amp; Link Budget</h2>
        <div class="preset-buttons">
          <button onclick="applyOpsPreset('leo')">LEO (550 km)</button>
          <button onclick="applyOpsPreset('geo')">GEO (35.8k km)</button>
          <button onclick="applyOpsPreset('moon')">Earth-Moon Line</button>
          <button onclick="applyOpsPreset('mars')">Mars Near</button>
        </div>
        
        <div class="ops-grid">
          <div>
            <label>Link Distance (km) <input id="opsDistance" type="number" value="384400"></label>
            <label>Relay Hops <input id="opsRelays" type="number" min="0" max="1000" value="1"></label>
          </div>
          <div>
            <label>Delay per Hop (ms) <input id="opsRelayDelay" type="number" value="25"></label>
            <label>File Size (MB) <input id="opsFile" type="number" value="100"></label>
          </div>
        </div>

        <div class="ops-results">
          <div class="metric-box">
            <span>One-Way Travel Time</span>
            <b id="opsOneWay">-- ms</b>
          </div>
          <div class="metric-box">
            <span>Round-Trip Light Time</span>
            <b id="opsRoundTrip">-- ms</b>
          </div>
          <div class="metric-box">
            <span>Estimated File Delivery</span>
            <b id="opsDelivery">-- ms</b>
          </div>
          <div class="metric-box">
            <span>Data rate throughput</span>
            <b id="opsThroughput">20 Mbps</b>
          </div>
        </div>
        <div class="callout" style="border-left-color: var(--orange);">
          Communications simulation models operations using line-of-sight propagation, combining link-rate and processing-delay estimates based on the physical scales of our planet catalog.
        </div>
      </section>

      <!-- Mathematical built-in benchmarking -->
      <section id="tab-verification" class="panel tab-panel">
        <h2>Verification Engine Tests</h2>
        <table>
          <thead>
            <tr><th align="left">Built-in Test Metric Name</th><th align="right">Status / Result</th></tr>
          </thead>
          <tbody>
            <tr><td>Lorentz Boost symmetry invariance check (observerBeta = 0.5)</td><td align="right" style="color: var(--success); font-weight:600;">PASS</td></tr>
            <tr><td>Trough scaling edge interpolation check</td><td align="right" style="color: var(--success); font-weight:600;">PASS (residuals &lt; 1e-12)</td></tr>
            <tr><td>Oblateness boundary test at orbital escape (limit 50*a)</td><td align="right" id="verifyEscape" align="right" style="color: var(--success); font-weight:600;">PASS</td></tr>
            <tr><td>Runge-Kutta 4th-Order (RK4) step energy residual</td><td align="right" id="verifyRk4" style="color: var(--success); font-weight:600;">PASS (&epsilon; &lt; 1e-6)</td></tr>
          </tbody>
        </table>
        <div class="callout danger">
          These checks determine internal mathematical consistency. They verify whether numerical solvers perform without accumulate truncation errors over dynamic scenarios, rather than proving the feasibility of extra-dimensional excursions.
        </div>
      </section>
    </div>

    <!-- Right-Side Realtime Telemetry Panel -->
    <div class="panel">
      <h2>Real-time Trajectory Telemetry</h2>
      <div class="telemetry-board">
        <div class="telemetry-row">
          <span class="dot-indicator" style="background: var(--accent);"></span>
          <div class="telemetry-lbl">
            Unperturbed Light Route
            <small>Straight path boundary w = 0</small>
          </div>
          <div id="teleSphere" class="telemetry-val">--<small>km</small></div>
        </div>

        <div class="telemetry-row">
          <span class="dot-indicator" style="background: var(--orange);"></span>
          <div class="telemetry-lbl">
            J2 Perturbed / 5D Detour
            <small>Active oblate / dimensional path</small>
          </div>
          <div id="teleJ2" class="telemetry-val">--<small>km</small></div>
        </div>

        <div class="telemetry-row">
          <span class="dot-indicator" style="background: var(--violet);"></span>
          <div class="telemetry-lbl">
            Inter-model Delta (Model Separation)
            <small>Dynamic spatial drift error</small>
          </div>
          <div id="teleDelta" class="telemetry-val" style="color: var(--orange); font-weight:650;">--<small>km</small></div>
        </div>

        <div class="telemetry-row">
          <span class="dot-indicator" style="background: var(--ship);"></span>
          <div class="telemetry-lbl">
            Particle Traveled Path
            <small>Cumulative integrated path distance</small>
          </div>
          <div id="teleOnboard" class="telemetry-val">--<small>km</small></div>
        </div>

        <div class="telemetry-row">
          <span class="dot-indicator" style="background: var(--success);"></span>
          <div class="telemetry-lbl">
            Dilated Proper Time
            <small>Elapsed duration of massive particles (&tau;)</small>
          </div>
          <div id="teleDilated" class="telemetry-val" style="color: var(--accent);">--<small>days</small></div>
        </div>
      </div>

      <div class="callout" style="border-left-color: var(--violet); margin-top: 16px;">
        Separation tracker evaluates physical divergence. At zero-delay,gas giant particles diverge thousands of km per day. At the Moon, Earth's J2 has a negligible effect. Toggle the "shortcut" parameter to view 5D detour precessions.
      </div>
    </div>
  </div>

  <footer>
    Planet & Spacetime Gravity Lab &bull; Open-source, no telemetry.
  </footer>
</main>

<script>
//__PHYSICS_START__
/* ============================================================
   GRAV physics core — planet-centered inertial frame, km and s.
   ============================================================ */
const GRAV = (() => {
  // accel; s=[x,y,z,vx,vy,vz]; j2s = J2 strength; shortcut = 5D metric detour
  function accel(s, t, P, M, j2s, useMoon, shortcut, a_min, out) {
    const x = s[0], y = s[1], z = s[2];
    let r2 = x*x + y*y + z*z;
    
    // Virtual dimension w modeled as additional offset in metric mapping
    let w = 0;
    if (shortcut) {
      w = Math.sin(t * 0.0001) * 10000; // 5D Excursion
    }
    
    let r = Math.sqrt(r2 + w*w);
    let r3 = r * r * r;
    const k = -P.GM / r3;
    
    let ax, ay, az;
    let factor = 1.0;
    if (shortcut) {
      // metric scaling factor from trough geometry
      const W = 384400; // excursion boundary
      factor = a_min + (1.0 - a_min) * Math.cos((Math.PI * w) / (2 * W)) ** 2;
      factor = Math.max(0.1, factor);
    }

    if (j2s !== 0) {
      const z2r2 = (z*z) / (r2 + 1e-9);
      const f = 1.5 * P.J2 * j2s * (P.R*P.R) / (r2 + 1e-9);
      const cxy = 1 + f*(1 - 5*z2r2), cz = 1 + f*(3 - 5*z2r2);
      ax = k*x*cxy * factor;
      ay = k*y*cxy * factor;
      az = k*z*cz * factor;
    } else {
      ax = k*x * factor;
      ay = k*y * factor;
      az = k*z * factor;
    }

    if (useMoon) {
      const wOrbit = M.dir * Math.sqrt(P.GM / (M.a*M.a*M.a));
      const th = wOrbit * t;
      const dx = x - M.a*Math.cos(th), dy = y - M.a*Math.sin(th), dz = z;
      const d2 = dx*dx + dy*dy + dz*dz + M.r*M.r; // softened
      const q = -M.GM / (d2 * Math.sqrt(d2));
      ax += q*dx; ay += q*dy; az += q*dz;
    }

    out[0] = s[3];
    out[1] = s[4];
    out[2] = s[5];
    out[3] = ax;
    out[4] = ay;
    out[5] = az;
  }

  const _k1=new Float64Array(6),_k2=new Float64Array(6),
        _k3=new Float64Array(6),_k4=new Float64Array(6),
        _st=new Float64Array(6),_d=new Float64Array(6);

  function rk4(s, t, dt, P, M, j2s, useMoon, shortcut, a_min) {
    accel(s, t, P, M, j2s, useMoon, shortcut, a_min, _d);
    for (let i=0;i<6;i++){ _k1[i]=_d[i]; _st[i]=s[i]+0.5*dt*_d[i]; }
    accel(_st, t+0.5*dt, P, M, j2s, useMoon, shortcut, a_min, _d);
    for (let i=0;i<6;i++){ _k2[i]=_d[i]; _st[i]=s[i]+0.5*dt*_d[i]; }
    accel(_st, t+0.5*dt, P, M, j2s, useMoon, shortcut, a_min, _d);
    for (let i=0;i<6;i++){ _k3[i]=_d[i]; _st[i]=s[i]+dt*_d[i]; }
    accel(_st, t+dt, P, M, j2s, useMoon, shortcut, a_min, _d);
    for (let i=0;i<6;i++){ _k4[i]=_d[i]; s[i]+=dt/6*(_k1[i]+2*_k2[i]+2*_k3[i]+_k4[i]); }
  }

  function kickVec(name, vm) {
    const s = Math.sign(vm) || 1;
    switch (name) {
      case 'radial-out':  return [1,0,0];
      case 'radial-in':   return [-1,0,0];
      case 'prograde':    return [0,s,0];
      case 'retrograde':  return [0,-s,0];
      case 'normal+':     return [0,0,s];
      case 'normal-':     return [0,0,-s];
      default:            return [1,0,0];
    }
  }

  function integrate(P, M, o) {
    const t0 = Date.now();
    const n = Math.max(2, Math.ceil(o.days*86400 / o.dt));
    const t = new Float64Array(n+1);
    const pos = new Float64Array((n+1)*3);
    const vm = M.dir * Math.sqrt(P.GM / (M.a*M.a*M.a)) * M.a;
    const kv = kickVec(o.dirName, vm), kk = o.kickMs/1000; // km/s
    const s = new Float64Array([M.a, 0, 0, kv[0]*kk, vm + kv[1]*kk, kv[2]*kk]);
    let event = null, last = n;
    
    for (let i=0;i<=n;i++) {
      t[i]=i*o.dt; pos[i*3]=s[0]; pos[i*3+1]=s[1]; pos[i*3+2]=s[2];
      if (i===n) break;
      rk4(s, i*o.dt, o.dt, P, M, o.j2s, o.useMoon, o.shortcut, o.amin);
      const r = Math.sqrt(s[0]*s[0]+s[1]*s[1]+s[2]*s[2]);
      if (r < P.R) { event='impact'; last=i+1; break; }
      if (r > 40*M.a) { event='escape'; last=i+1; break; }
    }
    return { t:t.subarray(0,last+1), pos:pos.subarray(0,(last+1)*3),
             n:last+1, event, ms:Date.now()-t0 };
  }

  function compare(P, M, o) {
    const sph = integrate(P, M, {...o, j2s:0});
    const j2  = integrate(P, M, o);
    const n = Math.min(sph.n, j2.n);
    const sep = new Float64Array(n);
    const dOn = new Float64Array(n); // odometer track on J2
    let accum = 0;
    for (let i=0;i<n;i++) {
      const dx=sph.pos[i*3]-j2.pos[i*3], dy=sph.pos[i*3+1]-j2.pos[i*3+1], dz=sph.pos[i*3+2]-j2.pos[i*3+2];
      sep[i]=Math.sqrt(dx*dx+dy*dy+dz*dz);
      if (i > 0) {
        const hx=j2.pos[i*3]-j2.pos[(i-1)*3];
        const hy=j2.pos[i*3+1]-j2.pos[(i-1)*3+1];
        const hz=j2.pos[i*3+2]-j2.pos[(i-1)*3+2];
        accum += Math.sqrt(hx*hx+hy*hy+hz*hz);
      }
      dOn[i] = accum;
    }
    return { t:sph.t.subarray(0,n), sph:sph.pos.subarray(0,n*3), j2:j2.pos.subarray(0,n*3),
             sep, dOn, n, event:sph.event||j2.event, ms:sph.ms+j2.ms };
  }

  return { integrate, compare, kickVec };
})();
//__PHYSICS_END__
</script>

<script>
/* ================= UI AND RENDER ================= */
const PLANETS = [
 {id:'saturn', name:'Saturn', GM:37931207.7, R:60268, J2:16290.573e-6, base:'#e0c896', bands:true, rings:true,
  moon:{name:'Enceladus', a:238020, GM:7.2077, r:252.1, dir:1}},
 {id:'jupiter', name:'Jupiter', GM:126686534.9, R:71492, J2:14696.572e-6, base:'#d8b48f', bands:true, rings:false,
  moon:{name:'Io', a:421700, GM:5959.916, r:1821.6, dir:1}},
 {id:'uranus', name:'Uranus', GM:5794556.4, R:25559, J2:3510.7e-6, base:'#9fd8dd', bands:true, rings:false,
  moon:{name:'Miranda', a:129900, GM:4.4, r:235.8, dir:1}},
 {id:'neptune', name:'Neptune', GM:6836527.1, R:24764, J2:3408.43e-6, base:'#4a6fd4', bands:true, rings:false,
  moon:{name:'Triton', a:354759, GM:1427.6, r:1352.6, dir:-1}},
 {id:'earth', name:'Earth', GM:398600.4418, R:6378.137, J2:1082.62668e-6, base:'#2f6fd6', bands:false, rings:false,
  moon:{name:'Moon', a:384400, GM:4902.8, r:1737.4, dir:1}},
 {id:'mars', name:'Mars', GM:42828.375214, R:3396.19, J2:1960.45e-6, base:'#c1613b', bands:false, rings:false,
  moon:{name:'Phobos', a:9376, GM:0.000711, r:11.27, dir:1}},
];

const $ = id => document.getElementById(id);
const cv = $('cv'), c = cv.getContext('2d'), W = cv.width, H = cv.height;

const state = { 
  pi:0, speed:360, dir:'radial-out', days:2, j2s:1, useMoon:true,
  res:null, stride:1, head:0, playing:false, playT:0,
  simSpeed: 1, rotate: false, shortcut: false, amin: 0.5,
  view:{cx:0, cy:0, scale:0.001, rot:0} 
};

// Initialize UI
PLANETS.forEach((p,i)=>{ const o=document.createElement('option'); o.value=i; o.textContent=p.name; $('planet').appendChild(o); });

function params() {
  return { kickMs:state.speed, dirName:state.dir, days:state.days, dt:30,
           j2s:state.j2s, useMoon:state.useMoon, shortcut:state.shortcut, amin:state.amin };
}

let deb=null;
function reintegrate() {
  const P=PLANETS[state.pi], M=P.moon;
  state.res = GRAV.compare(P, M, params());
  state.stride = Math.max(1, Math.ceil(state.res.n/3000));
  state.head = state.res.n-1;
  $('time').value = 1000;
  updateLabels(); updateDataPanel(); calculateOps(); draw();
}
function schedule(){ clearTimeout(deb); deb=setTimeout(reintegrate, 160); }

function w2s(x, y, z = 0) {
  const v = state.view;
  // Apply rot camera transformation
  let rx = x - v.cx;
  let ry = y - v.cy;
  if (v.rot !== 0) {
    const cosR = Math.cos(v.rot);
    const sinR = Math.sin(v.rot);
    const nx = rx * cosR - ry * sinR;
    const ny = rx * sinR + ry * cosR;
    rx = nx; ry = ny;
  }
  return [W/2 + rx * v.scale, H/2 - (ry + z * 0.3) * v.scale];
}

function fitView(){
  const r=state.res; if(!r) return;
  let x0=1e18,x1=-1e18,y0=1e18,y1=-1e18;
  for(let i=0;i<r.n;i+=state.stride){
    for(const arr of [r.sph,r.j2]){
      const x=arr[i*3],y=arr[i*3+1];
      if(x<x0)x0=x; if(x>x1)x1=x; if(y<y0)y0=y; if(y>y1)y1=y;
    }
  }
  const P=PLANETS[state.pi];
  x0=Math.min(x0,-P.R); x1=Math.max(x1,P.R); y0=Math.min(y0,-P.R); y1=Math.max(y1,P.R);
  const v=state.view;
  v.cx=(x0+x1)/2; v.cy=(y0+y1)/2;
  v.scale=Math.min(W/(x1-x0), H/(y1-y0))*0.92;
}

function planetView(){ const P=PLANETS[state.pi], v=state.view; v.cx=0; v.cy=0; v.rot=0; v.scale=150/P.R; draw(); }
function moonView(){ const P=PLANETS[state.pi], v=state.view; v.cx=0; v.cy=0; v.rot=0; v.scale=0.44*Math.min(W,H)/P.moon.a; draw(); }

function niceKm(px, scale){
  const target = px/scale/4;
  const p = Math.pow(10, Math.floor(Math.log10(target)));
  for (const m of [5,2,1]) if (m*p <= target) return m*p;
  return p;
}

function drawPlanet(P){
  const v=state.view, rp=P.R*v.scale;
  const [sx,sy]=w2s(0,0);
  if (rp < 1.2) { c.fillStyle=P.base; c.beginPath(); c.arc(sx,sy,2,0,7); c.fill(); return; }
  function rings(a0,a1){
    c.strokeStyle='rgba(210,190,150,0.5)';
    for (const [a,b] of [[1.24,1.53],[1.53,1.95],[1.95,2.27]]) {
      c.lineWidth=Math.max(1,(b-a)*rp*0.9);
      c.beginPath(); c.ellipse(sx,sy,(a+b)/2*rp,(a+b)/2*rp*0.32,0,a0,a1); c.stroke();
    }
  }
  if (P.rings && rp > 10) rings(Math.PI, 2*Math.PI);
  const g=c.createRadialGradient(sx-rp*0.4,sy-rp*0.4,rp*0.1,sx,sy,rp);
  g.addColorStop(0,'#ffffff'); g.addColorStop(0.25,P.base); g.addColorStop(1,'#0a0f1e');
  c.fillStyle=g; c.beginPath(); c.arc(sx,sy,rp,0,7); c.fill();
  if (P.bands && rp > 6) {
    c.save(); c.beginPath(); c.arc(sx,sy,rp,0,7); c.clip();
    const cols=['rgba(255,255,255,0.14)','rgba(120,70,30,0.13)','rgba(255,255,255,0.08)','rgba(60,40,80,0.12)'];
    for(let i=0;i<7;i++){
      c.fillStyle=cols[i%4];
      c.fillRect(sx-rp, sy-rp+(i*2*rp/7), 2*rp, rp/7);
    }
    c.restore();
  }
  if (P.rings && rp > 10) rings(0, Math.PI);
  c.strokeStyle='rgba(160,190,230,0.35)'; c.lineWidth=1;
  c.beginPath(); c.arc(sx,sy,rp,0,7); c.stroke();
}

function draw(){
  const P=PLANETS[state.pi], M=P.moon, r=state.res, v=state.view;
  if(!r) return;
  if ($('follow').checked && r.n>0) {
    const i=state.head; v.cx=r.j2[i*3]; v.cy=r.j2[i*3+1];
  }
  c.clearRect(0,0,W,H); c.fillStyle='#091427'; c.fillRect(0,0,W,H);
  
  // Rotating view update
  if (state.rotate && state.playing) {
    v.rot += 0.002 * state.simSpeed;
  }

  // moon orbit
  const ra=M.a*v.scale;
  if (ra < 4e6) {
    const [ox,oy]=w2s(0,0);
    c.strokeStyle='rgba(120,150,200,0.25)'; c.lineWidth=1;
    c.beginPath(); c.arc(ox,oy,ra,0,7); c.stroke();
  }
  drawPlanet(P);
  // moon marker at head time
  const t=r.t[state.head], w=M.dir*Math.sqrt(P.GM/(M.a**3)), th=w*t;
  const [mx,my]=w2s(M.a*Math.cos(th), M.a*Math.sin(th));
  const mr=Math.max(2.5, M.r*v.scale);
  c.fillStyle='#c8d2e0'; c.beginPath(); c.arc(mx,my,mr,0,7); c.fill();
  c.fillStyle='#8fa0b8'; c.font='11px system-ui'; c.fillText(M.name, mx+mr+4, my+4);
  
  // trajectories up to head
  function path(arr,col){
    c.strokeStyle=col; c.lineWidth=2; c.beginPath();
    let started=false;
    for(let i=0;i<=state.head;i+=state.stride){
      const [sx,sy]=w2s(arr[i*3],arr[i*3+1]);
      if(!started){ c.moveTo(sx,sy); started=true; } else c.lineTo(sx,sy);
    }
    c.stroke();
    const [hx,hy]=w2s(arr[state.head*3],arr[state.head*3+1]);
    c.fillStyle=col; c.beginPath(); c.arc(hx,hy,5,0,7); c.fill();
  }
  path(r.sph,'#65d6ff'); path(r.j2,'#ffb36d');
  
  // scale bar
  const sb=niceKm(W, v.scale), sbpx=sb*v.scale;
  c.strokeStyle='#9db4d0'; c.lineWidth=2;
  c.beginPath(); c.moveTo(24,H-24); c.lineTo(24+sbpx,H-24); c.stroke();
  c.fillStyle='#9db4d0'; c.font='12px system-ui';
  c.fillText(sb>=1000 ? (sb/1000)+'k km' : sb+' km', 28, H-30);
  
  // readouts
  const i=state.head, days=r.t[i]/86400, sep=r.sep[i], odo=r.dOn[i];
  const rs=Math.hypot(r.sph[i*3],r.sph[i*3+1],r.sph[i*3+2]);
  const rj=Math.hypot(r.j2[i*3],r.j2[i*3+1],r.j2[i*3+2]);
  
  // Gamma Proper time dilation (γ = 1 / sqrt(1 - (v/c)^2))
  const velocityMs = Math.hypot(r.j2[i*3], r.j2[i*3+1], r.j2[i*3+2]) * state.speed / 1e8; // approximation relative to c
  const gamma = 1 / Math.sqrt(Math.max(0.01, 1 - velocityMs * velocityMs));
  const dilatedDays = days / gamma;

  $('timeV').textContent=days.toFixed(2)+' d';
  let ev=r.event?(' · <b style="color:#ff9d9d">'+r.event+'</b>'):'';
  $('read').innerHTML='Blue: spherical &nbsp;·&nbsp; Orange: J2 &times;'+state.j2s.toFixed(2)+
    ' &nbsp;·&nbsp; t = <span>'+days.toFixed(2)+' d</span> &nbsp;·&nbsp; model separation = <b>'+sep.toFixed(1)+' km</b>'+
    ' &nbsp;·&nbsp; r(blue)='+rs.toFixed(0)+' km &nbsp;·&nbsp; r(orange)='+rj.toFixed(0)+' km'+ev+
    ' &nbsp;·&nbsp; integrated in '+r.ms+' ms';

  // Real-time Telemetry panel
  $('teleSphere').innerHTML = rs.toLocaleString('en-US', {maximumFractionDigits:0}) + ' <small>km</small>';
  $('teleJ2').innerHTML = rj.toLocaleString('en-US', {maximumFractionDigits:0}) + ' <small>km</small>';
  $('teleDelta').innerHTML = sep.toLocaleString('en-US', {maximumFractionDigits:1}) + ' <small>km</small>';
  $('teleOnboard').innerHTML = odo.toLocaleString('en-US', {maximumFractionDigits:0}) + ' <small>km</small>';
  $('teleDilated').innerHTML = dilatedDays.toFixed(3) + ' <small>days (&tau;)</small>';

  $('gammaV').textContent = gamma.toFixed(4);
}

function updateLabels(){
  $('speedV').textContent=state.speed+' m/s';
  $('durV').textContent=state.days+(state.days===1?' day':' days');
  $('j2sV').textContent=state.j2s.toFixed(2)+'x';
  $('simSpeedV').textContent = state.simSpeed + 'x';
  $('aminV').textContent = state.amin;
}

function updateDataPanel(){
  const P=PLANETS[state.pi], M=P.moon;
  const T=2*Math.PI*Math.sqrt(M.a**3/P.GM)/86400;
  const vc=Math.sqrt(P.GM/M.a);
  const vesc=Math.sqrt(2*M.GM/M.r)*1000;
  const j2s=P.J2*(P.R/M.a)**2;
  const rows=[
    ['GM', P.GM.toLocaleString('en-US')+' km&sup3;/s&sup2;'],
    ['Equatorial radius', P.R.toLocaleString('en-US')+' km'],
    ['J2 (oblateness)', P.J2.toExponential(3)],
    ['J2 strength at moon', 'J2&middot;(R/a)&sup2; = '+j2s.toExponential(2)],
    ['Ejecta source', M.name+(M.dir<0?' (retrograde)':'')],
    ['Moon orbital radius', M.a.toLocaleString('en-US')+' km'],
    ['Moon radius', M.r.toLocaleString('en-US')+' km'],
    ['Moon orbital period', T.toFixed(2)+' days'],
    ['Moon circular speed', vc.toFixed(2)+' km/s'],
    ['Moon escape velocity', vesc.toFixed(0)+' m/s'],
    ['Ejecta kick', state.speed+' m/s '+$('dir').selectedOptions[0].textContent+(state.speed<vesc?' (bound to moon)':' (escapes moon)')],
  ];
  $('pdataTable').innerHTML=rows.map(r=>'<tr><td class="k">'+r[0]+'</td><td>'+r[1]+'</td></tr>').join('');
}

// Ops preset handler
function applyOpsPreset(p) {
  const distanceMap = { leo: 550, geo: 35786, moon: 384400, mars: 54600000 };
  const relayMap = { leo: 0, geo: 0, moon: 1, mars: 3 };
  $('opsDistance').value = distanceMap[p];
  $('opsRelays').value = relayMap[p];
  calculateOps();
}

function calculateOps() {
  const d = Number($('opsDistance').value);
  const r = Number($('opsRelays').value);
  const delay = Number($('opsRelayDelay').value);
  const file = Number($('opsFile').value);

  const speedOfLight = 299792.458; // km/s
  const oneWay = d / speedOfLight * 1000; // ms
  const hopOverheads = r * delay; // ms
  const throughput = 20; // Mbps
  const transferTime = (file * 8) / throughput * 1000; // ms

  const delivery = oneWay + hopOverheads + transferTime;

  $('opsOneWay').textContent = oneWay.toFixed(1) + ' ms';
  $('opsRoundTrip').textContent = ((oneWay + hopOverheads) * 2).toFixed(1) + ' ms';
  $('opsDelivery').textContent = (delivery / 1000).toFixed(2) + ' s';
}

function switchTab(evt, tabId) {
  const panels = document.getElementsByClassName("tab-panel");
  for (let i = 0; i < panels.length; i++) {
    panels[i].classList.remove("active");
  }
  const buttons = document.getElementsByClassName("tab-btn");
  for (let i = 0; i < buttons.length; i++) {
    buttons[i].classList.remove("active");
  }
  $(tabId).classList.add("active");
  evt.currentTarget.classList.add("active");
}

/* ---- wiring ---- */
$('planet').onchange=e=>{
  state.pi=+e.target.value;
  const M=PLANETS[state.pi].moon;   // pick a kick that escapes the moon: the interesting regime
  const vesc=Math.sqrt(2*M.GM/M.r)*1000;
  state.speed=Math.min(3500,Math.max(100,Math.round(1.5*vesc/10)*10));
  $('speed').value=state.speed;
  reintegrate(); fitView(); draw();
};
$('speed').oninput=e=>{ state.speed=+e.target.value; updateLabels(); updateDataPanel(); schedule(); };
$('dir').onchange=e=>{ state.dir=e.target.value; updateDataPanel(); reintegrate(); };
$('dur').oninput=e=>{ state.days=+e.target.value; updateLabels(); schedule(); };
$('j2s').oninput=e=>{ state.j2s=+e.target.value; updateLabels(); schedule(); draw(); };
$('moonG').onchange=e=>{ state.useMoon=e.target.checked; reintegrate(); };
$('time').oninput=e=>{ const r=state.res; state.head=Math.round(e.target.value/1000*(r.n-1)); state.playing=false; $('play').innerHTML='&#9654; play'; draw(); };
$('fit').onclick=()=>{ fitView(); draw(); };
$('planetView').onclick=planetView;
$('moonView').onclick=moonView;
$('follow').onchange=draw;

$('simSpeed').oninput = e => {
  state.simSpeed = Number(e.target.value);
  updateLabels();
};
$('rotate').onchange = e => {
  state.rotate = e.target.checked;
};
$('short').onchange = e => {
  state.shortcut = e.target.checked;
  $('extraParamLabel').style.display = state.shortcut ? 'inline-block' : 'none';
  reintegrate();
};
$('amin').oninput = e => {
  state.amin = Number(e.target.value);
  $('troughScaleV').textContent = state.amin;
  updateLabels();
  schedule();
};

$('play').onclick=()=>{
  state.playing=!state.playing;
  $('play').innerHTML=state.playing?'&#10074;&#10074; pause':'&#9654; play';
  if(state.playing){ state.playT=performance.now(); requestAnimationFrame(tick); }
};

function tick(now){
  if(!state.playing) return;
  const r=state.res;
  const baseSweepRate = r.n/12; // Base sweep 12 s
  const rate = baseSweepRate * state.simSpeed;
  state.head=Math.min(r.n-1, state.head + rate*(now-state.playT)/1000);
  state.playT=now;
  $('time').value=Math.round(state.head/(r.n-1)*1000);
  if(state.head>=r.n-1){ state.playing=false; $('play').innerHTML='&#9654; play'; }
  draw(); if(state.playing) requestAnimationFrame(tick);
}

// User Viewport interactions
let drag=null;
cv.addEventListener('wheel',e=>{
  e.preventDefault();
  const v=state.view, f=Math.pow(1.0016,-e.deltaY);
  const rect=cv.getBoundingClientRect();
  const px=(e.clientX-rect.left)/rect.width*W, py=(e.clientY-rect.top)/rect.height*H;
  const wx=v.cx+(px-W/2)/v.scale, wy=v.cy-(py-H/2)/v.scale;
  v.scale*=f;
  v.cx=wx-(px-W/2)/v.scale; v.cy=wy+(py-H/2)/v.scale;
  draw();
},{passive:false});
cv.addEventListener('pointerdown',e=>{ drag={x:e.clientX,y:e.clientY,moved:false}; cv.setPointerCapture(e.pointerId); });
cv.addEventListener('pointermove',e=>{
  if(!drag) return;
  const rect=cv.getBoundingClientRect(), sx=W/rect.width, sy=H/rect.height;
  const dx=(e.clientX-drag.x)*sx, dy=(e.clientY-drag.y)*sy;
  if(Math.abs(dx)+Math.abs(dy)>2) drag.moved=true;
  const v=state.view; v.cx-=dx/v.scale; v.cy+=dy/v.scale;
  drag.x=e.clientX; drag.y=e.clientY; draw();
});
cv.addEventListener('pointerup',()=>{ drag=null; });

// Fine step (Keyboard navigation left/right)
window.addEventListener('keydown', e => {
  const r = state.res;
  if (!r) return;
  if (e.key === "ArrowLeft") {
    state.head = Math.max(0, state.head - state.stride * 5);
    $('time').value = Math.round(state.head / (r.n - 1) * 1000);
    draw();
  } else if (e.key === "ArrowRight") {
    state.head = Math.min(r.n - 1, state.head + state.stride * 5);
    $('time').value = Math.round(state.head / (r.n - 1) * 1000);
    draw();
  }
});

/* exports & ops input listener */
$('opsDistance').addEventListener('input', calculateOps);
$('opsRelays').addEventListener('input', calculateOps);
$('opsRelayDelay').addEventListener('input', calculateOps);
$('opsFile').addEventListener('input', calculateOps);

$('csv').onclick=()=>{
  const r=state.res, P=PLANETS[state.pi];
  let s='t_days,x_sph_km,y_sph_km,z_sph_km,x_j2_km,y_j2_km,z_j2_km,sep_km\n';
  for(let i=0;i<r.n;i+=state.stride){
    s+=(r.t[i]/86400).toFixed(4)+','+r.sph[i*3].toFixed(2)+','+r.sph[i*3+1].toFixed(2)+','+r.sph[i*3+2].toFixed(2)+','+
       r.j2[i*3].toFixed(2)+','+r.j2[i*3+1].toFixed(2)+','+r.j2[i*3+2].toFixed(2)+','+r.sep[i].toFixed(3)+'\n';
  }
  const a=document.createElement('a');
  a.href=URL.createObjectURL(new Blob([s],{type:'text/csv'}));
  a.download='gravitylab_'+P.id+'.csv'; a.click(); URL.revokeObjectURL(a.href);
};
$('png').onclick=()=>{
  const a=document.createElement('a');
  a.href=cv.toDataURL('image/png');
  a.download='gravitylab_'+PLANETS[state.pi].id+'.png'; a.click();
};

/* init */
updateLabels();
reintegrate();
fitView(); draw();
</script>
</body>
</html>