<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1"/>
  <title>Digital Deterrence — Software Over Steel (Clean)</title>
  <style>
    :root{
      --bg:#070A12;
      --panel:#0B1020;
      --ink:#EAF0FF;
      --muted:#A7B2D6;
      --line:rgba(255,255,255,.10);
      --accent:#7C3AED;
      --good:#22C55E;
      --bad:#EF4444;
      --warn:#F59E0B;
      --cyan:#22D3EE;
      --shadow: 0 12px 40px rgba(0,0,0,.45);
      --radius: 16px;
    }

    *{box-sizing:border-box}
    body{
      margin:0;
      font-family: ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
      background:
        radial-gradient(1200px 700px at 20% -20%, rgba(124,58,237,.32), transparent 60%),
        radial-gradient(900px 600px at 100% 0%, rgba(34,211,238,.18), transparent 55%),
        linear-gradient(180deg, #050712, #070A12);
      color:var(--ink);
    }

    header{
      padding:16px 16px 12px;
      position:sticky;
      top:0;
      backdrop-filter: blur(10px);
      background: rgba(7,10,18,.72);
      border-bottom:1px solid var(--line);
      z-index:10;
    }
    .headRow{
      max-width:1080px;
      margin:0 auto;
      display:flex;
      align-items:flex-end;
      justify-content:space-between;
      gap:12px;
    }
    header h1{margin:0;font-size:16px;font-weight:900;letter-spacing:.2px;}
    header p{margin:6px 0 0;color:var(--muted);font-size:12.5px;max-width:760px;line-height:1.35;}
    .headTag{
      font-size:11px;
      padding:6px 10px;
      border-radius:999px;
      border:1px solid var(--line);
      color:var(--muted);
      background: rgba(255,255,255,.04);
      white-space:nowrap;
    }

    main{
      max-width:1080px;
      margin:14px auto 24px;
      padding:0 14px;
      display:grid;
      grid-template-columns: 1.25fr .75fr;
      gap:14px;
    }
    @media (max-width: 980px){ main{grid-template-columns:1fr} }

    .panel{
      background: rgba(11,16,32,.76);
      border:1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      overflow:hidden;
    }
    .panelHeader{
      padding:14px;
      display:flex;
      align-items:center;
      justify-content:space-between;
      gap:10px;
      border-bottom:1px solid var(--line);
      background: linear-gradient(180deg, rgba(255,255,255,.03), transparent);
    }
    .panelHeader .left{display:flex;flex-direction:column;gap:4px;}
    .panelHeader .title{font-weight:950;font-size:13px;}
    .panelHeader .sub{font-size:12px;color:var(--muted);}

    .pill{
      display:inline-flex;align-items:center;gap:8px;
      font-size:11px;padding:6px 10px;border-radius:999px;
      border:1px solid var(--line);
      background: rgba(255,255,255,.04);
      color:var(--muted);
      white-space:nowrap;
    }
    .dot{
      width:8px;height:8px;border-radius:999px;
      background: var(--cyan);
      box-shadow: 0 0 18px rgba(34,211,238,.55);
    }
    .content{padding:14px;}

    /* Timeline */
    .timeline{display:flex;gap:8px;align-items:center;flex-wrap:wrap;margin-top:6px;}
    .node{
      display:flex;align-items:center;gap:8px;
      padding:6px 10px;border-radius:999px;border:1px solid var(--line);
      background: rgba(255,255,255,.03);
      color:var(--muted);font-size:11px;user-select:none;
    }
    .node .n{
      width:18px;height:18px;border-radius:999px;display:grid;place-items:center;
      font-weight:900;font-size:11px;background: rgba(255,255,255,.06);
      border:1px solid var(--line);color:var(--ink);
    }
    .node.active{border-color: rgba(124,58,237,.65);background: rgba(124,58,237,.12);color:var(--ink);}
    .node.done{border-color: rgba(34,197,94,.55);background: rgba(34,197,94,.10);color:var(--ink);}

    /* Map */
    .mapCard{
      border:1px solid var(--line);
      border-radius: 14px;
      background: radial-gradient(900px 320px at 50% 0%, rgba(34,211,238,.10), transparent 55%),
                  linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.01));
      overflow:hidden;
      position:relative;
      min-height: 250px;
    }
    .mapOverlay{
      position:absolute; inset:0; pointer-events:none;
      background: linear-gradient(0deg, rgba(7,10,18,.62), transparent 45%);
    }
    .mapHUD{
      position:absolute; left:12px; top:12px;
      display:flex; flex-direction:column; gap:6px; pointer-events:none;
    }
    .mapHUD .h{font-size:11px;color:var(--muted);}
    .mapHUD .big{font-size:13px;font-weight:950;color:var(--ink);}

    .pulse{animation:pulse 1.8s ease-in-out infinite;}
    @keyframes pulse{
      0%,100%{filter: drop-shadow(0 0 0 rgba(34,211,238,0))}
      50%{filter: drop-shadow(0 0 12px rgba(34,211,238,.40))}
    }

    /* Event card */
    .eventCard{
      margin-top:12px;
      border:1px solid var(--line);
      border-radius:14px;
      overflow:hidden;
      background: linear-gradient(180deg, rgba(124,58,237,.10), rgba(255,255,255,.02));
    }
    .eventTop{
      padding:12px;
      display:flex;align-items:flex-start;justify-content:space-between;gap:10px;
      border-bottom:1px solid var(--line);
    }
    .badge{
      display:inline-flex;align-items:center;gap:8px;
      font-size:11px;padding:6px 10px;border-radius:999px;
      background: rgba(124,58,237,.20);border:1px solid rgba(124,58,237,.45);
      color:var(--ink);white-space:nowrap;
    }
    .spark{width:8px;height:8px;border-radius:999px;background:var(--accent);box-shadow: 0 0 18px rgba(124,58,237,.65);}
    .eventText{padding:12px;font-size:13px;line-height:1.45;color:var(--ink);}

    .stressRow{
      padding:10px 12px 12px;
      display:grid;grid-template-columns: repeat(4,1fr);gap:10px;
    }
    @media (max-width: 560px){ .stressRow{grid-template-columns:repeat(2,1fr)} }
    .meter{border:1px solid var(--line);border-radius:12px;padding:10px;background: rgba(0,0,0,.18);}
    .meter .k{font-size:11px;color:var(--muted);margin-bottom:8px;}
    .meter .bar{height:10px;border-radius:999px;background: rgba(255,255,255,.08);position:relative;overflow:hidden;}
    .meter .fill{position:absolute;left:0;top:0;height:100%;width:0%;border-radius:999px;background: linear-gradient(90deg, rgba(34,211,238,.9), rgba(124,58,237,.9));}

    /* Right column: allocator + KPIs */
    .allocator{
      border:1px solid var(--line);
      border-radius:14px;
      background: rgba(0,0,0,.18);
      padding:12px;
    }
    .budgetRow{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-bottom:10px;}
    .budgetRow .b{font-weight:950;font-size:12px;}
    .budgetRow .remain{font-size:12px;color:var(--muted);}
    .tokenBar{height:10px;border-radius:999px;background: rgba(255,255,255,.08);overflow:hidden;position:relative;}
    .tokenFill{position:absolute;inset:0 auto 0 0;width:0%;border-radius:999px;background: linear-gradient(90deg, rgba(34,197,94,.9), rgba(34,211,238,.85), rgba(124,58,237,.85));transition: width .25s ease;}

    .allocGrid{display:grid;gap:10px;margin-top:12px;}
    .allocItem{
      border:1px solid var(--line);
      border-radius:14px;
      padding:10px;
      background: rgba(255,255,255,.03);
      display:grid;
      grid-template-columns: 1fr auto;
      gap:10px;
      align-items:center;
    }
    .name{font-weight:950;font-size:12.5px;display:flex;gap:8px;align-items:center;}
    .icon{width:26px;height:26px;border-radius:10px;display:grid;place-items:center;border:1px solid var(--line);background: rgba(255,255,255,.05);font-size:14px;}
    .desc{font-size:11px;color:var(--muted);margin-top:4px;line-height:1.25;}

    .stepper{display:flex;align-items:center;gap:8px;}
    .btn{
      border:1px solid var(--line);
      background: rgba(255,255,255,.04);
      color: var(--ink);
      border-radius: 12px;
      padding:10px 12px;
      font-weight:950;
      cursor:pointer;
      user-select:none;
    }
    .btn:hover{background: rgba(255,255,255,.06)}
    .btn:active{transform: translateY(1px)}
    .btnPrimary{background: rgba(124,58,237,.22);border-color: rgba(124,58,237,.55);}
    .btnPrimary:hover{background: rgba(124,58,237,.28)}
    .btnDanger{background: rgba(239,68,68,.18);border-color: rgba(239,68,68,.45);}
    .btnDanger:hover{background: rgba(239,68,68,.24)}
    .btnDisabled{opacity:.55;cursor:not-allowed;pointer-events:none;}

    .count{
      min-width:36px;text-align:center;font-weight:950;font-variant-numeric: tabular-nums;
      border:1px solid var(--line);background: rgba(0,0,0,.18);
      padding:8px 10px;border-radius:12px;
    }
    .hint{font-size:11px;color:var(--muted);line-height:1.35;margin-top:10px;}

    /* Action choice (single layer) */
    .choiceBox{
      margin-top:12px;
      border:1px solid var(--line);
      border-radius:14px;
      background: rgba(0,0,0,.18);
      padding:12px;
    }
    .choiceTitle{font-weight:950;font-size:12px;margin-bottom:8px;color:var(--ink);}
    .chips{display:flex;gap:8px;flex-wrap:wrap;}
    .chip{
      border:1px solid var(--line);
      background: rgba(255,255,255,.04);
      color: var(--muted);
      padding:8px 10px;
      border-radius:999px;
      font-size:11px;
      cursor:pointer;
      user-select:none;
    }
    .chip.active{
      color: var(--ink);
      border-color: rgba(34,211,238,.55);
      background: rgba(34,211,238,.10);
    }

    /* KPIs */
    .grid2{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
    @media (max-width: 560px){ .grid2{grid-template-columns:1fr} }
    .kpiCard{border:1px solid var(--line);border-radius:14px;background: rgba(0,0,0,.16);padding:12px;}
    .kpiCard .k{font-size:11px;color:var(--muted);margin-bottom:8px;display:flex;justify-content:space-between;gap:8px;}
    .kpiCard .bar{height:10px;border-radius:999px;background: rgba(255,255,255,.08);overflow:hidden;position:relative;}
    .kpiCard .fill{position:absolute;inset:0 auto 0 0;width:0%;border-radius:999px;background: linear-gradient(90deg, rgba(34,211,238,.95), rgba(124,58,237,.85));}

    /* AAR */
    .aar{
      margin-top:12px;
      border:1px solid var(--line);
      border-radius:14px;
      background: linear-gradient(180deg, rgba(34,211,238,.10), rgba(255,255,255,.02));
      padding:12px;
      display:none;
    }
    .aar h3{margin:0 0 6px;font-size:12.5px;font-weight:950;}
    .deltaRow{display:flex;gap:8px;flex-wrap:wrap;margin:8px 0 10px;}
    .delta{
      font-size:11px;padding:6px 10px;border-radius:999px;border:1px solid var(--line);
      background: rgba(0,0,0,.20);color: var(--muted);white-space:nowrap;
    }
    .delta.good{border-color: rgba(34,197,94,.55);background: rgba(34,197,94,.10);color: var(--ink);}
    .delta.bad{border-color: rgba(239,68,68,.55);background: rgba(239,68,68,.10);color: var(--ink);}
    .delta.warn{border-color: rgba(245,158,11,.55);background: rgba(245,158,11,.10);color: var(--ink);}
    .aar ul{margin:0;padding-left:18px;line-height:1.38;font-size:12.5px;color: var(--ink);}

    .footerActions{display:flex;justify-content:flex-end;gap:8px;flex-wrap:wrap;margin-top:12px;}

    table{width:100%;border-collapse:collapse;margin-top:10px;font-size:12px;border-radius:14px;overflow:hidden;}
    th,td{border:1px solid rgba(255,255,255,.10);padding:10px;vertical-align:top;}
    th{background: rgba(255,255,255,.05);text-align:left;color:var(--ink);font-weight:950;}
    tr:nth-child(even) td{background: rgba(255,255,255,.02);}
    .hl td{background: rgba(245,158,11,.10) !important;}

    .mono{font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;}
  </style>
</head>
<body>
<header>
  <div class="headRow">
    <div>
      <h1>Digital Deterrence: Can Software Substitute for Strategic Depth?</h1>
      <p>
        Allocate <span class="mono">100</span> points across AI-ISR, HUMINT, Alliances, and Conventional readiness.
        Then choose one action and commit. Watch how deterrence, warning, resilience, escalation risk, and legitimacy shift.
      </p>
    </div>
    <div class="headTag">8 turns • simple choices • export CSV</div>
  </div>
</header>

<main>
  <!-- LEFT: Map + briefing -->
  <section class="panel">
    <div class="panelHeader">
      <div class="left">
        <div class="title" id="turnTitle">Ready</div>
        <div class="sub" id="turnSub">Click Start. Then allocate to 100 and commit each turn.</div>
        <div class="timeline" id="timeline"></div>
      </div>
      <div class="pill"><span class="dot" id="modeDot"></span><span id="modeText">Standby</span></div>
    </div>

    <div class="content">
      <div class="mapCard">
        <svg viewBox="0 0 900 360" width="100%" height="100%" preserveAspectRatio="none">
          <defs>
            <radialGradient id="pulseGrad" cx="50%" cy="50%" r="50%">
              <stop offset="0%" stop-color="rgba(34,211,238,.45)"/>
              <stop offset="100%" stop-color="rgba(34,211,238,0)"/>
            </radialGradient>
          </defs>

          <rect id="zoneCyber" x="40" y="40" width="380" height="130" rx="18" fill="rgba(34,197,94,.08)" stroke="rgba(34,197,94,.25)"/>
          <rect id="zoneAir" x="460" y="40" width="400" height="130" rx="18" fill="rgba(124,58,237,.08)" stroke="rgba(124,58,237,.25)"/>
          <rect id="zoneMaritime" x="40" y="210" width="380" height="120" rx="18" fill="rgba(34,211,238,.08)" stroke="rgba(34,211,238,.25)"/>
          <rect id="zoneInfo" x="460" y="210" width="400" height="120" rx="18" fill="rgba(245,158,11,.08)" stroke="rgba(245,158,11,.25)"/>

          <text x="70" y="75" fill="rgba(234,240,255,.85)" font-size="18" font-weight="800">CYBER</text>
          <text x="490" y="75" fill="rgba(234,240,255,.85)" font-size="18" font-weight="800">AIRSPACE</text>
          <text x="70" y="245" fill="rgba(234,240,255,.85)" font-size="18" font-weight="800">MARITIME</text>
          <text x="490" y="245" fill="rgba(234,240,255,.85)" font-size="18" font-weight="800">INFORMATION</text>

          <circle id="pulse" cx="450" cy="180" r="56" fill="url(#pulseGrad)" opacity="0.25"></circle>
        </svg>

        <div class="mapHUD">
          <div class="h">Active domain</div>
          <div class="big" id="activeDomain">Standby</div>
        </div>
        <div class="mapOverlay"></div>
      </div>

      <div class="eventCard">
        <div class="eventTop">
          <div class="badge"><span class="spark"></span><span id="eventTag">Briefing</span></div>
          <div class="pill"><span class="dot"></span><span id="turnCount">0 / 8</span></div>
        </div>
        <div class="eventText" id="eventText">
          Click <b>Start</b>. Then allocate your budget to 100 and choose one action.
        </div>
        <div class="stressRow">
          <div class="meter"><div class="k">Threat</div><div class="bar"><div class="fill" id="mThreat"></div></div></div>
          <div class="meter"><div class="k">Cyber</div><div class="bar"><div class="fill" id="mCyber"></div></div></div>
          <div class="meter"><div class="k">Ambiguity</div><div class="bar"><div class="fill" id="mAmb"></div></div></div>
          <div class="meter"><div class="k">Politics</div><div class="bar"><div class="fill" id="mPol"></div></div></div>
        </div>
      </div>
    </div>
  </section>

  <!-- RIGHT: Controls + KPIs + AAR -->
  <section class="panel">
    <div class="panelHeader">
      <div class="left">
        <div class="title">Decision Console</div>
        <div class="sub">Allocate • choose action • commit</div>
      </div>
      <div class="pill"><span class="dot" id="readyDot"></span><span id="readyText">Not ready</span></div>
    </div>

    <div class="content">
      <div class="allocator">
        <div class="budgetRow">
          <div class="b">Budget</div>
          <div class="remain">Remaining: <span class="mono" id="remain">100</span> / 100</div>
        </div>
        <div class="tokenBar"><div class="tokenFill" id="tokenFill"></div></div>

        <div class="allocGrid">
          <div class="allocItem">
            <div>
              <div class="name"><span class="icon">📡</span>AI-ISR</div>
              <div class="desc">Sensors, fusion, automation</div>
            </div>
            <div class="stepper">
              <div class="btn" data-key="ai" data-step="-5">−5</div>
              <div class="btn" data-key="ai" data-step="-1">−</div>
              <div class="count mono" id="aiVal">25</div>
              <div class="btn" data-key="ai" data-step="1">+</div>
              <div class="btn" data-key="ai" data-step="5">+5</div>
            </div>
          </div>

          <div class="allocItem">
            <div>
              <div class="name"><span class="icon">🕵️</span>HUMINT</div>
              <div class="desc">Sources, access, intent readout</div>
            </div>
            <div class="stepper">
              <div class="btn" data-key="hum" data-step="-5">−5</div>
              <div class="btn" data-key="hum" data-step="-1">−</div>
              <div class="count mono" id="humVal">25</div>
              <div class="btn" data-key="hum" data-step="1">+</div>
              <div class="btn" data-key="hum" data-step="5">+5</div>
            </div>
          </div>

          <div class="allocItem">
            <div>
              <div class="name"><span class="icon">🤝</span>Alliances</div>
              <div class="desc">Sharing, liaison, political cover</div>
            </div>
            <div class="stepper">
              <div class="btn" data-key="ally" data-step="-5">−5</div>
              <div class="btn" data-key="ally" data-step="-1">−</div>
              <div class="count mono" id="allyVal">25</div>
              <div class="btn" data-key="ally" data-step="1">+</div>
              <div class="btn" data-key="ally" data-step="5">+5</div>
            </div>
          </div>

          <div class="allocItem">
            <div>
              <div class="name"><span class="icon">🛡️</span>Conventional</div>
              <div class="desc">Patrols, readiness, deterrence</div>
            </div>
            <div class="stepper">
              <div class="btn" data-key="conv" data-step="-5">−5</div>
              <div class="btn" data-key="conv" data-step="-1">−</div>
              <div class="count mono" id="convVal">25</div>
              <div class="btn" data-key="conv" data-step="1">+</div>
              <div class="btn" data-key="conv" data-step="5">+5</div>
            </div>
          </div>
        </div>

        <div class="hint">
          Commit activates only at exactly 100. Use ±5 for speed.
        </div>

        <div class="footerActions">
          <button class="btn btnDanger" id="resetBtn">Reset</button>
          <button class="btn" id="autoBtn">Auto-balance</button>
          <button class="btn btnPrimary btnDisabled" id="commitBtn">Commit</button>
        </div>
      </div>

      <div class="choiceBox">
        <div class="choiceTitle">Choose one action</div>
        <div class="chips" id="actionChips"></div>
      </div>

      <div class="panel" style="margin-top:12px;border-radius:14px;">
        <div class="panelHeader" style="border-radius:14px 14px 0 0;">
          <div class="left">
            <div class="title">System Status</div>
            <div class="sub">KPIs update after each commitment.</div>
          </div>
          <div class="pill"><span class="dot pulse"></span><span>Live</span></div>
        </div>
        <div class="content">
          <div class="grid2">
            <div class="kpiCard"><div class="k"><span>Deterrence</span><span class="mono" id="detN">50</span></div><div class="bar"><div class="fill" id="detFill"></div></div></div>
            <div class="kpiCard"><div class="k"><span>Warning</span><span class="mono" id="warnN">50</span></div><div class="bar"><div class="fill" id="warnFill"></div></div></div>
            <div class="kpiCard"><div class="k"><span>Resilience</span><span class="mono" id="resN">50</span></div><div class="bar"><div class="fill" id="resFill"></div></div></div>
            <div class="kpiCard"><div class="k"><span>Escalation</span><span class="mono" id="escN">30</span></div><div class="bar"><div class="fill" id="escFill"></div></div></div>
            <div class="kpiCard" style="grid-column:1 / -1;"><div class="k"><span>Legitimacy</span><span class="mono" id="repN">60</span></div><div class="bar"><div class="fill" id="repFill"></div></div></div>
          </div>

          <div class="aar" id="aar">
            <h3>After-Action Review</h3>
            <div class="deltaRow" id="deltaRow"></div>
            <ul id="aarList"></ul>
            <div class="footerActions">
              <button class="btn btnPrimary" id="nextBtn">Next turn</button>
              <button class="btn" id="csvBtn">Download CSV</button>
            </div>
          </div>

          <div class="footerActions" style="margin-top:12px;">
            <button class="btn btnPrimary" id="startBtn">Start</button>
          </div>

          <div id="summaryWrap"></div>
        </div>
      </div>
    </div>
  </section>
</main>

<script>
  const TURNS = [
    { id:1, tag:"Baseline posture", domain:"baseline",
      text:"Routine day. Tensions are stable but persistent. Choose a sustainable posture: credible deterrence without provoking escalation.",
      stress:{ threat:10, cyber:10, amb:10, pol:10 },
      actions:[
        { key:"monitor", label:"Quiet monitoring", note:"Preserves legitimacy." },
        { key:"tabletop", label:"Tabletop exercise", note:"Small resilience gain." },
        { key:"liaison", label:"Liaison check-in", note:"Small warning gain." }
      ]
    },
    { id:2, tag:"Maritime ambiguity", domain:"maritime",
      text:"Unmarked small craft cluster near an offshore energy asset. Signals are noisy; intent is unclear. False alarms are costly.",
      stress:{ threat:25, cyber:10, amb:30, pol:20 },
      actions:[
        { key:"patrol", label:"Increase patrols", note:"Deterrence up; escalation risk up." },
        { key:"shadow", label:"Shadow discreetly", note:"Warning up; escalation risk down." },
        { key:"query", label:"Query partners", note:"Warning up; small autonomy cost." }
      ]
    },
    { id:3, tag:"Drone harassment", domain:"air",
      text:"Low-cost drones near a sensitive site. Attribution is uncertain. Public expects response, but escalation risks are real.",
      stress:{ threat:35, cyber:10, amb:20, pol:25 },
      actions:[
        { key:"intercept", label:"Intercept & jam", note:"Deterrence up; escalation risk up." },
        { key:"harden", label:"Harden defenses", note:"Resilience up." },
        { key:"attrib", label:"Public attribution", note:"Legitimacy gamble." }
      ]
    },
    { id:4, tag:"Cyber intrusion", domain:"cyber",
      text:"Spearphishing targets government and energy firms. Confidence is high. Disruption is limited, but persistence is likely.",
      stress:{ threat:15, cyber:40, amb:15, pol:20 },
      actions:[
        { key:"hunt", label:"Threat-hunt surge", note:"Warning and resilience up." },
        { key:"patch", label:"Emergency patching", note:"Resilience up." },
        { key:"quietmsg", label:"Quiet diplomatic message", note:"Escalation risk down." }
      ]
    },
    { id:5, tag:"Disinformation burst", domain:"info",
      text:"A coordinated online campaign undermines trust. The story spreads internationally. Legitimacy becomes a security variable.",
      stress:{ threat:10, cyber:25, amb:20, pol:45 },
      actions:[
        { key:"trans", label:"Transparency briefing", note:"Legitimacy up." },
        { key:"counter", label:"Counter-messaging", note:"Small legitimacy up." },
        { key:"law", label:"Legal push", note:"Slow, but signals seriousness." }
      ]
    },
    { id:6, tag:"Alliance pressure", domain:"info",
      text:"A major partner offers enhanced early warning for expanded access. Domestic sovereignty concerns intensify.",
      stress:{ threat:20, cyber:15, amb:15, pol:45 },
      actions:[
        { key:"accept", label:"Accept package", note:"Warning up; legitimacy down." },
        { key:"limit", label:"Limited acceptance", note:"Balanced gains." },
        { key:"decline", label:"Decline politely", note:"Legitimacy up; warning cost." }
      ]
    },
    { id:7, tag:"Deception and clutter", domain:"maritime",
      text:"Traffic surges. Sensors saturate. This is exactly when adversaries hide probes. Ambiguity spikes.",
      stress:{ threat:30, cyber:15, amb:45, pol:20 },
      actions:[
        { key:"filter", label:"Tighten filters", note:"Warning up; risk of misses." },
        { key:"surgehum", label:"Surge HUMINT", note:"Reduces surprise." },
        { key:"task", label:"Stand up taskforce", note:"Resilience up." }
      ]
    },
    { id:8, tag:"Crisis peak", domain:"air",
      text:"Multi-domain incident: drones, cyber probing, ambiguous naval movement. Deter without triggering a spiral. Investors are watching.",
      stress:{ threat:45, cyber:25, amb:30, pol:30 },
      actions:[
        { key:"prop", label:"Proportional response", note:"Escalation control up." },
        { key:"forward", label:"Forward posture", note:"Deterrence up; escalation risk up." },
        { key:"coal", label:"Coalition signalling", note:"Legitimacy and warning up." }
      ]
    }
  ];

  const ACTION = {
    monitor:{ rep:+1 },
    tabletop:{ res:+3 },
    liaison:{ warn:+2 },

    patrol:{ det:+3, esc:+2, rep:-1 },
    shadow:{ warn:+3, esc:-1, rep:+1 },
    query:{ warn:+4, rep:-1 },

    intercept:{ det:+4, esc:+4, rep:-1 },
    harden:{ res:+4, det:+1 },
    attrib:{ rep:+2, det:+1, esc:+1 },

    hunt:{ warn:+3, res:+3 },
    patch:{ res:+4, rep:+1 },
    quietmsg:{ esc:-2, rep:+1, det:+1 },

    trans:{ rep:+5, warn:+1 },
    counter:{ rep:+2, warn:+1 },
    law:{ rep:+2, det:+1 },

    accept:{ warn:+5, rep:-3, esc:+1 },
    limit:{ warn:+3, rep:-1 },
    decline:{ rep:+3, warn:-2 },

    filter:{ warn:+2 },
    surgehum:{ warn:+3, res:+1, esc:-1 },
    task:{ res:+3, warn:+1 },

    prop:{ esc:-3, rep:+2 },
    forward:{ det:+4, esc:+4, rep:-1 },
    coal:{ warn:+3, rep:+3, det:+1, esc:-1 }
  };

  let idx = 0, started = false, locked = false;
  let alloc = { ai:25, hum:25, ally:25, conv:25 };
  let action = "monitor";
  let kpi = { det:50, warn:50, res:50, esc:30, rep:60 };
  const history = [];

  // DOM
  const timeline = document.getElementById("timeline");
  const turnTitle = document.getElementById("turnTitle");
  const turnSub = document.getElementById("turnSub");
  const eventTag = document.getElementById("eventTag");
  const eventText = document.getElementById("eventText");
  const turnCount = document.getElementById("turnCount");
  const activeDomain = document.getElementById("activeDomain");

  const mThreat = document.getElementById("mThreat");
  const mCyber = document.getElementById("mCyber");
  const mAmb = document.getElementById("mAmb");
  const mPol = document.getElementById("mPol");

  const remainEl = document.getElementById("remain");
  const tokenFill = document.getElementById("tokenFill");
  const aiVal = document.getElementById("aiVal");
  const humVal = document.getElementById("humVal");
  const allyVal = document.getElementById("allyVal");
  const convVal = document.getElementById("convVal");

  const readyText = document.getElementById("readyText");
  const readyDot = document.getElementById("readyDot");
  const modeText = document.getElementById("modeText");
  const modeDot = document.getElementById("modeDot");

  const actionChips = document.getElementById("actionChips");
  const commitBtn = document.getElementById("commitBtn");
  const resetBtn = document.getElementById("resetBtn");
  const autoBtn = document.getElementById("autoBtn");
  const startBtn = document.getElementById("startBtn");
  const nextBtn = document.getElementById("nextBtn");
  const csvBtn = document.getElementById("csvBtn");

  const aar = document.getElementById("aar");
  const aarList = document.getElementById("aarList");
  const deltaRow = document.getElementById("deltaRow");
  const summaryWrap = document.getElementById("summaryWrap");

  const detFill = document.getElementById("detFill");
  const warnFill = document.getElementById("warnFill");
  const resFill = document.getElementById("resFill");
  const escFill = document.getElementById("escFill");
  const repFill = document.getElementById("repFill");
  const detN = document.getElementById("detN");
  const warnN = document.getElementById("warnN");
  const resN = document.getElementById("resN");
  const escN = document.getElementById("escN");
  const repN = document.getElementById("repN");

  const pulse = document.getElementById("pulse");
  const zoneMaritime = document.getElementById("zoneMaritime");
  const zoneAir = document.getElementById("zoneAir");
  const zoneCyber = document.getElementById("zoneCyber");
  const zoneInfo = document.getElementById("zoneInfo");

  const clamp = (x,a=0,b=100)=>Math.max(a,Math.min(b,x));
  const sumAlloc = ()=>alloc.ai+alloc.hum+alloc.ally+alloc.conv;

  function setMode(text, color){
    modeText.textContent = text;
    modeDot.style.background = color || "var(--cyan)";
    modeDot.style.boxShadow = `0 0 18px ${color || "rgba(34,211,238,.55)"}`;
  }

  function setReady(ok){
    if(ok){
      readyText.textContent = "Ready";
      readyDot.style.background = "var(--good)";
      readyDot.style.boxShadow = "0 0 18px rgba(34,197,94,.55)";
    }else{
      readyText.textContent = "Not ready";
      readyDot.style.background = "var(--bad)";
      readyDot.style.boxShadow = "0 0 18px rgba(239,68,68,.45)";
    }
  }

  function renderTimeline(){
    timeline.innerHTML = "";
    for(let i=1;i<=TURNS.length;i++){
      const node = document.createElement("div");
      node.className = "node";
      if(started && i === idx+1) node.classList.add("active");
      if(started && i < idx+1) node.classList.add("done");
      node.innerHTML = `<div class="n">${i}</div><div>Turn</div>`;
      timeline.appendChild(node);
    }
  }

  function renderMeters(stress){
    const set = (el,v)=>{ el.style.width = `${clamp(v)}%`; };
    set(mThreat, stress.threat);
    set(mCyber, stress.cyber);
    set(mAmb, stress.amb);
    set(mPol, stress.pol);
  }

  function highlightDomain(domain){
    const reset = ()=>{
      zoneMaritime.setAttribute("stroke","rgba(34,211,238,.25)");
      zoneAir.setAttribute("stroke","rgba(124,58,237,.25)");
      zoneCyber.setAttribute("stroke","rgba(34,197,94,.25)");
      zoneInfo.setAttribute("stroke","rgba(245,158,11,.25)");
    };
    reset();

    let x=450,y=180,label="Standby";
    if(domain==="cyber"){ x=210; y=110; label="Cyber"; zoneCyber.setAttribute("stroke","rgba(34,197,94,.70)"); }
    if(domain==="air"){ x=650; y=110; label="Airspace"; zoneAir.setAttribute("stroke","rgba(124,58,237,.75)"); }
    if(domain==="maritime"){ x=210; y=270; label="Maritime"; zoneMaritime.setAttribute("stroke","rgba(34,211,238,.75)"); }
    if(domain==="info"){ x=650; y=270; label="Information"; zoneInfo.setAttribute("stroke","rgba(245,158,11,.75)"); }
    if(domain==="baseline"){ x=450; y=180; label="Multi-domain baseline"; }

    activeDomain.textContent = label;
    pulse.setAttribute("cx", x);
    pulse.setAttribute("cy", y);
    pulse.classList.add("pulse");
  }

  function renderAlloc(){
    aiVal.textContent = alloc.ai;
    humVal.textContent = alloc.hum;
    allyVal.textContent = alloc.ally;
    convVal.textContent = alloc.conv;

    const remaining = 100 - sumAlloc();
    remainEl.textContent = remaining;
    tokenFill.style.width = `${clamp(100 - remaining)}%`;

    const ready = (remaining === 0 && started && !locked);
    setReady(ready);
    commitBtn.classList.toggle("btnDisabled", !ready);
  }

  function renderKPI(){
    detN.textContent = kpi.det.toFixed(0);
    warnN.textContent = kpi.warn.toFixed(0);
    resN.textContent = kpi.res.toFixed(0);
    escN.textContent = kpi.esc.toFixed(0);
    repN.textContent = kpi.rep.toFixed(0);

    detFill.style.width = `${clamp(kpi.det)}%`;
    warnFill.style.width = `${clamp(kpi.warn)}%`;
    resFill.style.width = `${clamp(kpi.res)}%`;
    escFill.style.width = `${clamp(kpi.esc)}%`;
    repFill.style.width = `${clamp(kpi.rep)}%`;

    escFill.style.filter = "hue-rotate(140deg)";
  }

  function renderActions(turn){
    actionChips.innerHTML = "";
    turn.actions.forEach(a=>{
      const ch = document.createElement("div");
      ch.className = "chip";
      ch.dataset.act = a.key;
      ch.textContent = a.label;
      ch.title = a.note;
      if(a.key === action) ch.classList.add("active");
      actionChips.appendChild(ch);
    });
    // Default to first action if missing
    if(!turn.actions.some(a=>a.key===action)) action = turn.actions[0].key;
    Array.from(actionChips.querySelectorAll(".chip")).forEach(ch=>{
      ch.classList.toggle("active", ch.dataset.act === action);
    });
  }

  function evaluateTurn(turn, allocSnap, actKey){
    const w = allocSnap.ai/100, h = allocSnap.hum/100, al = allocSnap.ally/100, c = allocSnap.conv/100;
    const s = turn.stress;

    const ambiguityPenalty = (s.amb/100) * Math.max(0, (w - h - 0.12));

    const warnGain = (44*w) + (30*al) + (30*h) - (28*ambiguityPenalty);
    const detGain  = (46*c) + (18*w) + (18*al) + (18*h) - (10*(s.pol/100)*Math.max(0, c-0.45));
    const resGain  = (28*h) + (24*al) + (22*w) + (16*c) - (14*(s.cyber/100)*Math.max(0, 0.20-w));
    const escGain  = (30*c) + (10*w) + (10*al) - (20*h) + (18*(s.threat/100)*Math.max(0, c-0.35));
    const repGain  = (22*al) + (22*h) + (10*w) - (14*c) - (18*(s.pol/100)*Math.max(0, al-0.55));

    const drag = (s.threat + s.cyber + s.amb + s.pol)/420;

    let dDet = (detGain*0.33) - (10*drag);
    let dWarn= (warnGain*0.33) - (10*drag);
    let dRes = (resGain*0.30) - (9*drag);
    let dEsc = (escGain*0.34) + (10*drag);
    let dRep = (repGain*0.30) - (9*drag);

    const am = ACTION[actKey] || {};
    dDet += (am.det||0); dWarn += (am.warn||0); dRes += (am.res||0); dEsc += (am.esc||0); dRep += (am.rep||0);

    kpi.det = clamp(kpi.det + dDet);
    kpi.warn= clamp(kpi.warn+ dWarn);
    kpi.res = clamp(kpi.res + dRes);
    kpi.esc = clamp(kpi.esc + dEsc);
    kpi.rep = clamp(kpi.rep + dRep);

    const bullets = [];
    if(dWarn>=4) bullets.push("Warning improved: earlier decision time.");
    if(dWarn<=-4) bullets.push("Warning degraded: late detection risk increased.");
    if(dDet>=4 && dEsc<=3) bullets.push("Deterrence strengthened without major escalation.");
    if(dDet>=4 && dEsc>6) bullets.push("Deterrence rose, but escalation risk surged.");
    if(dRes>=4) bullets.push("Resilience improved: better shock absorption.");
    if(dRep<=-3) bullets.push("Legitimacy costs increased: overreaction or dependency narratives grew.");
    if(turn.id===7 && ambiguityPenalty>0.06) bullets.push("Deception penalty: sensing without enough intent interpretation under clutter.");
    if(turn.id===6 && allocSnap.ally>60) bullets.push("Alliance backlash: heavy reliance under political pressure reduced legitimacy.");
    if(bullets.length===0) bullets.push("Mixed result: trade-offs largely cancelled.");

    return { bullets, delta:{dDet,dWarn,dRes,dEsc,dRep} };
  }

  function lockAll(lock){
    locked = lock;
    document.querySelectorAll(".btn[data-key]").forEach(b=>{
      b.classList.toggle("btnDisabled", lock);
      b.style.pointerEvents = lock ? "none" : "auto";
    });
    resetBtn.classList.toggle("btnDisabled", lock);
    autoBtn.classList.toggle("btnDisabled", lock);
    commitBtn.classList.toggle("btnDisabled", lock || (100 - sumAlloc() !== 0));
  }

  function loadTurn(){
    const turn = TURNS[idx];
    started = true;

    setMode("Planning", "rgba(34,211,238,.9)");
    renderTimeline();

    turnTitle.textContent = `Turn ${turn.id}: ${turn.tag}`;
    turnSub.textContent = `Allocate to 100, choose one action, commit.`;
    eventTag.textContent = turn.tag;
    eventText.textContent = turn.text;
    turnCount.textContent = `${turn.id} / ${TURNS.length}`;

    renderMeters(turn.stress);
    highlightDomain(turn.domain);
    renderActions(turn);

    aar.style.display = "none";
    summaryWrap.innerHTML = "";
    lockAll(false);
    renderAlloc();
    renderKPI();
  }

  function start(){
    idx = 0;
    alloc = { ai:25, hum:25, ally:25, conv:25 };
    action = "monitor";
    kpi = { det:50, warn:50, res:50, esc:30, rep:60 };
    history.length = 0;
    loadTurn();
  }

  function commit(){
    if(!started || locked) return;
    if(100 - sumAlloc() !== 0) return;

    const turn = TURNS[idx];
    const allocSnap = { ...alloc };

    setMode("Committed", "rgba(34,197,94,.9)");
    lockAll(true);

    const result = evaluateTurn(turn, allocSnap, action);

    const fmt = (x)=> (x>=0?`+${x.toFixed(1)}`:x.toFixed(1));
    const chip = (label, val, invert=false)=>{
      const v = val;
      let cls = "warn";
      if(!invert){
        if(v >= 2.5) cls = "good";
        else if(v <= -2.5) cls = "bad";
      }else{
        if(v <= -2.5) cls = "good";
        else if(v >= 2.5) cls = "bad";
      }
      return `<div class="delta ${cls}">${label} ${fmt(v)}</div>`;
    };
    deltaRow.innerHTML =
      chip("DET", result.delta.dDet) +
      chip("WARN", result.delta.dWarn) +
      chip("RES", result.delta.dRes) +
      chip("ESC", result.delta.dEsc, true) +
      chip("LEG", result.delta.dRep);

    aarList.innerHTML = "";
    result.bullets.slice(0,6).forEach(b=>{
      const li = document.createElement("li");
      li.textContent = b;
      aarList.appendChild(li);
    });

    history.push({
      turn: turn.id, tag: turn.tag, domain: turn.domain,
      action,
      ai: allocSnap.ai, hum: allocSnap.hum, ally: allocSnap.ally, conv: allocSnap.conv,
      det: kpi.det.toFixed(0), warn: kpi.warn.toFixed(0), res: kpi.res.toFixed(0), esc: kpi.esc.toFixed(0), rep: kpi.rep.toFixed(0),
      dDet: result.delta.dDet.toFixed(1), dWarn: result.delta.dWarn.toFixed(1), dRes: result.delta.dRes.toFixed(1), dEsc: result.delta.dEsc.toFixed(1), dRep: result.delta.dRep.toFixed(1)
    });

    renderKPI();
    aar.style.display = "block";
  }

  function next(){
    idx += 1;
    if(idx >= TURNS.length){
      endSim();
      return;
    }
    loadTurn();
  }

  function endSim(){
    setMode("Completed", "rgba(245,158,11,.95)");
    renderTimeline();
    lockAll(true);
    setReady(false);

    const score = (kpi.det*0.20) + (kpi.warn*0.20) + (kpi.res*0.20) + (kpi.rep*0.22) + ((100-kpi.esc)*0.18);
    let verdict = "Mixed posture: gains came with meaningful escalation or legitimacy costs.";
    if(score >= 74) verdict = "High-performing posture: balanced software, alliances, and human interpretation with controlled escalation.";
    if(score <= 56) verdict = "Fragile posture: deterrence, warning, or resilience fell behind crisis tempo.";

    let html = `<div class="eventCard" style="margin-top:12px;">
      <div class="eventTop">
        <div class="badge"><span class="spark"></span><span>Run Summary</span></div>
        <div class="pill"><span class="dot"></span><span class="mono">${score.toFixed(0)} / 100</span></div>
      </div>
      <div class="eventText"><b>${verdict}</b><br><span style="color:rgba(167,178,214,.9);font-size:12px;">
      Download CSV if you want to submit your run. Key turns: 6–8.
      </span></div>
    </div>`;

    html += `<table><thead><tr>
      <th>Turn</th><th>Action</th><th>Alloc (AI/HUM/ALLY/CONV)</th>
      <th>KPIs (DET/WARN/RES/ESC/LEG)</th><th>Δ (DET/WARN/RES/ESC/LEG)</th>
    </tr></thead><tbody>`;

    history.forEach(r=>{
      const hl = (r.turn===6 || r.turn===7 || r.turn===8) ? " class='hl'" : "";
      html += `<tr${hl}>
        <td><b>${r.turn}</b><br><span style="color:rgba(167,178,214,.9);font-size:11px">${r.tag}</span></td>
        <td>${r.action}</td>
        <td>${r.ai}/${r.hum}/${r.ally}/${r.conv}</td>
        <td>${r.det}/${r.warn}/${r.res}/${r.esc}/${r.rep}</td>
        <td>${r.dDet}/${r.dWarn}/${r.dRes}/${r.dEsc}/${r.dRep}</td>
      </tr>`;
    });
    html += `</tbody></table>`;
    summaryWrap.innerHTML = html;
  }

  function toCSV(){
    const cols = ["turn","tag","domain","action","ai","hum","ally","conv","det","warn","res","esc","rep","dDet","dWarn","dRes","dEsc","dRep"];
    const lines = [cols.join(",")];
    history.forEach(r=>{
      const row = cols.map(c => `"${String(r[c]).replace(/"/g,'""')}"`);
      lines.push(row.join(","));
    });
    return lines.join("\n");
  }

  function downloadCSV(){
    const csv = toCSV();
    const blob = new Blob([csv], {type:"text/csv;charset=utf-8;"});
    const url = URL.createObjectURL(blob);
    const a = document.createElement("a");
    a.href = url;
    a.download = "digital_deterrence_clean_run.csv";
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  }

  function applyStep(key, step){
    if(!started || locked) return;
    const remaining = 100 - sumAlloc();
    if(step > 0 && remaining < step) return;
    if(step < 0 && alloc[key] + step < 0) return;
    alloc[key] = clamp(alloc[key] + step);
    renderAlloc();
  }

  function autoBalance(){
    if(!started || locked) return;
    const partial = alloc.ai + alloc.hum + alloc.ally;
    alloc.conv = clamp(100 - partial);
    renderAlloc();
  }

  function resetAlloc(){
    if(!started || locked) return;
    alloc = { ai:25, hum:25, ally:25, conv:25 };
    renderAlloc();
  }

  // Wire steppers
  document.querySelectorAll(".btn[data-key]").forEach(b=>{
    b.addEventListener("click", ()=>{
      applyStep(b.dataset.key, parseInt(b.dataset.step,10));
    });
  });

  actionChips.addEventListener("click", (e)=>{
    const t = e.target.closest(".chip");
    if(!t || locked || !started) return;
    action = t.dataset.act;
    Array.from(actionChips.querySelectorAll(".chip")).forEach(ch=>{
      ch.classList.toggle("active", ch.dataset.act === action);
    });
  });

  startBtn.addEventListener("click", start);
  resetBtn.addEventListener("click", resetAlloc);
  autoBtn.addEventListener("click", autoBalance);
  commitBtn.addEventListener("click", ()=>{ if(!commitBtn.classList.contains("btnDisabled")) commit(); });
  nextBtn.addEventListener("click", next);
  csvBtn.addEventListener("click", downloadCSV);

  // Initial state
  function init(){
    setMode("Standby", "rgba(34,211,238,.9)");
    setReady(false);
    renderTimeline();
    renderAlloc();
    renderKPI();
    // gentle pulse animation
    let phase = 0;
    setInterval(()=>{
      phase += 0.08;
      pulse.style.opacity = String(0.18 + 0.10*Math.sin(phase));
    }, 60);
  }
  init();
</script>
</body>
</html>
