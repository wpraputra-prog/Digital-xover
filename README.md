<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Wahyu Audio Project - Pro DSP v9.3 (RTA + DB Scale)</title>
    <style>
        :root {
            /* Modern Studio Color Palette */
            --bg-base: #0a0a0c; 
            --bg-panel: #13151a; 
            --bg-surface: #1c1f26;
            --accent-blue: #00d2ff; 
            --accent-red: #ff2a4d; 
            --accent-brand: #ffaa00;
            --text-main: #f0f2f5; 
            --text-muted: #7c8594; 
            --border: #2a2f3a;
            --radius: 12px; 
            --radius-sm: 6px;
            --led-green: #00e676; 
            --led-yellow: #ffea00; 
            --led-red: #ff1744;
            --shadow: 0 4px 15px rgba(0, 0, 0, 0.4);
        }

        * { box-sizing: border-box; margin: 0; padding: 0; touch-action: manipulation; }
        body { font-family: 'Inter', system-ui, sans-serif; background-color: var(--bg-base); color: var(--text-main); padding: 15px; }

        /* Container & Header */
        .container { max-width: 1200px; margin: 0 auto; display: flex; flex-direction: column; gap: 15px; }
        .header { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; padding: 20px; background: linear-gradient(135deg, var(--bg-panel) 0%, #0d0f12 100%); border: 1px solid var(--border); border-radius: var(--radius); box-shadow: var(--shadow); }
        .header h1 { font-size: 1.6rem; font-weight: 900; color: #fff; letter-spacing: -0.5px; }
        .header h1 span { color: var(--accent-brand); }
        .header-brand { font-size: 0.7rem; color: var(--text-muted); letter-spacing: 2px; margin-top: 4px; display: block; font-weight: 600;}

        /* Segmented Tabs Navigation */
        .tab-nav { display: flex; gap: 4px; margin-bottom: 5px; background: var(--bg-panel); padding: 6px; border-radius: 20px; border: 1px solid var(--border); box-shadow: var(--shadow); }
        .tab-btn { flex: 1; padding: 12px 10px; font-size: 0.75rem; font-weight: 700; background: transparent; border: none; border-radius: 16px; color: var(--text-muted); cursor: pointer; transition: all 0.3s ease; text-transform: uppercase; letter-spacing: 1px;}
        .tab-btn:hover { color: var(--text-main); }
        .tab-btn.active { background: var(--bg-surface); color: var(--text-main); box-shadow: 0 2px 8px rgba(0,0,0,0.2); }
        .tab-btn[data-target="tab-cha"].active { color: var(--accent-blue); }
        .tab-btn[data-target="tab-chb"].active { color: var(--accent-red); }
        .tab-btn[data-target="tab-master"].active { color: var(--accent-brand); }
        
        .tab-content { display: none; }
        .tab-content.active { display: flex; flex-direction: column; gap: 15px; animation: fadeIn 0.3s ease; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

        /* Preset Panel */
        .preset-panel { display: flex; gap: 8px; flex: 1; min-width: 280px; }
        .preset-panel select { flex: 2; padding: 10px; background: var(--bg-surface); border: 1px solid var(--border); color: var(--text-main); border-radius: var(--radius-sm); outline: none; font-size: 0.8rem; font-weight: 600; cursor: pointer;}
        .preset-panel button { flex: 1; padding: 10px; font-size: 0.75rem; font-weight: 800; border-radius: var(--radius-sm); border: none; cursor: pointer; text-transform: uppercase; letter-spacing: 1px;}
        .btn-save { background: rgba(0, 230, 118, 0.15); color: var(--led-green); border: 1px solid rgba(0, 230, 118, 0.3) !important; }
        .btn-save:active { background: var(--led-green); color: #000; }
        .btn-delete { background: rgba(255, 23, 68, 0.15); color: var(--led-red); border: 1px solid rgba(255, 23, 68, 0.3) !important; }
        .btn-delete:active { background: var(--led-red); color: #000; }

        /* Modules & Cards */
        .module-box { background: var(--bg-panel); border: 1px solid var(--border); border-radius: var(--radius); padding: 20px; box-shadow: var(--shadow); }
        .module-title { font-size: 0.75rem; color: var(--text-muted); text-transform: uppercase; letter-spacing: 1.5px; margin-bottom: 15px; font-weight: 800; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid var(--border); padding-bottom: 10px;}
        
        .grid-2col { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 15px; }

        /* Inputs & Buttons */
        input[type="file"], .filter-select { width: 100%; padding: 10px; background: var(--bg-surface); border: 1px solid var(--border); color: var(--text-main); font-size: 0.8rem; border-radius: var(--radius-sm); margin-bottom: 10px; outline:none; transition: 0.2s;}
        .filter-select:focus { border-color: var(--accent-brand); }
        
        button { background: var(--bg-surface); border: 1px solid var(--border); color: var(--text-main); border-radius: var(--radius-sm); padding: 10px; font-weight: 700; font-size: 0.8rem; cursor: pointer; transition: all 0.2s; text-transform: uppercase; letter-spacing: 0.5px;}
        button:active { transform: scale(0.97); }
        
        .btn-live { background: transparent; color: var(--text-muted); border: 1px solid var(--border); }
        .btn-live.active { background: var(--led-red); color: #fff; border-color: var(--led-red); box-shadow: 0 0 10px rgba(255, 23, 68, 0.4); }
        
        .btn-link { background: rgba(255, 170, 0, 0.1); border: 1px solid rgba(255, 170, 0, 0.3); color: var(--accent-brand); padding: 8px 12px; }
        .btn-link.active { background: var(--accent-brand); color: #000; box-shadow: 0 0 12px rgba(255, 170, 0, 0.4); }
        
        .btn-invert.active { background: var(--accent-blue); color: #000; border-color: var(--accent-blue); box-shadow: 0 0 10px rgba(0, 210, 255, 0.4); }

        /* Sleek VU Meter */
        .vu-meter-wrapper { width: 100%; margin-top: 5px; margin-bottom: 5px; position: relative; }
        .vu-meter-container { width: 100%; height: 8px; background: #050505; border-radius: 4px; border: 1px solid #1a1c23; overflow: hidden; position: relative; box-shadow: inset 0 1px 3px rgba(0,0,0,0.8); }
        .vu-meter-bar { height: 100%; width: 0%; transition: width 0.05s ease-out; box-shadow: 0 0 8px currentColor; }
        .vu-scale { display: flex; justify-content: space-between; font-size: 0.55rem; color: var(--text-muted); margin-top: 4px; font-weight: 700; padding: 0 1%; font-family: monospace;}
        
        .vu-master { background: linear-gradient(90deg, var(--led-green) 60%, var(--led-yellow) 85%, var(--led-red) 100%); }
        .vu-cha { background: linear-gradient(90deg, var(--accent-blue) 60%, var(--led-yellow) 85%, var(--led-red) 100%); }
        .vu-chb { background: linear-gradient(90deg, var(--accent-red) 60%, var(--led-yellow) 85%, var(--led-red) 100%); }

        /* Canvas Display */
        canvas { width: 100%; background: #050608; border-radius: var(--radius-sm); border: 1px solid #1a1c23; box-shadow: inset 0 2px 10px rgba(0,0,0,0.5); }
        .xover-canvas { height: 180px; } 
        .peq-canvas { height: 140px; margin-bottom: 15px; }

        .channel-header { display: flex; justify-content: space-between; align-items: center; padding-bottom: 15px; border-bottom: 1px solid var(--border); margin-bottom: 20px;}
        .channel-title { font-size: 1.2rem; font-weight: 900; display: flex; align-items: center; gap: 10px; letter-spacing: 1px;}
        .ch-extras { display: flex; gap: 10px; }

        .limiter-led { width: 10px; height: 10px; border-radius: 50%; background: #1a1a1a; margin-left: 8px; display: inline-block; box-shadow: inset 0 1px 2px #000; transition: all 0.05s; border: 1px solid #333;}
        .limiter-led.active { background: var(--led-red); box-shadow: 0 0 10px var(--led-red), inset 0 0 5px #fff; border-color: var(--led-red); }

        /* Control Grids & Knobs */
        .control-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 12px; margin-bottom: 20px;}
        .knob-group { display: flex; flex-direction: column; align-items: center; background: var(--bg-surface); border: 1px solid var(--border); padding: 15px 10px; border-radius: var(--radius-sm); transition: 0.2s;}
        .knob-group:hover { border-color: #3a4150; }
        .knob-group label { font-size: 0.65rem; color: var(--text-muted); margin-bottom: 12px; text-align: center; font-weight: 800; text-transform: uppercase; letter-spacing: 1px;}
        
        .val-input-group { display: flex; align-items: center; gap: 4px; margin-top: 10px; background: #08090a; padding: 4px 10px; border-radius: 12px; border: 1px solid #222; box-shadow: inset 0 1px 3px rgba(0,0,0,0.5);}
        .val-input { background: transparent; border: none; color: var(--text-main); font-family: monospace; font-size: 0.85rem; font-weight: 700; text-align: right; width: 45px; outline: none; }
        .val-input::-webkit-outer-spin-button, .val-input::-webkit-inner-spin-button { -webkit-appearance: none; margin: 0; }
        .val-unit { font-size: 0.65rem; color: var(--text-muted); font-weight: 700; }
        
        /* Custom Pro Faders (Sliders) */
        input[type="range"] { width: 100%; height: 4px; background: #222; outline: none; -webkit-appearance: none; border-radius: 2px; border: none; margin: 12px 0; position: relative; }
        input[type="range"]::-webkit-slider-thumb { 
            -webkit-appearance: none; width: 14px; height: 26px; background: #d1d5db; 
            border-radius: 4px; border: 1px solid #fff; cursor: pointer; 
            box-shadow: 0 2px 6px rgba(0,0,0,0.8), inset 0 -2px 2px rgba(0,0,0,0.2); 
            transition: 0.1s; 
        }
        input[type="range"]::-webkit-slider-thumb:active { background: var(--accent-brand); border-color: #fff; transform: scale(1.05);}
        
        .range-wrapper { display: flex; width: 100%; align-items: center; gap: 8px; flex-direction: column; }
        .range-wrapper.horizontal { flex-direction: row; }
        .btn-adjust { background: var(--bg-panel); color: var(--text-muted); border: 1px solid var(--border); width: 32px; height: 32px; display: flex; justify-content: center; align-items: center; border-radius: var(--radius-sm); font-size: 1.2rem; cursor:pointer;}
        .btn-adjust:hover { background: var(--bg-surface); color: var(--text-main); border-color: #3a4150;}

        /* PEQ Band Styles */
        .peq-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 12px; }
        .peq-band { background: var(--bg-surface); padding: 15px; border: 1px solid var(--border); border-radius: var(--radius-sm); }
        .peq-band-title { font-size: 0.75rem; color: var(--accent-brand); text-align: center; margin-bottom: 12px; font-weight: 900; letter-spacing: 1px; background: #08090a; padding: 6px; border-radius: 4px; border: 1px solid #222;}
        .peq-header { display: flex; justify-content: space-between; align-items: center; font-size: 0.65rem; color: var(--text-muted); font-weight: 800; margin-bottom: 5px; text-transform: uppercase;}

        #tab-cha .val-input { color: var(--accent-blue); }
        #tab-chb .val-input { color: var(--accent-red); }

        @media (min-width: 768px) {
            .tab-nav { display: none; }
            .tab-content { display: flex !important; flex-direction: column; gap: 20px; opacity: 1;}
            #dsp-container { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
            .master-span { grid-column: 1 / -1; }
        }
    </style>
</head>
<body>

<div class="container">
    <div class="header">
        <div>
            <h1>WU <span>AUDIO</span> DLMS</h1>
            <span class="header-brand">WAHYU AUDIO PROJECT - FULL DSP v9.3</span>
        </div>
        <div class="preset-panel">
            <select id="preset-selector"><option value="">-- Load Preset --</option></select>
            <button class="btn-save" onclick="savePreset()">SAVE</button>
            <button class="btn-delete" onclick="deletePreset()">DEL</button>
        </div>
        <button id="btn-stereo-link" class="btn-link" style="width: 100%; margin-top:5px; border-radius: 20px;">🔗 STEREO A/B LINK: OFF</button>
    </div>

    <div class="tab-nav">
        <button class="tab-btn active" data-target="tab-master">MASTER</button>
        <button class="tab-btn" data-target="tab-cha">CH A (L)</button>
        <button class="tab-btn" data-target="tab-chb">CH B (R)</button>
    </div>

    <div id="tab-master" class="tab-content active master-span">
        <div class="grid-2col">
            <div class="module-box">
                <div class="module-title">Main Input Source</div>
                <input type="file" id="audio-upload" accept="audio/*">
                
                <div class="range-wrapper horizontal" style="margin: 15px 0;">
                    <span id="time-current" style="font-size: 0.7rem; color: var(--text-muted); width: 45px; font-family: monospace;">0:00</span>
                    <input type="range" id="audio-seek" min="0" max="100" step="0.1" value="0">
                    <span id="time-total" style="font-size: 0.7rem; color: var(--text-muted); width: 45px; text-align: right; font-family: monospace;">0:00</span>
                </div>

                <div style="display: flex; gap: 8px; margin-bottom: 15px;">
                    <button id="btn-play" style="flex: 2; background: rgba(0, 230, 118, 0.1); color: var(--led-green); border-color: rgba(0,230,118,0.3);">▶ PLAY</button>
                    <button id="btn-stop" style="flex: 1;">⏹ STOP</button>
                </div>
                <button id="btn-live-input" class="btn-live" style="width: 100%; border-color: rgba(255, 170, 0, 0.3); color: var(--accent-brand);">🎙️ Enable Live Line-In</button>

                <div style="margin-top: 25px;">
                    <span style="font-size: 0.65rem; color: var(--text-muted); font-weight: 800; letter-spacing: 1px;">MASTER VU METER (dB)</span>
                    <div class="vu-meter-wrapper">
                        <div class="vu-meter-container"><div id="vu-master-bar" class="vu-meter-bar vu-master"></div></div>
                        <div class="vu-scale"><span>-60</span><span>-40</span><span>-20</span><span>-12</span><span>-6</span><span>0</span><span>+3</span></div>
                    </div>
                </div>
                
                <div style="margin-top:25px; padding-top:15px; border-top: 1px dashed var(--border);">
                    <div class="module-title" style="color:var(--accent-blue); border:none; padding:0; margin-bottom: 10px;">Delay Align Calculator</div>
                    <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom:15px;">
                        <div><label style="font-size:0.65rem; color:var(--text-muted); font-weight:700;">Jarak Main (M)</label><input type="number" id="calc-dist-main" class="filter-select" value="0" step="0.1" style="margin-bottom:0; background:#050505;"></div>
                        <div><label style="font-size:0.65rem; color:var(--text-muted); font-weight:700;">Jarak Sub (M)</label><input type="number" id="calc-dist-sub" class="filter-select" value="0" step="0.1" style="margin-bottom:0; background:#050505;"></div>
                    </div>
                    <div style="display:flex; justify-content: space-between; align-items:center; background: #050505; padding:12px; border-radius:var(--radius-sm); border:1px solid #222; box-shadow: inset 0 1px 4px #000;">
                        <span style="font-size:0.7rem; color:var(--text-muted); font-weight:600;">Kompensasi: <br><strong id="calc-result" style="color:var(--led-green); font-size:1.1rem; font-family: monospace;">0.00 ms</strong></span>
                        <div style="display:flex; flex-direction:column; gap:6px;">
                            <button class="btn-link" onclick="applyCalculatedDelay('l')" style="padding:6px 10px; font-size:0.65rem; border-radius:4px;">SET CH A</button>
                            <button class="btn-link" onclick="applyCalculatedDelay('r')" style="padding:6px 10px; font-size:0.65rem; border-radius:4px;">SET CH B</button>
                        </div>
                    </div>
                </div>
            </div>

            <div class="module-box" style="display: flex; flex-direction: column; justify-content: space-between;">
                <div>
                    <div class="module-title">Pink Noise Generator (RTA)</div>
                    <div style="margin-top: 5px; border-bottom: 1px solid var(--border); padding-bottom: 20px;">
                        <button id="btn-noise-toggle" style="width: 100%; color: var(--led-red); border-color: rgba(255, 23, 68, 0.3); background: rgba(255, 23, 68, 0.05);">PINK NOISE (OFF)</button>
                        <span style="display:block; font-size:0.65rem; color:var(--text-muted); text-align:center; margin-top:10px; font-family: monospace;">Spectrum: 20Hz - 20kHz</span>
                    </div>
                </div>
                
                <div style="padding-top: 20px; text-align: center;">
                    <div class="module-title" style="justify-content: center; border:none;">Master Output Level</div>
                    <input type="range" id="vol-slider" min="0" max="150" step="1" value="80" style="margin: 20px 0;">
                    <div class="val-input-group" style="width: max-content; margin: 0 auto; background: #050505;"><input type="number" id="num-vol-slider" class="val-input" value="80" style="color:var(--accent-brand); font-size: 1rem; width: 55px;"><span class="val-unit">%</span></div>
                </div>
            </div>
        </div>
        
        <div class="module-box">
            <div class="module-title">Real-Time Analyzer (RTA) & Crossover Response</div>
            <canvas id="xover-master" class="xover-canvas"></canvas>
        </div>
    </div>

    <div id="dsp-container">
        <!-- CHANNEL A -->
        <div id="tab-cha" class="tab-content module-box">
            <div class="channel-header">
                <div class="channel-title" style="color: var(--accent-blue);">CH A (LEFT)</div>
                <div class="ch-extras">
                    <button id="polarity-l" class="btn-live btn-invert">Ø INV</button>
                    <button id="mute-l" class="btn-live">MUTE</button>
                </div>
            </div>
            
            <div style="margin-bottom: 25px;">
                <span style="font-size: 0.65rem; color: var(--text-muted); font-weight: 800; letter-spacing: 1px;">OUT A METER (dB)</span>
                <div class="vu-meter-wrapper">
                    <div class="vu-meter-container"><div id="vu-cha-bar" class="vu-meter-bar vu-cha"></div></div>
                    <div class="vu-scale"><span>-60</span><span>-40</span><span>-20</span><span>-12</span><span>-6</span><span>0</span><span>+3</span></div>
                </div>
            </div>

            <div class="control-grid">
                <div class="knob-group"><label>Gain</label><input type="range" id="gain-l" min="-24" max="12" step="0.1" value="0"><div class="val-input-group"><input type="number" id="num-gain-l" class="val-input" value="0" step="0.1"><span class="val-unit">dB</span></div></div>
                <div class="knob-group"><label>Limiter <span id="led-limit-l" class="limiter-led"></span></label><input type="range" id="limit-l" min="-40" max="0" step="1" value="-2"><div class="val-input-group"><input type="number" id="num-limit-l" class="val-input" value="-2" step="1"><span class="val-unit">dB</span></div></div>
                <div class="knob-group"><label>Delay</label><input type="range" id="delay-l" min="0" max="100" step="0.1" value="0"><div class="val-input-group"><input type="number" id="num-delay-l" class="val-input" value="0" step="0.1"><span class="val-unit">ms</span></div></div>
                <div class="knob-group"><label>Phase(APF)</label><input type="range" id="apf-l" min="20" max="20000" step="1" value="20000"><div class="val-input-group"><input type="number" id="num-apf-l" class="val-input" value="20000" step="1"><span class="val-unit">Hz</span></div></div>
            </div>
            
            <div class="control-grid">
                <div class="knob-group" style="grid-column: span 2;">
                    <label>HPF (Low Cut)</label>
                    <select id="hpf-type-l" class="filter-select"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select>
                    <div class="range-wrapper horizontal">
                        <button class="btn-adjust" onclick="adjustVal('hpf-l', -1)">-</button>
                        <input type="range" id="hpf-l" min="20" max="16000" step="1" value="40">
                        <button class="btn-adjust" onclick="adjustVal('hpf-l', 1)">+</button>
                    </div>
                    <div class="val-input-group"><input type="number" id="num-hpf-l" class="val-input" value="40" step="1"><span class="val-unit">Hz</span></div>
                </div>
                <div class="knob-group" style="grid-column: span 2;">
                    <label>LPF (High Cut)</label>
                    <select id="lpf-type-l" class="filter-select"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select>
                    <div class="range-wrapper horizontal">
                        <button class="btn-adjust" onclick="adjustVal('lpf-l', -10)">-</button>
                        <input type="range" id="lpf-l" min="20" max="16000" step="10" value="16000">
                        <button class="btn-adjust" onclick="adjustVal('lpf-l', 10)">+</button>
                    </div>
                    <div class="val-input-group"><input type="number" id="num-lpf-l" class="val-input" value="16000" step="10"><span class="val-unit">Hz</span></div>
                </div>
            </div>
            
            <div class="module-title">Auto FIR Generator</div>
            <div style="display: flex; gap: 8px; margin-bottom: 20px;">
                <select id="auto-fir-type-l" class="filter-select" style="margin: 0; flex: 2;"><option value="flat">Preset: Linear Flat</option><option value="sub">Preset: Sub Enhancer</option></select>
                <button id="btn-fir-l" style="flex: 1; border-color: var(--border);">FIR (OFF)</button>
            </div>

            <div class="module-title">Parametric EQ (5-Band)</div>
            <canvas id="peq-canvas-l" class="peq-canvas"></canvas>
            <div class="peq-grid" id="peq-ui-l"></div>
        </div>

        <!-- CHANNEL B -->
        <div id="tab-chb" class="tab-content module-box">
            <div class="channel-header">
                <div class="channel-title" style="color: var(--accent-red);">CH B (RIGHT)</div>
                <div class="ch-extras">
                    <button id="polarity-r" class="btn-live btn-invert">Ø INV</button>
                    <button id="mute-r" class="btn-live">MUTE</button>
                </div>
            </div>

            <div style="margin-bottom: 25px;">
                <span style="font-size: 0.65rem; color: var(--text-muted); font-weight: 800; letter-spacing: 1px;">OUT B METER (dB)</span>
                <div class="vu-meter-wrapper">
                    <div class="vu-meter-container"><div id="vu-chb-bar" class="vu-meter-bar vu-chb"></div></div>
                    <div class="vu-scale"><span>-60</span><span>-40</span><span>-20</span><span>-12</span><span>-6</span><span>0</span><span>+3</span></div>
                </div>
            </div>

            <div class="control-grid">
                <div class="knob-group"><label>Gain</label><input type="range" id="gain-r" min="-24" max="12" step="0.1" value="0"><div class="val-input-group"><input type="number" id="num-gain-r" class="val-input" value="0" step="0.1"><span class="val-unit">dB</span></div></div>
                <div class="knob-group"><label>Limiter <span id="led-limit-r" class="limiter-led"></span></label><input type="range" id="limit-r" min="-40" max="0" step="1" value="-2"><div class="val-input-group"><input type="number" id="num-limit-r" class="val-input" value="-2" step="1"><span class="val-unit">dB</span></div></div>
                <div class="knob-group"><label>Delay</label><input type="range" id="delay-r" min="0" max="100" step="0.1" value="0"><div class="val-input-group"><input type="number" id="num-delay-r" class="val-input" value="0" step="0.1"><span class="val-unit">ms</span></div></div>
                <div class="knob-group"><label>Phase(APF)</label><input type="range" id="apf-r" min="20" max="20000" step="1" value="20000"><div class="val-input-group"><input type="number" id="num-apf-r" class="val-input" value="20000" step="1"><span class="val-unit">Hz</span></div></div>
            </div>
            
            <div class="control-grid">
                <div class="knob-group" style="grid-column: span 2;">
                    <label>HPF (Low Cut)</label>
                    <select id="hpf-type-r" class="filter-select"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select>
                    <div class="range-wrapper horizontal">
                        <button class="btn-adjust" onclick="adjustVal('hpf-r', -1)">-</button>
                        <input type="range" id="hpf-r" min="20" max="16000" step="1" value="40">
                        <button class="btn-adjust" onclick="adjustVal('hpf-r', 1)">+</button>
                    </div>
                    <div class="val-input-group"><input type="number" id="num-hpf-r" class="val-input" value="40" step="1"><span class="val-unit">Hz</span></div>
                </div>
                <div class="knob-group" style="grid-column: span 2;">
                    <label>LPF (High Cut)</label>
                    <select id="lpf-type-r" class="filter-select"><option value="BW12">BW12</option><option value="BW24">BW24</option><option value="LR24">LR24</option><option value="LR48" selected>LR48</option></select>
                    <div class="range-wrapper horizontal">
                        <button class="btn-adjust" onclick="adjustVal('lpf-r', -10)">-</button>
                        <input type="range" id="lpf-r" min="20" max="16000" step="10" value="16000">
                        <button class="btn-adjust" onclick="adjustVal('lpf-r', 10)">+</button>
                    </div>
                    <div class="val-input-group"><input type="number" id="num-lpf-r" class="val-input" value="16000" step="10"><span class="val-unit">Hz</span></div>
                </div>
            </div>

            <div class="module-title">Auto FIR Generator</div>
            <div style="display: flex; gap: 8px; margin-bottom: 20px;">
                <select id="auto-fir-type-r" class="filter-select" style="margin: 0; flex: 2;"><option value="flat">Preset: Linear Flat</option><option value="sub">Preset: Sub Enhancer</option></select>
                <button id="btn-fir-r" style="flex: 1; border-color: var(--border);">FIR (OFF)</button>
            </div>

            <div class="module-title">Parametric EQ (5-Band)</div>
            <canvas id="peq-canvas-r" class="peq-canvas"></canvas>
            <div class="peq-grid" id="peq-ui-r"></div>
        </div>
    </div>
</div>

<audio id="core-audio" crossorigin="anonymous"></audio>

<script>
    const tabs = document.querySelectorAll('.tab-btn');
    const contents = document.querySelectorAll('.tab-content');
    tabs.forEach(tab => {
        tab.addEventListener('click', () => {
            tabs.forEach(t => t.classList.remove('active')); contents.forEach(c => c.classList.remove('active'));
            tab.classList.add('active'); document.getElementById(tab.dataset.target).classList.add('active');
            setTimeout(() => { drawPEQ('l'); drawPEQ('r'); }, 50);
        });
    });

    let isStereoLinked = false;
    document.getElementById('btn-stereo-link').addEventListener('click', function() {
        isStereoLinked = !isStereoLinked; this.classList.toggle('active');
        this.innerText = isStereoLinked ? "🔗 STEREO A/B LINK: ON" : "🔗 STEREO A/B LINK: OFF";
    });

    function adjustVal(id, step) {
        let el = document.getElementById(id);
        let newVal = parseFloat(el.value) + step;
        if(newVal < parseFloat(el.min)) newVal = el.min;
        if(newVal > parseFloat(el.max)) newVal = el.max;
        el.value = newVal; el.dispatchEvent(new Event('input', {bubbles: true}));
    }

    // DELAY CALCULATOR LOGIC
    function calculateDelay() {
        let dm = parseFloat(document.getElementById('calc-dist-main').value) || 0;
        let ds = parseFloat(document.getElementById('calc-dist-sub').value) || 0;
        let speedOfSound = 343;
        let diff = Math.abs(dm - ds);
        let delayMs = (diff / speedOfSound) * 1000;
        document.getElementById('calc-result').innerText = delayMs.toFixed(2) + " ms";
        return delayMs;
    }
    
    document.getElementById('calc-dist-main').addEventListener('input', calculateDelay);
    document.getElementById('calc-dist-sub').addEventListener('input', calculateDelay);
    
    function applyCalculatedDelay(ch) {
        let delayValue = calculateDelay().toFixed(1);
        let targetId = 'delay-' + ch;
        let delaySlider = document.getElementById(targetId);
        
        if (delaySlider) {
            delaySlider.value = delayValue;
            delaySlider.dispatchEvent(new Event('input', {bubbles: true}));
            alert(`Delay ${delayValue}ms berhasil diterapkan ke Channel ${ch === 'l' ? 'A' : 'B'}`);
        }
    }

    // PRESET SYSTEM
    function initDefaultPresets() {
        if(!localStorage.getItem('wu_preset_Tasso_KF760_FOH')) {
            localStorage.setItem('wu_preset_Tasso_KF760_FOH', JSON.stringify({"gain-l":"0","limit-l":"-2","delay-l":"0","apf-l":"20000","hpf-type-l":"LR48","hpf-l":"55","lpf-type-l":"LR24","lpf-l":"16000","gain-r":"0","limit-r":"-2","delay-r":"0","apf-r":"20000","hpf-type-r":"LR48","hpf-r":"55","lpf-type-r":"LR24","lpf-r":"16000"}));
        }
    }

    function updatePresetDropdown() {
        let sel = document.getElementById('preset-selector');
        sel.innerHTML = '<option value="">-- Load Preset --</option>';
        for(let i=0; i<localStorage.length; i++) {
            let key = localStorage.key(i);
            if(key.startsWith('wu_preset_')) {
                let name = key.replace('wu_preset_', '');
                sel.innerHTML += `<option value="${name}">${name.replace(/_/g, ' ')}</option>`;
            }
        }
    }
    
    function savePreset() {
        let name = prompt("Masukkan Nama Preset Baru:");
        if(!name) return;
        let preset = {};
        document.querySelectorAll('input[type="range"], select').forEach(el => {
            if(el.id && el.id !== 'audio-seek' && el.id !== 'preset-selector') preset[el.id] = el.value;
        });
        localStorage.setItem('wu_preset_' + name.replace(/ /g, '_'), JSON.stringify(preset));
        updatePresetDropdown();
        alert(`Preset "${name}" berhasil disimpan!`);
    }

    function deletePreset() {
        let sel = document.getElementById('preset-selector').value;
        if(!sel) return alert("Pilih preset yang mau dihapus!");
        if(confirm(`Yakin hapus preset "${sel.replace(/_/g, ' ')}"?`)) {
            localStorage.removeItem('wu_preset_' + sel);
            updatePresetDropdown();
        }
    }

    document.getElementById('preset-selector').addEventListener('change', function() {
        if(!this.value) return;
        let preset = JSON.parse(localStorage.getItem('wu_preset_' + this.value));
        if(!preset) return;
        for(let key in preset) {
            let el = document.getElementById(key);
            if(el) { el.value = preset[key]; el.dispatchEvent(new Event(el.tagName === 'SELECT' ? 'change' : 'input')); }
        }
    });

    window.onload = () => { initDefaultPresets(); updatePresetDropdown(); };

    function generatePEQUI(ch) {
        const container = document.getElementById(`peq-ui-${ch}`);
        const defaults = [ {f:65, q:2, g:0}, {f:250, q:1.5, g:-2}, {f:1000, q:1.5, g:0.5}, {f:4000, q:2, g:2}, {f:8000, q:2.5, g:1} ];
        let html = '';
        defaults.forEach((d, i) => {
            html += `<div class="peq-band"><div class="peq-band-title">BAND ${i+1}</div>
                <div class="peq-header"><span>Gain</span><div class="val-input-group"><input type="number" id="num-peq-gain-${ch}-${i}" class="val-input" value="${d.g}" step="0.1"><span class="val-unit">dB</span></div></div>
                <input type="range" id="peq-gain-${ch}-${i}" min="-18" max="18" step="0.1" value="${d.g}">
                <div class="peq-header"><span>Freq</span><div class="val-input-group"><input type="number" id="num-peq-freq-${ch}-${i}" class="val-input" value="${d.f}" step="1"><span class="val-unit">Hz</span></div></div>
                <input type="range" id="peq-freq-${ch}-${i}" min="20" max="16000" step="1" value="${d.f}">
                <div class="peq-header"><span>Q</span><div class="val-input-group"><input type="number" id="num-peq-q-${ch}-${i}" class="val-input" value="${d.q}" step="0.05"><span class="val-unit"></span></div></div>
                <input type="range" id="peq-q-${ch}-${i}" min="0.1" max="15" step="0.05" value="${d.q}"></div>`;
        });
        container.innerHTML = html;
    }
    generatePEQUI('l'); generatePEQUI('r');

    // DSP AUDIO ENGINE (48kHz Studio Quality)
    let audioCtx, source, liveSource, liveStream;
    let masterGain, limiterL, limiterR, gainL, gainR, splitter, merger;
    let hpfL = [], lpfL = [], peqL = [], delayL, apfL, firL, firDryL, firWetL;
    let hpfR = [], lpfR = [], peqR = [], delayR, apfR, firR, firDryR, firWetR;
    let analyserMaster, analyserL, analyserR, dataArrM, dataArrL, dataArrR, freqDataM;
    let pinkNoiseNode, oscGain;
    let isFirLActive = false, isFirRActive = false;
    let polarityL = 1, polarityR = 1;
    
    const MAX_RENDER = 100;
    let freqs = new Float32Array(MAX_RENDER), mag = new Float32Array(MAX_RENDER), phase = new Float32Array(MAX_RENDER), totalMag = new Float32Array(MAX_RENDER);
    for (let i = 0; i < MAX_RENDER; i++) freqs[i] = 20 * Math.pow(1000, i / MAX_RENDER);

    function createPinkNoiseBuffer() {
        let bufferSize = 2 * audioCtx.sampleRate;
        let noiseBuf = audioCtx.createBuffer(1, bufferSize, audioCtx.sampleRate);
        let output = noiseBuf.getChannelData(0);
        let b0=0, b1=0, b2=0, b3=0, b4=0, b5=0, b6=0;
        for (let i = 0; i < bufferSize; i++) {
            let white = Math.random() * 2 - 1;
            b0 = 0.99886 * b0 + white * 0.0555179;
            b1 = 0.99332 * b1 + white * 0.0750759;
            b2 = 0.96900 * b2 + white * 0.1538520;
            b3 = 0.86650 * b3 + white * 0.3104856;
            b4 = 0.55000 * b4 + white * 0.5329522;
            b5 = -0.7616 * b5 - white * 0.0168980;
            let pink = b0 + b1 + b2 + b3 + b4 + b5 + b6 + white * 0.5362;
            output[i] = pink * 0.05;
            b6 = white * 0.115926;
        }
        return noiseBuf;
    }

    function createFlatFIRBuffer() {
        let buffer = audioCtx.createBuffer(1, audioCtx.sampleRate, audioCtx.sampleRate);
        let channelData = buffer.getChannelData(0);
        channelData[0] = 1.0; 
        for (let i = 1; i < channelData.length; i++) {
            channelData[i] = 0.0;
        }
        return buffer;
    }

    function initAudio() {
        if (audioCtx) return;
        const audioOptions = { sampleRate: 48000, latencyHint: 'interactive' };
        audioCtx = new (window.AudioContext || window.webkitAudioContext)(audioOptions);
        
        masterGain = audioCtx.createGain(); masterGain.gain.value = 0.8;
        splitter = audioCtx.createChannelSplitter(2); merger = audioCtx.createChannelMerger(2);
        
        analyserMaster = audioCtx.createAnalyser(); analyserL = audioCtx.createAnalyser(); analyserR = audioCtx.createAnalyser();
        analyserMaster.fftSize = 2048; analyserMaster.smoothingTimeConstant = 0.85;
        analyserL.fftSize = 2048; analyserR.fftSize = 2048;
        dataArrM = new Float32Array(analyserMaster.fftSize); 
        dataArrL = new Float32Array(analyserL.fftSize); 
        dataArrR = new Float32Array(analyserR.fftSize);
        freqDataM = new Uint8Array(analyserMaster.frequencyBinCount);

        // Init CH L Nodes (With Brickwall Limiter)
        gainL = audioCtx.createGain(); 
        limiterL = audioCtx.createDynamicsCompressor(); 
        limiterL.knee.value = 0.0; limiterL.ratio.value = 20.0; limiterL.attack.value = 0.002; limiterL.release.value = 0.050;
        delayL = audioCtx.createDelay(1.0); 
        apfL = audioCtx.createBiquadFilter(); apfL.type = 'allpass'; apfL.frequency.value = 20000;
        firL = audioCtx.createConvolver(); firL.normalize = false; firL.buffer = createFlatFIRBuffer();
        firDryL = audioCtx.createGain(); firWetL = audioCtx.createGain(); firWetL.gain.value = 0;
        for(let i=0; i<4; i++) { hpfL.push(audioCtx.createBiquadFilter()); hpfL[i].type = 'highpass'; lpfL.push(audioCtx.createBiquadFilter()); lpfL[i].type = 'lowpass'; }
        for(let i=0; i<5; i++) { peqL.push(audioCtx.createBiquadFilter()); peqL[i].type = 'peaking'; }

        // Init CH R Nodes
        gainR = audioCtx.createGain(); 
        limiterR = audioCtx.createDynamicsCompressor(); 
        limiterR.knee.value = 0.0; limiterR.ratio.value = 20.0; limiterR.attack.value = 0.002; limiterR.release.value = 0.050;
        delayR = audioCtx.createDelay(1.0); 
        apfR = audioCtx.createBiquadFilter(); apfR.type = 'allpass'; apfR.frequency.value = 20000;
        firR = audioCtx.createConvolver(); firR.normalize = false; firR.buffer = createFlatFIRBuffer();
        firDryR = audioCtx.createGain(); firWetR = audioCtx.createGain(); firWetR.gain.value = 0;
        for(let i=0; i<4; i++) { hpfR.push(audioCtx.createBiquadFilter()); hpfR[i].type = 'highpass'; lpfR.push(audioCtx.createBiquadFilter()); lpfR[i].type = 'lowpass'; }
        for(let i=0; i<5; i++) { peqR.push(audioCtx.createBiquadFilter()); peqR[i].type = 'peaking'; }

        // Routing Graph L
        splitter.connect(hpfL[0], 0); for(let i=0; i<3; i++) hpfL[i].connect(hpfL[i+1]); hpfL[3].connect(lpfL[0]); for(let i=0; i<3; i++) lpfL[i].connect(lpfL[i+1]); lpfL[3].connect(apfL);
        apfL.connect(peqL[0]); for(let i=0; i<4; i++) peqL[i].connect(peqL[i+1]);
        peqL[4].connect(firDryL); peqL[4].connect(firL); firL.connect(firWetL); firDryL.connect(delayL); firWetL.connect(delayL);
        delayL.connect(limiterL); limiterL.connect(gainL); gainL.connect(analyserL); gainL.connect(merger, 0, 0);

        // Routing Graph R
        splitter.connect(hpfR[0], 1); for(let i=0; i<3; i++) hpfR[i].connect(hpfR[i+1]); hpfR[3].connect(lpfR[0]); for(let i=0; i<3; i++) lpfR[i].connect(lpfR[i+1]); lpfR[3].connect(apfR);
        apfR.connect(peqR[0]); for(let i=0; i<4; i++) peqR[i].connect(peqR[i+1]);
        peqR[4].connect(firDryR); peqR[4].connect(firR); firR.connect(firWetR); firDryR.connect(delayR); firWetR.connect(delayR);
        delayR.connect(limiterR); limiterR.connect(gainR); gainR.connect(analyserR); gainR.connect(merger, 0, 1);

        merger.connect(masterGain); masterGain.connect(analyserMaster); masterGain.connect(audioCtx.destination);

        source = audioCtx.createMediaElementSource(document.getElementById('core-audio'));
        source.connect(splitter);

        oscGain = audioCtx.createGain(); oscGain.gain.value = 0; oscGain.connect(splitter);

        triggerInitialValues();
        renderRealtimeGraphics(); 
    }

    // Precise Q-Factor Linkwitz-Riley Algorithm
    function applyFilter(nodes, type, freq, isHPF) {
        let f = parseFloat(freq); let b = isHPF ? 10 : 24000;
        
        for(let i=0; i<4; i++) { nodes[i].frequency.value = b; nodes[i].Q.value = 0.7071; }
        
        if (type === 'BW12') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.7071; } 
        else if (type === 'BW24') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.5412; nodes[1].frequency.value = f; nodes[1].Q.value = 1.3065; }
        else if (type === 'LR24') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.7071; nodes[1].frequency.value = f; nodes[1].Q.value = 0.7071; }
        else if (type === 'LR48') { nodes[0].frequency.value = f; nodes[0].Q.value = 0.5412; nodes[1].frequency.value = f; nodes[1].Q.value = 1.3065; nodes[2].frequency.value = f; nodes[2].Q.value = 0.5412; nodes[3].frequency.value = f; nodes[3].Q.value = 1.3065; }
    }

    const bindCtrl = (id, action) => {
        const range = document.getElementById(id);
        const num = document.getElementById('num-' + id);
        if(!range || !num) return;

        const updateVal = (v, isTrusted) => {
            if(audioCtx) action(parseFloat(v));
            if(isStereoLinked && isTrusted && (id.endsWith('-l') || id.endsWith('-r'))) {
                let otherId = id.endsWith('-l') ? id.replace('-l', '-r') : id.replace('-r', '-l');
                let otherRange = document.getElementById(otherId);
                let otherNum = document.getElementById('num-' + otherId);
                if(otherRange && otherNum) { 
                    otherRange.value = v; otherNum.value = v; 
                    otherRange.dispatchEvent(new Event('input', {bubbles: false})); 
                }
            }
        };

        range.addEventListener('input', e => { num.value = e.target.value; updateVal(e.target.value, e.isTrusted); });
        
        num.addEventListener('change', e => {
            let v = parseFloat(e.target.value);
            if(v < parseFloat(range.min)) v = range.min;
            if(v > parseFloat(range.max)) v = range.max;
            range.value = v; num.value = v; updateVal(v, e.isTrusted);
        });
    };

    ['l', 'r'].forEach(ch => {
        bindCtrl(`gain-${ch}`, v => (ch==='l'?gainL:gainR).gain.value = Math.pow(10, v/20) * (ch==='l'?polarityL:polarityR));
        bindCtrl(`limit-${ch}`, v => (ch==='l'?limiterL:limiterR).threshold.value = v);
        bindCtrl(`delay-${ch}`, v => (ch==='l'?delayL:delayR).delayTime.value = v/1000);
        bindCtrl(`apf-${ch}`, v => (ch==='l'?apfL:apfR).frequency.value = v);
        
        const bindDropdown = (typeId) => {
            document.getElementById(typeId).addEventListener('change', function(e) { 
                document.getElementById(typeId.replace('-type','')).dispatchEvent(new Event('input')); 
                if(isStereoLinked && e.isTrusted) {
                    let otherId = typeId.endsWith('-l') ? typeId.replace('-l', '-r') : typeId.replace('-r', '-l');
                    let otherEl = document.getElementById(otherId);
                    if(otherEl) { otherEl.value = this.value; otherEl.dispatchEvent(new Event('change')); }
                }
            });
        };
        bindDropdown(`hpf-type-${ch}`);
        bindDropdown(`lpf-type-${ch}`);
        
        bindCtrl(`hpf-${ch}`, v => { applyFilter(ch==='l'?hpfL:hpfR, document.getElementById(`hpf-type-${ch}`).value, v, true); });
        bindCtrl(`lpf-${ch}`, v => { applyFilter(ch==='l'?lpfL:lpfR, document.getElementById(`lpf-type-${ch}`).value, v, false); });

        document.getElementById(`mute-${ch}`).addEventListener('click', function(e) {
            this.classList.toggle('active');
            let isMuted = this.classList.contains('active');
            if(audioCtx) (ch==='l'?gainL:gainR).gain.value = isMuted ? 0 : Math.pow(10, document.getElementById(`gain-${ch}`).value/20) * (ch==='l'?polarityL:polarityR);
            
            if(isStereoLinked && e.isTrusted) {
                let otherId = ch==='l' ? 'mute-r' : 'mute-l';
                let otherBtn = document.getElementById(otherId);
                if(isMuted !== otherBtn.classList.contains('active')) otherBtn.click();
            }
        });

        document.getElementById(`polarity-${ch}`).addEventListener('click', function(e) {
            this.classList.toggle('active');
            let isInverted = this.classList.contains('active');
            if(ch==='l') polarityL = isInverted ? -1 : 1; else polarityR = isInverted ? -1 : 1;
            if(!document.getElementById(`mute-${ch}`).classList.contains('active') && audioCtx) {
                (ch==='l'?gainL:gainR).gain.value = Math.pow(10, document.getElementById(`gain-${ch}`).value/20) * (ch==='l'?polarityL:polarityR);
            }
        });

        for(let i=0; i<5; i++) {
            bindCtrl(`peq-gain-${ch}-${i}`, v => { (ch==='l'?peqL:peqR)[i].gain.value = v; drawPEQ(ch); });
            bindCtrl(`peq-freq-${ch}-${i}`, v => { (ch==='l'?peqL:peqR)[i].frequency.value = v; drawPEQ(ch); });
            bindCtrl(`peq-q-${ch}-${i}`, v => { (ch==='l'?peqL:peqR)[i].Q.value = v; drawPEQ(ch); });
        }
    });

    bindCtrl('vol-slider', v => { if(audioCtx) masterGain.gain.value = v / 100; });

    function triggerInitialValues() {
        ['l','r'].forEach(ch => {
            ['gain','limit','delay','apf','hpf','lpf'].forEach(p => { document.getElementById(`${p}-${ch}`).dispatchEvent(new Event('input')); });
            for(let i=0; i<5; i++) { ['gain','freq','q'].forEach(p => { document.getElementById(`peq-${p}-${ch}-${i}`).dispatchEvent(new Event('input')); }); }
        });
        document.getElementById('vol-slider').dispatchEvent(new Event('input')); 
    }

    function renderRealtimeGraphics() {
        if (!audioCtx) { requestAnimationFrame(renderRealtimeGraphics); return; }
        
        requestAnimationFrame(renderRealtimeGraphics);
        
        const getDbLevel = (analyser, dataArray) => {
            analyser.getFloatTimeDomainData(dataArray);
            let max = 0;
            for(let i=0; i<dataArray.length; i++){
                let v = Math.abs(dataArray[i]);
                if(v > max) max = v;
            }
            let db = 20 * Math.log10(max || 1e-6);
            let minDb = -60, maxDb = 3;
            let percentage = ((db - minDb) / (maxDb - minDb)) * 100;
            return Math.max(0, Math.min(100, percentage));
        };
        
        let m = getDbLevel(analyserMaster, dataArrM); document.getElementById('vu-master-bar').style.width = m + '%';
        let l = getDbLevel(analyserL, dataArrL); document.getElementById('vu-cha-bar').style.width = l + '%';
        let r = getDbLevel(analyserR, dataArrR); document.getElementById('vu-chb-bar').style.width = r + '%';

        if(limiterL.reduction >= 1) document.getElementById('led-limit-l').classList.add('active'); else document.getElementById('led-limit-l').classList.remove('active');
        if(limiterR.reduction >= 1) document.getElementById('led-limit-r').classList.add('active'); else document.getElementById('led-limit-r').classList.remove('active');

        const c = document.getElementById('xover-master');
        if (c && c.offsetParent !== null) { 
            const ctx = c.getContext('2d'); 
            const w = c.width = c.clientWidth, h = c.height = 180;
            
            ctx.fillStyle = '#050608'; ctx.fillRect(0,0,w,h); 
            ctx.strokeStyle = '#1a1c23'; ctx.font = '9px monospace'; ctx.fillStyle='#444';
            [50,100,500,1000,5000,10000].forEach(f => { let x = (Math.log10(f/20)/3)*w; ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,h); ctx.stroke(); ctx.fillText(f>=1000?f/1000+'k':f, x+2, 10); });

            analyserMaster.getByteFrequencyData(freqDataM);
            ctx.fillStyle = 'rgba(255, 170, 0, 0.15)'; 
            for(let i=0; i<w; i+=2) {
                let freq = 20 * Math.pow(1000, (i/w)); 
                let nyquist = audioCtx.sampleRate / 2;
                let bin = Math.floor((freq / nyquist) * freqDataM.length);
                let val = freqDataM[bin] || 0;
                let barHeight = (val / 255) * h;
                ctx.fillRect(i, h - barHeight, 2, barHeight);
            }

            const plot = (hp, lp, color) => {
                totalMag.fill(1.0);
                for(let j=0; j<4; j++) { 
                    hp[j].getFrequencyResponse(freqs, mag, phase); for(let k=0; k<MAX_RENDER; k++) totalMag[k]*=mag[k]; 
                    lp[j].getFrequencyResponse(freqs, mag, phase); for(let k=0; k<MAX_RENDER; k++) totalMag[k]*=mag[k]; 
                }
                ctx.beginPath(); ctx.strokeStyle = color; ctx.lineWidth = 2.5;
                for(let k=0; k<MAX_RENDER; k++) { 
                    let x = (k/MAX_RENDER)*w; let db = 20*Math.log10(totalMag[k] || 1e-6); let y = h/2 - (db * 1.5); 
                    if(k===0) ctx.moveTo(x,y); else ctx.lineTo(x,y); 
                }
                ctx.stroke();
            };
            plot(hpfL, lpfL, 'var(--accent-blue)'); plot(hpfR, lpfR, 'var(--accent-red)');
        }
    }

    function drawPEQ(ch) {
        const c = document.getElementById(`peq-canvas-${ch}`); if(!c || !audioCtx) return;
        const ctx = c.getContext('2d'); const w = c.width = c.clientWidth, h = c.height = 140;
        ctx.fillStyle = '#050608'; ctx.fillRect(0,0,w,h); ctx.strokeStyle = '#1a1c23'; ctx.font = '9px monospace'; ctx.fillStyle='#444';
        [50,100,500,1000,5000,10000].forEach(f => { let x = (Math.log10(f/20)/3)*w; ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,h); ctx.stroke(); ctx.fillText(f>=1000?f/1000+'k':f, x+2, 10); });
        
        totalMag.fill(1.0); let nodes = ch==='l'?peqL:peqR;
        for(let j=0; j<5; j++) { nodes[j].getFrequencyResponse(freqs, mag, phase); for(let i=0; i<MAX_RENDER; i++) totalMag[i]*=mag[i]; }
        ctx.beginPath(); ctx.strokeStyle = ch==='l'?'var(--accent-blue)':'var(--accent-red)'; ctx.lineWidth = 2.5;
        for(let i=0; i<MAX_RENDER; i++) { let x = (i/MAX_RENDER)*w; let db = 20*Math.log10(totalMag[i] || 1e-6); let y = h/2 - (db * 2); if(i===0) ctx.moveTo(x,y); else ctx.lineTo(x,y); }
        ctx.stroke();
    }

    const audioEl = document.getElementById('core-audio');
    
    document.getElementById('audio-upload').addEventListener('change', e => { if(e.target.files.length) audioEl.src = URL.createObjectURL(e.target.files[0]); });
    document.getElementById('btn-play').addEventListener('click', () => { initAudio(); if(audioCtx.state === 'suspended') audioCtx.resume(); audioEl.play(); });
    document.getElementById('btn-stop').addEventListener('click', () => { audioEl.pause(); audioEl.currentTime = 0; });
    
    document.getElementById('btn-live-input').addEventListener('click', async function() {
        initAudio();
        if(this.classList.contains('active')) {
            if(liveSource) liveSource.disconnect();
            if(liveStream) liveStream.getTracks().forEach(t => t.stop());
            this.classList.remove('active');
            this.innerText = "🎙️ Enable Live Line-In";
            this.style.color = "var(--accent-brand)";
        } else {
            try {
                liveStream = await navigator.mediaDevices.getUserMedia({ audio: { echoCancellation: false, noiseSuppression: false, autoGainControl: false } });
                liveSource = audioCtx.createMediaStreamSource(liveStream);
                liveSource.connect(splitter);
                this.classList.add('active');
                this.innerText = "🎙️ Live Line-In (ACTIVE)";
                this.style.color = "#fff";
            } catch (err) {
                alert("Akses input audio ditolak. Pastikan browser mengizinkan penggunaan mikrofon/line-in.");
            }
        }
    });

    const formatTime = s => `${Math.floor(s/60)}:${Math.floor(s%60).toString().padStart(2,'0')}`;
    audioEl.addEventListener('timeupdate', () => {
        if(!audioEl.duration) return;
        document.getElementById('audio-seek').value = (audioEl.currentTime / audioEl.duration) * 100;
        document.getElementById('time-current').innerText = formatTime(audioEl.currentTime);
    });
    audioEl.addEventListener('loadedmetadata', () => {
        document.getElementById('time-total').innerText = formatTime(audioEl.duration);
    });
    document.getElementById('audio-seek').addEventListener('input', e => {
        if(audioEl.duration) audioEl.currentTime = (e.target.value / 100) * audioEl.duration;
    });

    document.getElementById('btn-noise-toggle').addEventListener('click', function() {
        initAudio();
        if(!pinkNoiseNode) {
            pinkNoiseNode = audioCtx.createBufferSource();
            pinkNoiseNode.buffer = createPinkNoiseBuffer();
            pinkNoiseNode.loop = true;
            pinkNoiseNode.connect(oscGain);
            pinkNoiseNode.start();
        }
        let isActive = this.innerText.includes("ON");
        if(isActive) { 
            oscGain.gain.value = 0; 
            this.innerText = "PINK NOISE (OFF)"; 
            this.style.color = "var(--led-red)"; 
            this.style.borderColor = "rgba(255, 23, 68, 0.3)";
            this.style.background = "rgba(255, 23, 68, 0.05)";
        } else { 
            oscGain.gain.value = 1.0; 
            this.innerText = "PINK NOISE (ON)"; 
            this.style.color = "var(--led-green)"; 
            this.style.borderColor = "rgba(0, 230, 118, 0.5)"; 
            this.style.background = "rgba(0, 230, 118, 0.1)";
        }
    });
</script>
</body>
</html>
