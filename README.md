<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Wahyu Audio - Pro DLMS v12.1 (Natural Master Mastering)</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800;900&family=JetBrains+Mono:wght@400;700&display=swap');

        :root {
            --bg-base: #060709; --bg-panel: #0d1015; --bg-surface: #151921; --bg-input: #080a0d;
            --accent-a: #00e5ff; --accent-b: #ff2a4d; --accent-master: #ffaa00;
            --text-main: #e2e8f0; --text-muted: #64748b;
            --border: #1e293b; --border-light: #334155;
            --rad-lg: 16px; --rad-md: 10px; --rad-sm: 6px;
            --glow-a: 0 0 15px rgba(0, 229, 255, 0.2); --glow-b: 0 0 15px rgba(255, 42, 77, 0.2);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; user-select: none; }
        body { font-family: 'Inter', sans-serif; background-color: var(--bg-base); color: var(--text-main); padding: 15px; -webkit-font-smoothing: antialiased; }
        
        .mono { font-family: 'JetBrains Mono', monospace; }
        .container { max-width: 1400px; margin: 0 auto; display: flex; flex-direction: column; gap: 15px; }
        
        .topbar { display: flex; flex-wrap: wrap; justify-content: space-between; align-items: center; background: var(--bg-panel); border: 1px solid var(--border); padding: 15px 20px; border-radius: var(--rad-lg); box-shadow: 0 10px 30px rgba(0,0,0,0.5); gap: 15px; }
        .brand h1 { font-size: 1.5rem; font-weight: 900; letter-spacing: -0.5px; background: linear-gradient(90deg, #fff, var(--text-muted)); -webkit-background-clip: text; -webkit-text-fill-color: transparent;}
        .brand h1 span { color: var(--accent-master); -webkit-text-fill-color: var(--accent-master);}
        .brand p { font-size: 0.65rem; color: var(--text-muted); font-weight: 800; letter-spacing: 2px; text-transform: uppercase; margin-top: 4px; }
        
        .global-controls { display: flex; gap: 10px; flex: 1; min-width: 300px; }
        select, input[type="file"], input[type="number"] { background: var(--bg-input); color: var(--text-main); border: 1px solid var(--border); padding: 10px 12px; border-radius: var(--rad-sm); outline: none; font-size: 0.8rem; font-weight: 600; width: 100%; transition: 0.2s;}
        select:focus, input:focus { border-color: var(--text-muted); }
        
        button { background: var(--bg-surface); color: var(--text-main); border: 1px solid var(--border); padding: 10px 15px; border-radius: var(--rad-sm); font-size: 0.75rem; font-weight: 800; cursor: pointer; transition: 0.2s; text-transform: uppercase; letter-spacing: 0.5px; }
        button:hover { background: var(--border); }
        button:active { transform: scale(0.96); }
        .btn-green { background: rgba(0, 230, 118, 0.1); color: #00e676; border-color: rgba(0, 230, 118, 0.3); }
        .btn-red { background: rgba(255, 23, 68, 0.1); color: #ff1744; border-color: rgba(255, 23, 68, 0.3); }
        .btn-amber { background: rgba(255, 170, 0, 0.1); color: var(--accent-master); border-color: rgba(255, 170, 0, 0.3); }
        .btn-link.active { background: var(--accent-master); color: #000; box-shadow: 0 0 15px rgba(255, 170, 0, 0.4); }
        .btn-toggle.active { background: #ff1744; color: #fff; border-color: #ff1744; box-shadow: 0 0 15px rgba(255,23,68,0.4); }
        .btn-inv.active { background: var(--accent-a); color: #000; border-color: var(--accent-a); box-shadow: var(--glow-a); }

        .tabs { display: flex; background: var(--bg-panel); padding: 5px; border-radius: var(--rad-md); border: 1px solid var(--border); overflow-x: auto; scrollbar-width: none; }
        .tabs::-webkit-scrollbar { display: none; }
        .tab-btn { flex: 1; min-width: 100px; background: transparent; border: none; color: var(--text-muted); border-radius: var(--rad-sm); padding: 12px; position: relative; }
        .tab-btn.active { background: var(--bg-surface); color: #fff; box-shadow: 0 4px 10px rgba(0,0,0,0.3); }
        .tab-btn[data-target="tab-master"].active { border-bottom: 2px solid var(--accent-master); }
        .tab-btn[data-target="tab-ina"].active { border-bottom: 2px solid var(--accent-a); color: var(--accent-a); }
        .tab-btn[data-target="tab-inb"].active { border-bottom: 2px solid var(--accent-b); color: var(--accent-b); }
        .tab-btn[data-target="tab-out1"].active { border-bottom: 2px solid var(--accent-a); }
        .tab-btn[data-target="tab-out2"].active { border-bottom: 2px solid var(--accent-b); }

        .tab-content { display: none; animation: slideUp 0.3s ease; }
        .tab-content.active { display: flex; flex-direction: column; gap: 15px; }
        @keyframes slideUp { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .panel { background: var(--bg-panel); border: 1px solid var(--border); border-radius: var(--rad-lg); padding: 20px; box-shadow: 0 5px 20px rgba(0,0,0,0.4); }
        .panel-header { display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--border); padding-bottom: 12px; margin-bottom: 15px; }
        .panel-title { font-size: 0.85rem; font-weight: 800; color: var(--text-muted); letter-spacing: 1.5px; text-transform: uppercase; display: flex; align-items: center; gap: 10px; }
        .badge-a { color: var(--accent-a); text-shadow: var(--glow-a); }
        .badge-b { color: var(--accent-b); text-shadow: var(--glow-b); }

        .grid-2 { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 15px; }
        .grid-4 { display: grid; grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); gap: 10px; }

        .ctrl-group { background: var(--bg-surface); border: 1px solid var(--border); border-radius: var(--rad-md); padding: 15px; display: flex; flex-direction: column; gap: 10px; position: relative; }
        .ctrl-label { font-size: 0.65rem; color: var(--text-muted); font-weight: 800; text-transform: uppercase; display: flex; justify-content: space-between; align-items: center;}
        
        .val-display { background: var(--bg-input); border: 1px solid var(--border-light); padding: 4px 8px; border-radius: var(--rad-sm); display: flex; align-items: center; gap: 4px; box-shadow: inset 0 2px 4px rgba(0,0,0,0.5);}
        .val-display input { background: transparent; border: none; padding: 0; color: #fff; font-size: 0.9rem; text-align: right; width: 45px; }
        .val-display span { font-size: 0.6rem; color: var(--text-muted); font-weight: 700; }

        input[type="range"].fader { -webkit-appearance: none; width: 100%; height: 6px; background: #000; border-radius: 3px; outline: none; border: 1px solid #1a1a1a; }
        input[type="range"].fader::-webkit-slider-thumb { -webkit-appearance: none; width: 16px; height: 24px; background: #94a3b8; border-radius: 4px; cursor: pointer; border: 2px solid #fff; box-shadow: 0 0 10px rgba(0,0,0,0.8); transition: 0.1s; }
        input[type="range"].fader::-webkit-slider-thumb:active { background: var(--accent-master); transform: scale(1.1); }
        
        select#playlist-ui { padding: 5px; font-size: 0.75rem; font-family: monospace; border: 1px solid var(--border-light); border-radius: var(--rad-sm); background: #050608; margin-bottom: 10px; height: 80px; }
        select#playlist-ui option { padding: 6px; border-bottom: 1px solid #111; cursor: pointer; }
        select#playlist-ui option:checked { background: rgba(0, 230, 118, 0.2); color: #00e676; }
        
        .time-display { font-family: 'JetBrains Mono', monospace; font-size: 0.75rem; color: var(--accent-master); min-width: 45px; text-align: center; }

        .geq-container { display: flex; gap: 8px; overflow-x: auto; padding: 15px 5px; background: var(--bg-input); border-radius: var(--rad-md); border: 1px solid var(--border); box-shadow: inset 0 5px 15px rgba(0,0,0,0.6); scrollbar-width: thin; scrollbar-color: var(--border) transparent;}
        .geq-band { display: flex; flex-direction: column; align-items: center; min-width: 32px; gap: 10px; }
        .geq-fader-wrapper { height: 160px; display: flex; align-items: center; justify-content: center; position: relative; }
        input[type="range"].geq-fader { -webkit-appearance: slider-vertical; width: 24px; height: 100%; cursor: pointer; }
        .geq-val { font-size: 0.65rem; color: #fff; font-weight: 700; background: #000; padding: 2px 4px; border-radius: 3px; border: 1px solid #222;}
        .geq-freq { font-size: 0.6rem; color: var(--text-muted); font-weight: 700; transform: rotate(-45deg); margin-top: 5px;}

        canvas { width: 100%; background: #030406; border-radius: var(--rad-sm); border: 1px solid #1a1c23; box-shadow: inset 0 0 20px rgba(0,0,0,0.8); margin-bottom: 10px; }
        .cv-rta { height: 220px; } .cv-comp { height: 160px; } .cv-geq { height: 120px; } .cv-peq { height: 180px; }

        .meter-wrap { width: 100%; margin: 10px 0; }
        .meter-bg { height: 8px; background: #000; border-radius: 4px; overflow: hidden; border: 1px solid #1a1a1a; }
        .meter-bar { height: 100%; width: 0%; transition: width 0.05s linear; }
        .m-master { background: linear-gradient(90deg, #00e676 60%, #ffea00 85%, #ff1744 100%); }
        .m-a { background: linear-gradient(90deg, var(--accent-a) 70%, #ffea00 90%, #ff1744 100%); }
        .m-b { background: linear-gradient(90deg, var(--accent-b) 70%, #ffea00 90%, #ff1744 100%); }
        .meter-scale { display: flex; justify-content: space-between; font-size: 0.55rem; color: var(--text-muted); margin-top: 4px; font-weight: bold; }

        .routing-matrix { display: flex; flex-direction: column; gap: 10px; background: #050608; padding: 15px; border-radius: var(--rad-md); border: 1px dashed var(--border-light); }
        .routing-select { font-size: 1rem; font-weight: 900; text-align: center; border: 2px solid var(--accent-master); color: var(--accent-master); background: rgba(255, 170, 0, 0.05); }

        .calc-panel { background: rgba(0, 229, 255, 0.05); border: 1px solid rgba(0, 229, 255, 0.2); padding: 15px; border-radius: var(--rad-md); display: flex; flex-direction: column; gap: 10px;}

        @media (min-width: 1024px) {
            #dsp-container { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }
            .full-span { grid-column: 1 / -1; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="topbar">
        <div class="brand">
            <h1>WU <span>AUDIO</span> DLMS</h1>
            <p>Pro DSP Matrix v12.1 • Mastering Grade</p>
        </div>
        <div class="global-controls">
            <select id="preset-sel"><option value="">-- Presets --</option></select>
            <button class="btn-green" onclick="savePreset()">SAVE</button>
            <button class="btn-red" onclick="deletePreset()">DEL</button>
            <button id="btn-link" class="btn-link" style="width: 140px;">🔗 LINK A/B</button>
        </div>
    </div>

    <div class="tabs">
        <button class="tab-btn active" data-target="tab-master">MASTER / RTA</button>
        <button class="tab-btn" data-target="tab-ina">INPUT A (GEQ)</button>
        <button class="tab-btn" data-target="tab-inb">INPUT B (GEQ)</button>
        <button class="tab-btn" data-target="tab-out1">OUTPUT 1 (DSP)</button>
        <button class="tab-btn" data-target="tab-out2">OUTPUT 2 (DSP)</button>
    </div>

    <div id="tab-master" class="tab-content active full-span">
        <div class="grid-2">
            <div class="panel">
                <div class="panel-header"><div class="panel-title">MAIN SOURCE & PLAYLIST</div></div>
                
                <input type="file" id="audio-file" accept="audio/*" multiple style="margin-bottom: 5px;" title="Pilih beberapa lagu sekaligus">
                <select id="playlist-ui" size="4"></select>
                
                <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 15px; padding: 10px; background: #000; border-radius: var(--rad-sm); border: 1px solid #1a1a1a;">
                    <span class="time-display" id="time-curr">0:00</span>
                    <input type="range" class="fader" id="seek-bar" min="0" max="100" step="0.1" value="0" style="margin: 0;">
                    <span class="time-display" id="time-dur" style="color: var(--text-muted);">0:00</span>
                </div>

                <div style="display: flex; gap: 10px; margin-bottom: 20px;">
                    <button id="btn-play" class="btn-green" style="flex: 2;">▶ PLAY</button>
                    <button id="btn-stop" style="flex: 1;">⏹ STOP</button>
                </div>
                
                <div class="grid-2" style="gap: 10px;">
                    <button id="btn-mic" class="btn-amber">🎙️ LIVE LINE-IN</button>
                    <button id="btn-pink" class="btn-red">〰️ PINK NOISE</button>
                </div>
                
                <div style="margin-top: 25px;">
                    <div class="ctrl-label">MASTER FADER <span class="mono" id="lbl-master-vol">80%</span></div>
                    <input type="range" class="fader" id="master-vol" min="0" max="150" value="80" style="margin-top:10px;">
                    <div class="meter-wrap">
                        <div class="meter-bg"><div class="meter-bar m-master" id="vu-master"></div></div>
                        <div class="meter-scale"><span>-60</span><span>-40</span><span>-20</span><span>-12</span><span>-6</span><span>0</span><span>+3</span></div>
                    </div>
                </div>
            </div>

            <div class="panel calc-panel">
                <div class="panel-header" style="border-color: rgba(0, 229, 255, 0.2);"><div class="panel-title" style="color:var(--accent-a);">DELAY ALIGN CALCULATOR</div></div>
                <div class="grid-2">
                    <div><span class="ctrl-label">Jarak Main (M)</span><input type="number" id="calc-main" value="0" step="0.1" style="background:#000;"></div>
                    <div><span class="ctrl-label">Jarak Sub (M)</span><input type="number" id="calc-sub" value="0" step="0.1" style="background:#000;"></div>
                </div>
                <div style="display:flex; justify-content:space-between; align-items:center; background:#000; padding:15px; border-radius:var(--rad-sm); margin-top:10px;">
                    <div><span style="font-size:0.65rem; color:var(--text-muted);">KOMPENSASI WAKTU</span><br><strong class="mono" id="calc-res" style="color:var(--accent-a); font-size:1.2rem;">0.00 ms</strong></div>
                    <div style="display:flex; flex-direction:column; gap:5px;">
                        <button onclick="applyCalcDelay('out1')" style="font-size:0.6rem; padding:6px;">SET OUT 1</button>
                        <button onclick="applyCalcDelay('out2')" style="font-size:0.6rem; padding:6px;">SET OUT 2</button>
                    </div>
                </div>
            </div>
        </div>

        <div class="panel">
            <div class="panel-header">
                <div class="panel-title">PRECISION REAL-TIME ANALYZER (RTA) <span style="font-size: 0.6rem; color:var(--text-muted);">16384-Point FFT</span></div>
            </div>
            <canvas id="rta-canvas" class="cv-rta"></canvas>
        </div>
    </div>

    <div id="dsp-container">
        <div id="tab-ina" class="tab-content panel full-span">
            <div class="panel-header">
                <div class="panel-title badge-a">INPUT A (LEFT)</div>
                <div class="global-controls" style="flex:none; min-width:auto;"><button id="pol-ina" class="btn-inv">Ø INV</button><button id="mute-ina" class="btn-toggle">MUTE</button></div>
            </div>
            
            <div class="grid-2">
                <div class="ctrl-group">
                    <div class="ctrl-label">Input Gain (Pre-EQ)</div>
                    <input type="range" id="gain-ina" class="fader" min="-24" max="12" step="0.1" value="0">
                    <div class="meter-wrap"><div class="meter-bg"><div class="meter-bar m-a" id="vu-ina"></div></div><div class="meter-scale"><span>-60</span><span>0</span><span>+3</span></div></div>
                </div>
            </div>

            <div class="grid-2" style="margin-top: 10px;">
                <div><div class="panel-title" style="margin-bottom:10px;">DYNAMIC COMPRESSOR CURVE</div><canvas id="comp-cv-ina" class="cv-comp"></canvas></div>
                <div class="grid-2" style="gap:10px; align-content:start;">
                    <div class="ctrl-group"><div class="ctrl-label">Threshold <div class="val-display"><input type="number" id="num-comp-thr-ina" value="-12"><span>dB</span></div></div><input type="range" class="fader" id="comp-thr-ina" min="-60" max="0" step="1" value="-12"></div>
                    <div class="ctrl-group"><div class="ctrl-label">Ratio <div class="val-display"><input type="number" id="num-comp-rat-ina" value="4"><span>:1</span></div></div><input type="range" class="fader" id="comp-rat-ina" min="1" max="20" step="0.1" value="4"></div>
                    <div class="ctrl-group"><div class="ctrl-label">Attack <div class="val-display"><input type="number" id="num-comp-att-ina" value="10"><span>ms</span></div></div><input type="range" class="fader" id="comp-att-ina" min="0" max="200" step="1" value="10"></div>
                    <div class="ctrl-group"><div class="ctrl-label">Release <div class="val-display"><input type="number" id="num-comp-rel-ina" value="250"><span>ms</span></div></div><input type="range" class="fader" id="comp-rel-ina" min="10" max="1000" step="10" value="250"></div>
                </div>
            </div>

            <div style="margin-top: 15px;">
                <div class="panel-header"><div class="panel-title">31-BAND GRAPHIC EQ</div></div>
                <canvas id="geq-cv-ina" class="cv-geq"></canvas>
                <div class="geq-container" id="geq-ui-ina"></div>
            </div>
        </div>

        <div id="tab-inb" class="tab-content panel full-span">
            <div class="panel-header">
                <div class="panel-title badge-b">INPUT B (RIGHT)</div>
                <div class="global-controls" style="flex:none; min-width:auto;"><button id="pol-inb" class="btn-inv">Ø INV</button><button id="mute-inb" class="btn-toggle">MUTE</button></div>
            </div>
            
            <div class="grid-2">
                <div class="ctrl-group">
                    <div class="ctrl-label">Input Gain (Pre-EQ)</div>
                    <input type="range" id="gain-inb" class="fader" min="-24" max="12" step="0.1" value="0">
                    <div class="meter-wrap"><div class="meter-bg"><div class="meter-bar m-b" id="vu-inb"></div></div><div class="meter-scale"><span>-60</span><span>0</span><span>+3</span></div></div>
                </div>
            </div>

            <div class="grid-2" style="margin-top: 10px;">
                <div><div class="panel-title" style="margin-bottom:10px;">DYNAMIC COMPRESSOR CURVE</div><canvas id="comp-cv-inb" class="cv-comp"></canvas></div>
                <div class="grid-2" style="gap:10px; align-content:start;">
                    <div class="ctrl-group"><div class="ctrl-label">Threshold <div class="val-display"><input type="number" id="num-comp-thr-inb" value="-12"><span>dB</span></div></div><input type="range" class="fader" id="comp-thr-inb" min="-60" max="0" step="1" value="-12"></div>
                    <div class="ctrl-group"><div class="ctrl-label">Ratio <div class="val-display"><input type="number" id="num-comp-rat-inb" value="4"><span>:1</span></div></div><input type="range" class="fader" id="comp-rat-inb" min="1" max="20" step="0.1" value="4"></div>
                    <div class="ctrl-group"><div class="ctrl-label">Attack <div class="val-display"><input type="number" id="num-comp-att-inb" value="10"><span>ms</span></div></div><input type="range" class="fader" id="comp-att-inb" min="0" max="200" step="1" value="10"></div>
                    <div class="ctrl-group"><div class="ctrl-label">Release <div class="val-display"><input type="number" id="num-comp-rel-inb" value="250"><span>ms</span></div></div><input type="range" class="fader" id="comp-rel-inb" min="10" max="1000" step="10" value="250"></div>
                </div>
            </div>

            <div style="margin-top: 15px;">
                <div class="panel-header"><div class="panel-title">31-BAND GRAPHIC EQ</div></div>
                <canvas id="geq-cv-inb" class="cv-geq"></canvas>
                <div class="geq-container" id="geq-ui-inb"></div>
            </div>
        </div>

        <div id="tab-out1" class="tab-content panel">
            <div class="panel-header">
                <div class="panel-title badge-a">OUTPUT 1</div>
                <div class="global-controls" style="flex:none; min-width:auto;"><button id="pol-out1" class="btn-inv">Ø INV</button><button id="mute-out1" class="btn-toggle">MUTE</button></div>
            </div>
            
            <div class="routing-matrix">
                <div class="ctrl-label" style="justify-content:center; color:var(--accent-master);">MATRIX ROUTING SELECT</div>
                <select id="route-out1" class="routing-select">
                    <option value="A" selected>SOURCE: INPUT A (LEFT)</option>
                    <option value="B">SOURCE: INPUT B (RIGHT)</option>
                    <option value="SUM">SOURCE: SUM (A + B)</option>
                </select>
            </div>

            <div class="grid-2" style="margin-top:15px; grid-template-columns: 1fr;">
                <div class="ctrl-group">
                    <div class="ctrl-label">Output Gain</div>
                    <input type="range" id="gain-out1" class="fader" min="-24" max="12" step="0.1" value="0">
                    <div class="meter-wrap"><div class="meter-bg"><div class="meter-bar m-a" id="vu-out1"></div></div></div>
                </div>
            </div>

            <div class="grid-4" style="margin-top:15px;">
                <div class="ctrl-group"><div class="ctrl-label">Limiter <div class="val-display"><input type="number" id="num-lim-out1" value="-2"></div></div><input type="range" class="fader" id="lim-out1" min="-40" max="0" step="1" value="-2"></div>
                <div class="ctrl-group"><div class="ctrl-label">Delay <div class="val-display"><input type="number" id="num-dly-out1" value="0"></div></div><input type="range" class="fader" id="dly-out1" min="0" max="100" step="0.1" value="0"></div>
                <div class="ctrl-group"><div class="ctrl-label">Phase(APF) <div class="val-display"><input type="number" id="num-apf-out1" value="20000"></div></div><input type="range" class="fader" id="apf-out1" min="20" max="20000" step="1" value="20000"></div>
                <div class="ctrl-group"><div class="ctrl-label">FIR Gen <select id="fir-out1" style="padding:2px; margin-top:5px;"><option value="off">OFF</option><option value="flat">FLAT</option></select></div></div>
            </div>

            <div class="panel-header" style="margin-top:20px;"><div class="panel-title">CROSSOVER & PEQ (5-BAND)</div></div>
            <canvas id="peq-cv-out1" class="cv-peq"></canvas>
            
            <div class="grid-2">
                <div class="ctrl-group" style="border-color: rgba(0, 229, 255, 0.3);">
                    <div class="ctrl-label">HPF (Low Cut) <select id="hpf-type-out1" style="width:70px; padding:2px;"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select></div>
                    <input type="range" id="hpf-out1" class="fader" min="20" max="16000" step="1" value="40">
                    <div class="val-display" style="justify-content:center;"><input type="number" id="num-hpf-out1" value="40"><span>Hz</span></div>
                </div>
                <div class="ctrl-group" style="border-color: rgba(0, 229, 255, 0.3);">
                    <div class="ctrl-label">LPF (High Cut) <select id="lpf-type-out1" style="width:70px; padding:2px;"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select></div>
                    <input type="range" id="lpf-out1" class="fader" min="20" max="16000" step="10" value="16000">
                    <div class="val-display" style="justify-content:center;"><input type="number" id="num-lpf-out1" value="16000"><span>Hz</span></div>
                </div>
            </div>
            <div id="peq-ui-out1" style="display:flex; flex-direction:column; gap:10px; margin-top:10px;"></div>
        </div>

        <div id="tab-out2" class="tab-content panel">
            <div class="panel-header">
                <div class="panel-title badge-b">OUTPUT 2</div>
                <div class="global-controls" style="flex:none; min-width:auto;"><button id="pol-out2" class="btn-inv">Ø INV</button><button id="mute-out2" class="btn-toggle">MUTE</button></div>
            </div>
            
            <div class="routing-matrix">
                <div class="ctrl-label" style="justify-content:center; color:var(--accent-master);">MATRIX ROUTING SELECT</div>
                <select id="route-out2" class="routing-select">
                    <option value="A">SOURCE: INPUT A (LEFT)</option>
                    <option value="B" selected>SOURCE: INPUT B (RIGHT)</option>
                    <option value="SUM">SOURCE: SUM (A + B)</option>
                </select>
            </div>

            <div class="grid-2" style="margin-top:15px; grid-template-columns: 1fr;">
                <div class="ctrl-group">
                    <div class="ctrl-label">Output Gain</div>
                    <input type="range" id="gain-out2" class="fader" min="-24" max="12" step="0.1" value="0">
                    <div class="meter-wrap"><div class="meter-bg"><div class="meter-bar m-b" id="vu-out2"></div></div></div>
                </div>
            </div>

            <div class="grid-4" style="margin-top:15px;">
                <div class="ctrl-group"><div class="ctrl-label">Limiter <div class="val-display"><input type="number" id="num-lim-out2" value="-2"></div></div><input type="range" class="fader" id="lim-out2" min="-40" max="0" step="1" value="-2"></div>
                <div class="ctrl-group"><div class="ctrl-label">Delay <div class="val-display"><input type="number" id="num-dly-out2" value="0"></div></div><input type="range" class="fader" id="dly-out2" min="0" max="100" step="0.1" value="0"></div>
                <div class="ctrl-group"><div class="ctrl-label">Phase(APF) <div class="val-display"><input type="number" id="num-apf-out2" value="20000"></div></div><input type="range" class="fader" id="apf-out2" min="20" max="20000" step="1" value="20000"></div>
                <div class="ctrl-group"><div class="ctrl-label">FIR Gen <select id="fir-out2" style="padding:2px; margin-top:5px;"><option value="off">OFF</option><option value="flat">FLAT</option></select></div></div>
            </div>

            <div class="panel-header" style="margin-top:20px;"><div class="panel-title">CROSSOVER & PEQ (5-BAND)</div></div>
            <canvas id="peq-cv-out2" class="cv-peq"></canvas>
            
            <div class="grid-2">
                <div class="ctrl-group" style="border-color: rgba(255, 42, 77, 0.3);">
                    <div class="ctrl-label">HPF (Low Cut) <select id="hpf-type-out2" style="width:70px; padding:2px;"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select></div>
                    <input type="range" id="hpf-out2" class="fader" min="20" max="16000" step="1" value="40">
                    <div class="val-display" style="justify-content:center;"><input type="number" id="num-hpf-out2" value="40"><span>Hz</span></div>
                </div>
                <div class="ctrl-group" style="border-color: rgba(255, 42, 77, 0.3);">
                    <div class="ctrl-label">LPF (High Cut) <select id="lpf-type-out2" style="width:70px; padding:2px;"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select></div>
                    <input type="range" id="lpf-out2" class="fader" min="20" max="16000" step="10" value="16000">
                    <div class="val-display" style="justify-content:center;"><input type="number" id="num-hpf-out2" value="16000"><span>Hz</span></div>
                </div>
            </div>
            <div id="peq-ui-out2" style="display:flex; flex-direction:column; gap:10px; margin-top:10px;"></div>
        </div>

    </div>
</div>

<audio id="core-audio" crossorigin="anonymous"></audio>

<script>
    document.querySelectorAll('.tab-btn').forEach(tab => {
        tab.addEventListener('click', () => {
            document.querySelectorAll('.tab-btn, .tab-content').forEach(el => el.classList.remove('active'));
            tab.classList.add('active'); 
            document.getElementById(tab.dataset.target).classList.add('active');
            requestAnimationFrame(drawAllCanvas);
        });
    });

    let isLinked = false;
    document.getElementById('btn-link').addEventListener('click', function() {
        isLinked = !isLinked;
        this.classList.toggle('active');
        this.innerText = isLinked ? "🔗 LINK A/B (ON)" : "🔗 LINK A/B";
    });

    const geqFreqs = [20, 25, 31.5, 40, 50, 63, 80, 100, 125, 160, 200, 250, 315, 400, 500, 630, 800, 1000, 1250, 1600, 2000, 2500, 3150, 4000, 5000, 6300, 8000, 10000, 12500, 16000, 20000];
    ['ina', 'inb'].forEach(ch => {
        let html = '';
        geqFreqs.forEach((f, i) => {
            let label = f >= 1000 ? (f/1000) + 'k' : f;
            html += `<div class="geq-band">
                <span class="geq-val mono" id="v-geq-${ch}-${i}">0.0</span>
                <div class="geq-fader-wrapper"><input type="range" class="geq-fader" id="geq-${ch}-${i}" min="-15" max="15" step="0.5" value="0"></div>
                <span class="geq-freq">${label}</span>
            </div>`;
        });
        document.getElementById(`geq-ui-${ch}`).innerHTML = html;
    });

    ['out1', 'out2'].forEach(ch => {
        let html = '';
        [65, 250, 1000, 4000, 8000].forEach((f, i) => {
            html += `<div class="ctrl-group grid-4" style="gap:5px; padding:10px;">
                <div style="font-size:0.7rem; font-weight:800; color:var(--text-muted); align-self:center;">BAND ${i+1}</div>
                <div><div class="val-display" style="margin-bottom:5px;"><input type="number" id="num-peq-f-${ch}-${i}" value="${f}"><span>Hz</span></div><input type="range" class="fader" id="peq-f-${ch}-${i}" min="20" max="16000" step="1" value="${f}"></div>
                <div><div class="val-display" style="margin-bottom:5px;"><input type="number" id="num-peq-g-${ch}-${i}" value="0"><span>dB</span></div><input type="range" class="fader" id="peq-g-${ch}-${i}" min="-18" max="18" step="0.1" value="0"></div>
                <div><div class="val-display" style="margin-bottom:5px;"><input type="number" id="num-peq-q-${ch}-${i}" value="1.5"><span>Q</span></div><input type="range" class="fader" id="peq-q-${ch}-${i}" min="0.1" max="10" step="0.1" value="1.5"></div>
            </div>`;
        });
        document.getElementById(`peq-ui-${ch}`).innerHTML = html;
    });

    function calcDelay() {
        let m = parseFloat(document.getElementById('calc-main').value) || 0;
        let s = parseFloat(document.getElementById('calc-sub').value) || 0;
        let ms = (Math.abs(m - s) / 343) * 1000;
        document.getElementById('calc-res').innerText = ms.toFixed(2) + ' ms';
        return ms;
    }
    document.getElementById('calc-main').addEventListener('input', calcDelay);
    document.getElementById('calc-sub').addEventListener('input', calcDelay);
    function applyCalcDelay(ch) {
        let el = document.getElementById(`dly-${ch}`);
        if(el) { el.value = calcDelay().toFixed(1); el.dispatchEvent(new Event('input')); }
    }

    let audioCtx, srcFile, srcMic, streamMic, pinkSrc, oscGain;
    let masterGain, masterLimiter, splitter, merger;
    let anaM, anaA, anaB, anaO1, anaO2, anaCompInA, anaCompInB;
    let freqDataM; 
    
    const CH = {
        ina: { gain: null, pol: 1, geq: [], comp: null },
        inb: { gain: null, pol: 1, geq: [], comp: null },
        out1: { gain: null, pol: 1, hpf: [], lpf: [], peq: [], apf: null, delay: null, limit: null },
        out2: { gain: null, pol: 1, hpf: [], lpf: [], peq: [], apf: null, delay: null, limit: null }
    };
    const routeGains = { a_1: null, b_1: null, a_2: null, b_2: null };
    const outMix = { out1: null, out2: null };

    function createPinkNoise() {
        let bs = 2 * audioCtx.sampleRate; let buf = audioCtx.createBuffer(1, bs, audioCtx.sampleRate);
        let out = buf.getChannelData(0); let b0=0,b1=0,b2=0,b3=0,b4=0,b5=0,b6=0;
        for(let i=0; i<bs; i++) {
            let w = Math.random()*2-1;
            b0 = 0.99886*b0 + w*0.0555179; b1 = 0.99332*b1 + w*0.0750759; b2 = 0.96900*b2 + w*0.1538520; b3 = 0.86650*b3 + w*0.3104856; b4 = 0.55000*b4 + w*0.5329522; b5 = -0.7616*b5 - w*0.0168980;
            out[i] = (b0+b1+b2+b3+b4+b5+b6+w*0.5362)*0.05; b6 = w*0.115926;
        }
        return buf;
    }

    function initAudio() {
        if(audioCtx) return;
        audioCtx = new (window.AudioContext || window.webkitAudioContext)({sampleRate: 48000});
        
        srcFile = audioCtx.createMediaElementSource(document.getElementById('core-audio'));
        oscGain = audioCtx.createGain(); oscGain.gain.value = 0;
        
        splitter = audioCtx.createChannelSplitter(2); merger = audioCtx.createChannelMerger(2);
        
        // Master Mastering Chain (Memberikan kehalusan & ketebalan ala YouTube)
        masterGain = audioCtx.createGain(); masterGain.gain.value = 0.85;
        masterLimiter = audioCtx.createDynamicsCompressor();
        masterLimiter.threshold.value = -1.0; // Mencegah 0dB clipping
        masterLimiter.knee.value = 0.0;
        masterLimiter.ratio.value = 20.0;    // Brickwall limit
        masterLimiter.attack.value = 0.001;  // 1ms fast attack
        masterLimiter.release.value = 0.050;

        srcFile.connect(splitter); oscGain.connect(splitter);

        anaM = audioCtx.createAnalyser(); anaM.fftSize = 16384; anaM.smoothingTimeConstant = 0.85;
        freqDataM = new Uint8Array(anaM.frequencyBinCount);

        anaA = audioCtx.createAnalyser(); anaB = audioCtx.createAnalyser(); 
        anaO1 = audioCtx.createAnalyser(); anaO2 = audioCtx.createAnalyser();
        [anaA, anaB, anaO1, anaO2].forEach(a => { a.fftSize = 2048; a.smoothingTimeConstant = 0.85; });

        anaCompInA = audioCtx.createAnalyser(); anaCompInA.fftSize = 512;
        anaCompInB = audioCtx.createAnalyser(); anaCompInB.fftSize = 512;

        ['ina', 'inb'].forEach((k, idx) => {
            CH[k].gain = audioCtx.createGain(); CH[k].comp = audioCtx.createDynamicsCompressor();
            splitter.connect(CH[k].gain, idx); let last = CH[k].gain; last.connect(k==='ina'?anaA:anaB);
            for(let i=0; i<31; i++) {
                let f = audioCtx.createBiquadFilter(); f.type = 'peaking'; f.frequency.value = geqFreqs[i]; f.Q.value = 1.4;
                last.connect(f); last = f; CH[k].geq.push(f);
            }
            last.connect(k === 'ina' ? anaCompInA : anaCompInB); last.connect(CH[k].comp);
        });

        ['out1', 'out2'].forEach(k => outMix[k] = audioCtx.createGain());
        routeGains.a_1 = audioCtx.createGain(); routeGains.b_1 = audioCtx.createGain();
        routeGains.a_2 = audioCtx.createGain(); routeGains.b_2 = audioCtx.createGain();
        CH.ina.comp.connect(routeGains.a_1); CH.ina.comp.connect(routeGains.a_2);
        CH.inb.comp.connect(routeGains.b_1); CH.inb.comp.connect(routeGains.b_2);
        routeGains.a_1.connect(outMix.out1); routeGains.b_1.connect(outMix.out1);
        routeGains.a_2.connect(outMix.out2); routeGains.b_2.connect(outMix.out2);

        ['out1', 'out2'].forEach((k, idx) => {
            let last = outMix[k];
            for(let i=0; i<4; i++) { let f = audioCtx.createBiquadFilter(); f.type = 'highpass'; last.connect(f); last = f; CH[k].hpf.push(f); }
            for(let i=0; i<4; i++) { let f = audioCtx.createBiquadFilter(); f.type = 'lowpass'; last.connect(f); last = f; CH[k].lpf.push(f); }
            for(let i=0; i<5; i++) { let f = audioCtx.createBiquadFilter(); f.type = 'peaking'; last.connect(f); last = f; CH[k].peq.push(f); }
            
            CH[k].apf = audioCtx.createBiquadFilter(); CH[k].apf.type = 'allpass'; CH[k].apf.frequency.value = 20000;
            CH[k].delay = audioCtx.createDelay(1.0);
            CH[k].limit = audioCtx.createDynamicsCompressor(); CH[k].limit.ratio.value = 20; CH[k].limit.attack.value = 0.002;
            CH[k].gain = audioCtx.createGain();

            last.connect(CH[k].apf); CH[k].apf.connect(CH[k].delay); CH[k].delay.connect(CH[k].limit); CH[k].limit.connect(CH[k].gain);
            CH[k].gain.connect(k==='out1'?anaO1:anaO2); CH[k].gain.connect(merger, 0, idx);
        });

        // Rantai Master Akhir: Merger -> Master Gain -> Master Limiter -> Destination
        merger.connect(masterGain);
        masterGain.connect(masterLimiter);
        masterLimiter.connect(anaM);
        masterLimiter.connect(audioCtx.destination);

        attachParams(); updateMatrix(); 
        document.querySelectorAll('input[type="range"]').forEach(e => e.dispatchEvent(new Event('input')));
        requestAnimationFrame(renderRealtime);
    }

    function setXover(nodes, type, freq, isHpf) {
        let f = parseFloat(freq); let b = isHpf ? 10 : 24000;
        for(let i=0; i<4; i++) { nodes[i].frequency.value = b; nodes[i].Q.value = 0.707; }
        if (type === 'BW12') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.707; } 
        else if (type === 'BW24') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.541; nodes[1].frequency.value = f; nodes[1].Q.value = 1.306; }
        else if (type === 'LR24') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.707; nodes[1].frequency.value = f; nodes[1].Q.value = 0.707; }
        else if (type === 'LR48') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.541; nodes[1].frequency.value = f; nodes[1].Q.value = 1.306; nodes[2].frequency.value = f; nodes[2].Q.value = 0.541; nodes[3].frequency.value = f; nodes[3].Q.value = 1.306; }
    }

    function updateMatrix() {
        if(!audioCtx) return;
        let r1 = document.getElementById('route-out1').value;
        routeGains.a_1.gain.value = (r1 === 'A' || r1 === 'SUM') ? 1 : 0; routeGains.b_1.gain.value = (r1 === 'B' || r1 === 'SUM') ? 1 : 0;
        let r2 = document.getElementById('route-out2').value;
        routeGains.a_2.gain.value = (r2 === 'A' || r2 === 'SUM') ? 1 : 0; routeGains.b_2.gain.value = (r2 === 'B' || r2 === 'SUM') ? 1 : 0;
    }
    document.getElementById('route-out1').addEventListener('change', updateMatrix);
    document.getElementById('route-out2').addEventListener('change', updateMatrix);

    const syncVal = (id, val) => {
        if(!isLinked) return;
        let targetId = id.includes('-ina') ? id.replace('-ina','-inb') : id.includes('-inb') ? id.replace('-inb','-ina') : 
                       id.includes('-out1') ? id.replace('-out1','-out2') : id.includes('-out2') ? id.replace('-out2','-out1') : null;
        if(targetId) { let el = document.getElementById(targetId); if(el && el.value != val) { el.value = val; el.dispatchEvent(new Event('input', {bubbles:false})); } }
    };

    const bindCtrl = (id, act, cb) => {
        let rng = document.getElementById(id), num = document.getElementById('num-'+id); if(!rng) return;
        let update = (v, trust) => { v = parseFloat(v); if(num) num.value = v; if(audioCtx) act(v); if(cb) cb(); if(trust) syncVal(id, v); };
        rng.addEventListener('input', e => update(e.target.value, e.isTrusted));
        if(num) num.addEventListener('change', e => { rng.value = e.target.value; update(e.target.value, e.isTrusted); });
    };

    function attachParams() {
        bindCtrl('master-vol', v => masterGain.gain.value = v/100, () => document.getElementById('lbl-master-vol').innerText = Math.round(masterGain.gain.value*100)+'%');
        const toggleBtn = (id, chObj, isPol) => {
            document.getElementById(id).addEventListener('click', function(e) {
                this.classList.toggle('active'); let state = this.classList.contains('active');
                if(isPol) chObj.pol = state ? -1 : 1;
                let volEl = document.getElementById(id.replace('pol-','gain-').replace('mute-','gain-'));
                if(audioCtx) chObj.gain.gain.value = document.getElementById(id.replace('pol-','mute-')).classList.contains('active') ? 0 : Math.pow(10, (volEl?volEl.value:0)/20) * chObj.pol;
                if(isLinked && e.isTrusted) {
                    let tid = id.includes('ina')?id.replace('ina','inb'):id.includes('inb')?id.replace('inb','ina'):id.includes('out1')?id.replace('out1','out2'):id.replace('out2','out1');
                    let tel = document.getElementById(tid); if(tel.classList.contains('active') !== state) tel.click();
                }
            });
        };

        ['ina', 'inb'].forEach(ch => {
            toggleBtn(`mute-${ch}`, CH[ch], false); toggleBtn(`pol-${ch}`, CH[ch], true);
            bindCtrl(`gain-${ch}`, v => CH[ch].gain.gain.value = document.getElementById(`mute-${ch}`).classList.contains('active') ? 0 : Math.pow(10, v/20)*CH[ch].pol);
            bindCtrl(`comp-thr-${ch}`, v => CH[ch].comp.threshold.value = v, () => drawComp(ch));
            bindCtrl(`comp-rat-${ch}`, v => CH[ch].comp.ratio.value = v, () => drawComp(ch));
            bindCtrl(`comp-att-${ch}`, v => CH[ch].comp.attack.value = v/1000);
            bindCtrl(`comp-rel-${ch}`, v => CH[ch].comp.release.value = v/1000);
            
            for(let i=0; i<31; i++) {
                let rng = document.getElementById(`geq-${ch}-${i}`); let lbl = document.getElementById(`v-geq-${ch}-${i}`);
                rng.addEventListener('input', e => {
                    let v = parseFloat(e.target.value); lbl.innerText = v>0?'+'+v.toFixed(1):v.toFixed(1);
                    if(audioCtx) CH[ch].geq[i].gain.value = v; drawGEQ(ch);
                    if(isLinked && e.isTrusted) { let tid = ch==='ina'?`geq-inb-${i}`:`geq-ina-${i}`; let tel = document.getElementById(tid); if(tel.value != v) { tel.value = v; tel.dispatchEvent(new Event('input',{bubbles:false})); } }
                });
            }
        });

        ['out1', 'out2'].forEach(ch => {
            toggleBtn(`mute-${ch}`, CH[ch], false); toggleBtn(`pol-${ch}`, CH[ch], true);
            bindCtrl(`gain-${ch}`, v => CH[ch].gain.gain.value = document.getElementById(`mute-${ch}`).classList.contains('active') ? 0 : Math.pow(10, v/20)*CH[ch].pol);
            bindCtrl(`lim-${ch}`, v => CH[ch].limit.threshold.value = v);
            bindCtrl(`dly-${ch}`, v => CH[ch].delay.delayTime.value = v/1000);
            bindCtrl(`apf-${ch}`, v => CH[ch].apf.frequency.value = v);
            let applyXv = () => { if(!audioCtx) return; setXover(CH[ch].hpf, document.getElementById(`hpf-type-${ch}`).value, document.getElementById(`hpf-${ch}`).value, true); setXover(CH[ch].lpf, document.getElementById(`lpf-type-${ch}`).value, document.getElementById(`lpf-${ch}`).value, false); drawPEQ(ch); };
            bindCtrl(`hpf-${ch}`, v => applyXv()); bindCtrl(`lpf-${ch}`, v => applyXv());
            document.getElementById(`hpf-type-${ch}`).addEventListener('change', e => { applyXv(); if(isLinked && e.isTrusted) syncVal(`hpf-type-${ch}`, e.target.value); });
            document.getElementById(`lpf-type-${ch}`).addEventListener('change', e => { applyXv(); if(isLinked && e.isTrusted) syncVal(`lpf-type-${ch}`, e.target.value); });

            for(let i=0; i<5; i++) {
                bindCtrl(`peq-f-${ch}-${i}`, v => { CH[ch].peq[i].frequency.value = v; drawPEQ(ch); });
                bindCtrl(`peq-g-${ch}-${i}`, v => { CH[ch].peq[i].gain.value = v; drawPEQ(ch); });
                bindCtrl(`peq-q-${ch}-${i}`, v => { CH[ch].peq[i].Q.value = v; drawPEQ(ch); });
            }
        });
    }

    const MAX_P = 100; let freqs = new Float32Array(MAX_P), mag = new Float32Array(MAX_P), phs = new Float32Array(MAX_P), tMag = new Float32Array(MAX_P);
    for (let i = 0; i < MAX_P; i++) freqs[i] = 20 * Math.pow(1000, i / MAX_P);

    function drawGrid(ctx, w, h) {
        ctx.fillStyle = '#030406'; ctx.fillRect(0,0,w,h); ctx.strokeStyle = '#1a1c23'; ctx.font = '9px monospace'; ctx.fillStyle='#555';
        [50,100,500,1000,5000,10000].forEach(f => { let x = (Math.log10(f/20)/3)*w; ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,h); ctx.stroke(); ctx.fillText(f>=1000?f/1000+'k':f, x+2, 10); });
        ctx.beginPath(); ctx.moveTo(0, h/2); ctx.lineTo(w, h/2); ctx.strokeStyle='#222'; ctx.stroke();
    }

    function drawGEQ(ch) {
        let c = document.getElementById(`geq-cv-${ch}`); if(!c || !audioCtx) return;
        let ctx = c.getContext('2d'), w = c.width = c.clientWidth, h = c.height = 120; drawGrid(ctx, w, h);
        tMag.fill(1.0); for(let j=0; j<31; j++) { CH[ch].geq[j].getFrequencyResponse(freqs, mag, phs); for(let i=0; i<MAX_P; i++) tMag[i]*=mag[i]; }
        ctx.beginPath(); ctx.strokeStyle = ch==='ina'?'#00e5ff':'#ff2a4d'; ctx.lineWidth = 2.5;
        for(let i=0; i<MAX_P; i++) { let x = (i/MAX_P)*w; let db = 20*Math.log10(tMag[i] || 1e-6); let y = h/2 - (db * 3); if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y); }
        ctx.stroke();
    }

    function drawPEQ(ch) {
        let c = document.getElementById(`peq-cv-${ch}`); if(!c || !audioCtx) return;
        let ctx = c.getContext('2d'), w = c.width = c.clientWidth, h = c.height = 180; drawGrid(ctx, w, h);
        tMag.fill(1.0); 
        CH[ch].hpf[3].getFrequencyResponse(freqs, mag, phs); for(let i=0; i<MAX_P; i++) tMag[i]*=mag[i];
        CH[ch].lpf[3].getFrequencyResponse(freqs, mag, phs); for(let i=0; i<MAX_P; i++) tMag[i]*=mag[i];
        for(let j=0; j<5; j++) { CH[ch].peq[j].getFrequencyResponse(freqs, mag, phs); for(let i=0; i<MAX_P; i++) tMag[i]*=mag[i]; }
        ctx.beginPath(); ctx.strokeStyle = ch==='out1'?'#00e5ff':'#ff2a4d'; ctx.lineWidth = 2.5;
        for(let i=0; i<MAX_P; i++) { let x = (i/MAX_P)*w; let db = 20*Math.log10(tMag[i] || 1e-6); let y = h/2 - (db * 3); if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y); }
        ctx.stroke();
    }

    function drawComp(ch, inDb = -60) {
        let c = document.getElementById(`comp-cv-${ch}`); if(!c) return;
        let ctx = c.getContext('2d'), w = c.width = c.clientWidth, h = c.height = 160;
        ctx.fillStyle = '#030406'; ctx.fillRect(0,0,w,h);
        
        let thr = parseFloat(document.getElementById(`comp-thr-${ch}`).value), rat = parseFloat(document.getElementById(`comp-rat-${ch}`).value);
        let thrX = ((thr + 60) / 60) * w;
        ctx.strokeStyle = '#334155'; ctx.lineWidth = 1; ctx.setLineDash([4, 4]);
        ctx.beginPath(); ctx.moveTo(thrX, 0); ctx.lineTo(thrX, h); ctx.stroke(); ctx.setLineDash([]);
        
        ctx.strokeStyle = '#1e293b'; ctx.lineWidth = 1; ctx.beginPath(); ctx.moveTo(0,h); ctx.lineTo(w,0); ctx.stroke();
        let outDb = inDb < thr ? inDb : thr + (inDb - thr) / rat;
        
        if (inDb > -60) {
            let dotX = ((inDb + 60) / 60) * w, dotY = h - ((outDb + 60) / 60) * h;
            ctx.fillStyle = ch==='ina'?'rgba(0, 229, 255, 0.15)':'rgba(255, 42, 77, 0.15)';
            ctx.fillRect(0, dotY, dotX, h - dotY);
        }

        ctx.beginPath(); ctx.strokeStyle = ch==='ina'?'#00e5ff':'#ff2a4d'; ctx.lineWidth = 2.5;
        for(let i = -60; i <= 0; i++) {
            let o = i < thr ? i : thr + (i - thr) / rat;
            let x = ((i + 60) / 60) * w, y = h - ((o + 60) / 60) * h;
            if(i === -60) ctx.moveTo(x,y); else ctx.lineTo(x,y);
        }
        ctx.stroke();

        if (inDb > -60) {
            let dotX = ((inDb + 60) / 60) * w, dotY = h - ((outDb + 60) / 60) * h;
            ctx.beginPath(); ctx.arc(dotX, dotY, 5, 0, 2*Math.PI); ctx.fillStyle = '#fff';
            ctx.shadowBlur = 12; ctx.shadowColor = ch==='ina'?'#00e5ff':'#ff2a4d'; ctx.fill(); ctx.shadowBlur = 0;
            let gr = inDb - outDb;
            if (gr > 0.1) {
                ctx.fillStyle = '#ff1744'; let grH = (gr / 20) * h; if (grH > h) grH = h;
                ctx.fillRect(w - 6, 0, 6, grH);
            }
        }
    }

    function drawAllCanvas() { ['ina','inb'].forEach(ch => { drawGEQ(ch); drawComp(ch); }); ['out1','out2'].forEach(ch => drawPEQ(ch)); }
    window.addEventListener('resize', drawAllCanvas); drawAllCanvas();

    function renderRealtime() {
        if(!audioCtx) return requestAnimationFrame(renderRealtime);
        let getDb = (a) => { if(!a) return -60; let d = new Float32Array(a.fftSize); a.getFloatTimeDomainData(d); let m = 0; for(let i=0; i<d.length; i++) if(Math.abs(d[i])>m) m = Math.abs(d[i]); return 20*Math.log10(m||1e-6); };

        document.getElementById('vu-master').style.width = Math.max(0, Math.min(100, ((getDb(anaM) + 60) / 63)*100))+'%';
        document.getElementById('vu-ina').style.width = Math.max(0, Math.min(100, ((getDb(anaA) + 60) / 63)*100))+'%';
        document.getElementById('vu-inb').style.width = Math.max(0, Math.min(100, ((getDb(anaB) + 60) / 63)*100))+'%';
        document.getElementById('vu-out1').style.width = Math.max(0, Math.min(100, ((getDb(anaO1) + 60) / 63)*100))+'%';
        document.getElementById('vu-out2').style.width = Math.max(0, Math.min(100, ((getDb(anaO2) + 60) / 63)*100))+'%';

        let cA = document.getElementById('comp-cv-ina'); if(cA && cA.offsetParent !== null) drawComp('ina', Math.max(-60, getDb(anaCompInA)));
        let cB = document.getElementById('comp-cv-inb'); if(cB && cB.offsetParent !== null) drawComp('inb', Math.max(-60, getDb(anaCompInB)));

        let c = document.getElementById('rta-canvas');
        if(c && c.offsetParent !== null && freqDataM) {
            let ctx = c.getContext('2d'), w = c.width = c.clientWidth, h = c.height = 220; drawGrid(ctx, w, h);
            anaM.getByteFrequencyData(freqDataM);
            
            ctx.fillStyle = 'rgba(255, 170, 0, 0.4)';
            let nyquist = audioCtx.sampleRate / 2;
            let binCount = anaM.frequencyBinCount;
            
            for(let i=0; i<w; i+=2) {
                let f1 = 20 * Math.pow(1000, i/w);
                let f2 = 20 * Math.pow(1000, (i+2)/w);
                let bin1 = Math.floor((f1 / nyquist) * binCount);
                let bin2 = Math.floor((f2 / nyquist) * binCount);
                
                let maxVal = 0;
                if(bin1 === bin2) {
                    maxVal = freqDataM[bin1] || 0;
                } else {
                    for(let b=bin1; b<=bin2 && b<binCount; b++) {
                        if(freqDataM[b] > maxVal) maxVal = freqDataM[b];
                    }
                }
                
                let bh = (maxVal / 255) * h;
                ctx.fillRect(i, h-bh, 2, bh);
            }
        }
        requestAnimationFrame(renderRealtime);
    }

    const ae = document.getElementById('core-audio');
    let playlist = [];
    let currentTrackIdx = 0;
    const formatTime = s => isNaN(s) ? "0:00" : `${Math.floor(s/60)}:${Math.floor(s%60).toString().padStart(2, '0')}`;

    document.getElementById('audio-file').addEventListener('change', function(e) {
        if(!e.target.files.length) return;
        playlist = Array.from(e.target.files);
        let listUI = document.getElementById('playlist-ui');
        listUI.innerHTML = '';
        playlist.forEach((file, index) => {
            listUI.innerHTML += `<option value="${index}">${index + 1}. ${file.name}</option>`;
        });
        playTrack(0);
    });

    document.getElementById('playlist-ui').addEventListener('change', function(e) {
        playTrack(parseInt(e.target.value));
    });

    function playTrack(index) {
        if(playlist.length === 0 || index >= playlist.length) return;
        currentTrackIdx = index;
        document.getElementById('playlist-ui').value = index;
        ae.src = URL.createObjectURL(playlist[index]);
        initAudio();
        if(audioCtx.state === 'suspended') audioCtx.resume();
        ae.play();
    }

    ae.addEventListener('ended', () => {
        if (currentTrackIdx + 1 < playlist.length) {
            playTrack(currentTrackIdx + 1);
        }
    });

    const seekBar = document.getElementById('seek-bar');
    const timeCurr = document.getElementById('time-curr');
    const timeDur = document.getElementById('time-dur');
    
    ae.addEventListener('loadedmetadata', () => { timeDur.innerText = formatTime(ae.duration); });
    
    ae.addEventListener('timeupdate', () => {
        if(!ae.duration) return;
        let percent = (ae.currentTime / ae.duration) * 100;
        seekBar.value = percent;
        timeCurr.innerText = formatTime(ae.currentTime);
    });
    
    seekBar.addEventListener('input', (e) => {
        if(ae.duration) ae.currentTime = (e.target.value / 100) * ae.duration;
    });

    document.getElementById('btn-play').addEventListener('click', () => { initAudio(); if(audioCtx.state==='suspended') audioCtx.resume(); ae.play(); });
    document.getElementById('btn-stop').addEventListener('click', () => { ae.pause(); ae.currentTime = 0; });
    
    document.getElementById('btn-pink').addEventListener('click', function() {
        initAudio();
        if(!pinkSrc) { pinkSrc = audioCtx.createBufferSource(); pinkSrc.buffer = createPinkNoise(); pinkSrc.loop = true; pinkSrc.connect(oscGain); pinkSrc.start(); }
        this.classList.toggle('active');
        if(this.classList.contains('active')) { oscGain.gain.value = 1; this.innerText = "〰️ PINK NOISE (ON)"; }
        else { oscGain.gain.value = 0; this.innerText = "〰️ PINK NOISE"; }
    });

    document.getElementById('btn-mic').addEventListener('click', async function() {
        initAudio();
        if(this.classList.contains('active')) {
            if(srcMic) srcMic.disconnect(); if(streamMic) streamMic.getTracks().forEach(t=>t.stop());
            this.classList.remove('active'); this.innerText = "🎙️ LIVE LINE-IN";
        } else {
            try {
                streamMic = await navigator.mediaDevices.getUserMedia({audio: {echoCancellation:false, noiseSuppression:false, autoGainControl:false}});
                srcMic = audioCtx.createMediaStreamSource(streamMic); srcMic.connect(splitter);
                this.classList.add('active'); this.innerText = "🎙️ LIVE IN (ACTIVE)";
            } catch(e) { alert("Mic Access Denied!"); }
        }
    });

    function loadPresetList() {
        let s = document.getElementById('preset-sel'); s.innerHTML = '<option value="">-- Presets --</option>';
        for(let i=0; i<localStorage.length; i++) { let k = localStorage.key(i); if(k.startsWith('wu_')) s.innerHTML += `<option value="${k}">${k.replace('wu_','')}</option>`; }
    }
    function savePreset() {
        let n = prompt("Nama Preset:"); if(!n) return;
        let d = {}; document.querySelectorAll('input[type="range"], select').forEach(e => { if(e.id && e.id !== 'preset-sel' && e.id !== 'playlist-ui' && e.id !== 'seek-bar') d[e.id] = e.value; });
        localStorage.setItem('wu_'+n, JSON.stringify(d)); loadPresetList(); alert("Saved!");
    }
    function deletePreset() {
        let v = document.getElementById('preset-sel').value; if(!v) return;
        if(confirm("Hapus?")) { localStorage.removeItem(v); loadPresetList(); }
    }
    document.getElementById('preset-sel').addEventListener('change', function() {
        if(!this.value) return; let d = JSON.parse(localStorage.getItem(this.value)); if(!d) return;
        for(let k in d) { let e = document.getElementById(k); if(e) { e.value = d[k]; e.dispatchEvent(new Event(e.tagName==='SELECT'?'change':'input')); } }
    });
    window.onload = loadPresetList;

</script>
</body>
</html>
