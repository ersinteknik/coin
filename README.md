<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>TradingView-stil Teknik Analiz - Demo (Düzeltildi)</title>
  <!-- Lightweight Charts (standalone build) -->
  <script src="https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js"></script>
  <style>
    :root{--bg:#0b1220;--panel:#0f1724;--text:#d6e3ff;--muted:#98a3bd}
    [data-theme="light"]{--bg:#f6f8fb;--panel:#ffffff;--text:#0b1220;--muted:#65708a}
    html,body{height:100%;margin:0;font-family:Inter,system-ui,Segoe UI,Roboto,Helvetica,Arial}
    body{background:var(--bg);color:var(--text);}
    .app{display:grid;grid-template-rows:auto 1fr;min-height:100vh}
    header{display:flex;align-items:center;gap:12px;padding:10px 14px;background:linear-gradient(90deg,rgba(255,255,255,0.02),transparent);backdrop-filter:blur(4px)}
    .brand{font-weight:700;font-size:1.1rem}
    .controls{margin-left:auto;display:flex;gap:8px;align-items:center}
    select,input,button{background:var(--panel);color:var(--text);border:1px solid rgba(255,255,255,0.04);padding:6px 8px;border-radius:6px}
    #container{display:grid;grid-template-columns:1fr 320px;gap:12px;padding:12px;flex:1}
    #chart-area{background:var(--panel);border-radius:8px;padding:8px;display:flex;flex-direction:column}
    #chart{flex:1;min-height:420px}
    .sidebar{background:var(--panel);border-radius:8px;padding:12px;color:var(--muted);overflow:auto}
    .indicator{margin-bottom:10px}
    .btn{cursor:pointer}
    footer{padding:8px 12px;font-size:12px;color:var(--muted);text-align:center}
    @media(max-width:900px){#container{grid-template-columns:1fr} .sidebar{order:2}}
    .draw-cursor{cursor:crosshair}
  </style>
</head>
<body>
  <div class="app" id="app" data-theme="dark">
    <header>
      <div class="brand">TradingClone • Teknik Analiz</div>

      <label>
        Coin:
        <input id="symbol" value="BTCUSDT" style="width:120px;text-transform:uppercase" />
      </label>

      <label>
        Zaman Aralığı:
        <select id="interval">
          <option value="1m">1m</option>
          <option value="5m" selected>5m</option>
          <option value="15m">15m</option>
          <option value="1h">1h</option>
          <option value="4h">4h</option>
          <option value="1d">1D</option>
        </select>
      </label>

      <div class="controls">
        <button id="loadBtn" class="btn">Canlı Bağlan</button>
        <button id="pauseBtn" class="btn">Durdur</button>
        <button id="drawBtn" class="btn">Çizim: Trend</button>
        <select id="indicatorSelect">
          <option value="none">Gösterge ekle</option>
          <option value="sma">SMA (20)</option>
          <option value="ema">EMA (20)</option>
          <option value="rsi">RSI (14)</option>
          <option value="macd">MACD (12,26,9)</option>
          <option value="bb">Bollinger (20,2)</option>
        </select>
        <button id="themeBtn" class="btn">Tema Değiştir</button>
      </div>
    </header>

    <div id="container">
      <div id="chart-area">
        <div id="chart"></div>
        <div id="indicatorChart" style="height:120px;margin-top:8px"></div>
      </div>

      <aside class="sidebar">
        <h4>Hızlı Ayarlar</h4>
        <div class="indicator">Canlı veriler Binance WebSocket'inden alınır. (Tarayıcı, GitHub Pages üzerinde çalışır.)</div>
        <div class="indicator">Çizim: `Çizim: Trend` tuşuna bas, grafikte sürükleyerek trend çizin.</div>
        <div class="indicator">Favori: GitHub Pages üzerinde index.html koyup repo->Settings->Pages ile yayınlayın.</div>
        <hr />
        <h4>Mevcut Durum</h4>
        <pre id="status" style="white-space:pre-wrap;color:var(--text)"></pre>
      </aside>
    </div>

    <footer>Demo — GitHub Pages ile uyumlu. Varsayılan: BTCUSDT 5m</footer>
  </div>

  <script>
  // --- Utility & indicators ---
  function sma(values, period){
    const res=[]; for(let i=0;i<values.length;i++){ if(i<period-1){ res.push(null); continue; } let sum=0; for(let j=0;j<period;j++) sum+=values[i-j]; res.push(sum/period); } return res;
  }
  function ema(values, period){
    const res=[]; const k=2/(period+1); let prev=null; for(let i=0;i<values.length;i++){ if(i===0){ res.push(values[0]); prev=values[0]; continue; } const val = (values[i]-prev)*k + prev; res.push(val); prev=val; } return res.map((v,i)=> i < period-1 ? null : v);
  }
  function rsi(values, period=14){
    const gains=[], losses=[], res=[]; let prevAvgGain=0, prevAvgLoss=0; for(let i=0;i<values.length;i++){ if(i===0){ res.push(null); continue; } const diff = values[i]-values[i-1]; gains.push(Math.max(0,diff)); losses.push(Math.max(0,-diff)); if(i < period){ res.push(null); continue; } if(i===period){ const avgGain = gains.slice(0,period).reduce((a,b)=>a+b,0)/period; const avgLoss = losses.slice(0,period).reduce((a,b)=>a+b,0)/period; prevAvgGain = avgGain; prevAvgLoss = avgLoss; const rs = avgLoss===0?100:avgGain/avgLoss; res.push(100 - 100/(1+rs)); continue; } prevAvgGain = (prevAvgGain*(period-1) + gains[gains.length-1])/period; prevAvgLoss = (prevAvgLoss*(period-1) + losses[losses.length-1])/period; const rs = prevAvgLoss===0?100:prevAvgGain/prevAvgLoss; res.push(100 - 100/(1+rs)); } return res;
  }
  function macd(values, fast=12, slow=26, signal=9){
    const emaFast = ema(values, fast).map(v=>v===null?0:v);
    const emaSlow = ema(values, slow).map(v=>v===null?0:v);
    const macdLine = values.map((v,i)=> (emaFast[i]||0) - (emaSlow[i]||0));
    const signalLine = ema(macdLine, signal);
    const hist = macdLine.map((v,i)=> v - (signalLine[i]||0));
    return {macdLine, signalLine, hist};
  }
  function bollinger(values, period=20, mult=2){
    const res=[]; for(let i=0;i<values.length;i++){ if(i < period-1){ res.push({upper:null,mid:null,lower:null}); continue; } const slice = values.slice(i-period+1,i+1); const mean = slice.reduce((a,b)=>a+b,0)/period; const variance = slice.reduce((a,b)=> a + (b-mean)*(b-mean),0)/period; const sd = Math.sqrt(variance); res.push({upper: mean + mult*sd, mid: mean, lower: mean - mult*sd}); } return res;
  }

  // --- Safe creation helpers to handle different lightweight-charts builds ---
  function createCandlestickOrFallback(chart, opts){
    // Some builds/versions may not expose addCandlestickSeries; try common fallbacks.
    if(!chart) throw new Error('Chart nesnesi yok');
    if(typeof chart.addCandlestickSeries === 'function') return chart.addCandlestickSeries(opts);
    if(typeof chart.addBarSeries === 'function') return chart.addBarSeries(opts); // bar is close enough
    if(typeof chart.addOHLCSeries === 'function') return chart.addOHLCSeries(opts);
    // final fallback: line series (will plot close prices only)
    return chart.addLineSeries(opts);
  }

  // --- Chart setup ---
  const chartContainer = document.getElementById('chart');
  const layoutColors = {
    background: { color: getComputedStyle(document.documentElement).getPropertyValue('--bg').trim() || '#0b1220' },
    textColor: getComputedStyle(document.documentElement).getPropertyValue('--text').trim() || '#d6e3ff'
  };

  const chartOptions = {
    layout: layoutColors,
    rightPriceScale: { visible:true, mode:1, autoScale:true },
    timeScale: { timeVisible:true, secondsVisible:false }
  };

  let chart = null;
  try{
    chart = LightweightCharts.createChart(chartContainer, chartOptions);
  }catch(e){
    // If createChart fails, show friendly message in status area later and continue with a safe state
    console.error('createChart hata:', e);
  }

  // Create candle series with fallback
  let candleSeries = null;
  try{
    candleSeries = createCandlestickOrFallback(chart, { priceFormat:{ type:'price' } });
  }catch(e){
    console.warn('Serie oluşturulamadı, fallback uygulanıyor:', e);
    // try a simple line series as last resort
    try{ candleSeries = chart.addLineSeries(); }catch(err){ console.error('Line series de oluşturulamadı', err); }
  }

  let indicatorSeries = null; // could be a series or object { lines: [...] }
  let indicatorSubChart = null;

  const statusEl = document.getElementById('status');
  function setStatus(s){ statusEl.textContent = s; }

  // --- Binance REST historical klines ---
  async function fetchKlines(symbol, interval, limit=500){
    symbol = symbol.toUpperCase();
    const url = `https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=${interval}&limit=${limit}`;
    const res = await fetch(url);
    if(!res.ok) throw new Error('Binance historical fetch failed: ' + res.status);
    const data = await res.json();
    return data.map(d=>({ time: Math.floor(d[0]/1000), open: parseFloat(d[1]), high: parseFloat(d[2]), low: parseFloat(d[3]), close: parseFloat(d[4]), volume: parseFloat(d[5]) }));
  }

  // --- Draw / update ---
  async function loadAndDraw(){
    const sym = document.getElementById('symbol').value.trim().toUpperCase();
    const interval = document.getElementById('interval').value;
    try{
      setStatus(`Geçmiş veriler alınıyor: ${sym} ${interval} ...`);
      const klines = await fetchKlines(sym, interval);
      if(!candleSeries){ setStatus('Grafik serisi oluşturulamadı.'); return; }
      // For line-series fallback we need to convert data accordingly
      const isLineFallback = typeof candleSeries.update === 'function' && typeof candleSeries.setData === 'function' && !('bars' in candleSeries);
      // set data directly
      candleSeries.setData(klines);
      setStatus(`Geçmiş veriler yüklendi. Son: ${new Date(klines[klines.length-1].time*1000).toLocaleString()}`);
      updateIndicators(klines.map(k=>k.close), klines);
    }catch(e){ console.error(e); setStatus('Veri alınamadı: ' + e.message); }
  }

  // Clean up any previous indicator series/subchart
  function clearIndicators(){
    if(indicatorSeries){
      if(Array.isArray(indicatorSeries.lines)){
        indicatorSeries.lines.forEach(s=>{ try{ chart.removeSeries(s); }catch(e){} });
      }else{
        try{ chart.removeSeries(indicatorSeries); }catch(e){}
      }
      indicatorSeries = null;
    }
    // clear subchart container and try to free reference
    const ic = document.getElementById('indicatorChart');
    if(ic){ ic.innerHTML = ''; }
    indicatorSubChart = null;
  }

  function updateIndicators(priceArray, klines){
    const sel = document.getElementById('indicatorSelect').value;
    clearIndicators();

    try{
      if(sel === 'sma'){
        const vals = sma(priceArray,20).map((v,i)=> v===null? null : { time: klines[i].time, value: v }).filter(Boolean);
        indicatorSeries = chart.addLineSeries({ lineWidth: 2 });
        indicatorSeries.setData(vals);
        setStatus('SMA(20) eklendi');
      }else if(sel === 'ema'){
        const vals = ema(priceArray,20).map((v,i)=> v===null? null : { time: klines[i].time, value: v }).filter(Boolean);
        indicatorSeries = chart.addLineSeries({ lineWidth: 2 });
        indicatorSeries.setData(vals);
        setStatus('EMA(20) eklendi');
      }else if(sel === 'rsi'){
        const r = rsi(priceArray,14).map((v,i)=> v===null? null : { time: klines[i].time, value: v }).filter(Boolean);
        // create small subchart inside indicatorChart
        const ic = document.getElementById('indicatorChart'); if(ic) ic.innerHTML='';
        indicatorSubChart = LightweightCharts.createChart(document.getElementById('indicatorChart'), { width: chartContainer.clientWidth, height: 120, layout: layoutColors });
        const line = indicatorSubChart.addLineSeries(); line.setData(r);
        setStatus('RSI(14) eklendi');
      }else if(sel === 'macd'){
        const m = macd(priceArray,12,26,9);
        const histData = m.hist.map((v,i)=> ({ time: klines[i].time, value: v })).filter(d=> !isNaN(d.value));
        const ic = document.getElementById('indicatorChart'); if(ic) ic.innerHTML='';
        indicatorSubChart = LightweightCharts.createChart(document.getElementById('indicatorChart'), { width: chartContainer.clientWidth, height: 120, layout: layoutColors });
        const hist = indicatorSubChart.addHistogramSeries(); hist.setData(histData);
        const macdLine = indicatorSubChart.addLineSeries(); macdLine.setData(m.macdLine.map((v,i)=> ({ time: klines[i].time, value: v })).filter(d=> !isNaN(d.value)));
        indicatorSeries = { lines: [hist, macdLine] };
        setStatus('MACD eklendi');
      }else if(sel === 'bb'){
        const b = bollinger(priceArray,20,2);
        const upper = b.map((v,i)=> v.upper===null? null : { time: klines[i].time, value: v.upper }).filter(Boolean);
        const mid = b.map((v,i)=> v.mid===null? null : { time: klines[i].time, value: v.mid }).filter(Boolean);
        const lower = b.map((v,i)=> v.lower===null? null : { time: klines[i].time, value: v.lower }).filter(Boolean);
        const sUpper = chart.addLineSeries({ lineWidth:1 }); sUpper.setData(upper);
        const sMid = chart.addLineSeries({ lineWidth:1 }); sMid.setData(mid);
        const sLower = chart.addLineSeries({ lineWidth:1 }); sLower.setData(lower);
        indicatorSeries = { lines: [sUpper, sMid, sLower] };
        setStatus('Bollinger(20,2) eklendi');
      }else{
        setStatus('Gösterge kaldırıldı');
      }
    }catch(e){ console.error(e); setStatus('Indikatör eklenirken hata: ' + e.message); }
  }

  // --- WebSocket live updates (Binance) ---
  let ws = null; let wsAlive = false;
  function startWebsocket(symbol, interval){
    stopWebsocket();
    try{
      const lc = symbol.toLowerCase(); const stream = `${lc}@kline_${interval}`; const url = `wss://stream.binance.com:9443/ws/${stream}`;
      ws = new WebSocket(url);
      ws.onopen = ()=>{ wsAlive=true; setStatus('WS Bağlandı: '+symbol+' '+interval); };
      ws.onmessage = (evt)=>{
        try{
          const msg = JSON.parse(evt.data);
          if(!msg.k) return;
          const k = msg.k;
          const candle = { time: Math.floor(k.t/1000), open: parseFloat(k.o), high: parseFloat(k.h), low: parseFloat(k.l), close: parseFloat(k.c), volume: parseFloat(k.v) };
          if(candleSeries && typeof candleSeries.update === 'function') candleSeries.update(candle);
        }catch(e){ console.warn('WS mesajı işlenirken hata', e); }
      };
      ws.onclose = ()=>{ wsAlive=false; setStatus('WS bağlantısı kapandı'); };
      ws.onerror = (e)=>{ setStatus('WS hata'); };
    }catch(e){ setStatus('WS başlatılamadı: ' + e.message); }
  }
  function stopWebsocket(){ if(ws){ try{ ws.close(); }catch(e){} ws=null; wsAlive=false; } }

  // --- Controls ---
  document.getElementById('loadBtn').addEventListener('click', async ()=>{
    await loadAndDraw();
    const sym = document.getElementById('symbol').value.trim().toUpperCase();
    const interval = document.getElementById('interval').value;
    startWebsocket(sym, interval);
  });
  document.getElementById('pauseBtn').addEventListener('click', ()=>{ stopWebsocket(); setStatus('Canlı durduruldu'); });
  document.getElementById('indicatorSelect').addEventListener('change', async ()=>{
    const sym = document.getElementById('symbol').value.trim().toUpperCase();
    const interval = document.getElementById('interval').value;
    fetchKlines(sym, interval).then(klines => updateIndicators(klines.map(k=>k.close), klines)).catch(e=>setStatus('Indikatör için veri alınamadı'));
  });

  // theme toggle
  document.getElementById('themeBtn').addEventListener('click', ()=>{
    const app = document.getElementById('app'); const cur = app.getAttribute('data-theme'); const next = cur === 'dark'? 'light':'dark'; app.setAttribute('data-theme', next);
    const layout = { background: { color: getComputedStyle(document.documentElement).getPropertyValue('--bg').trim() }, textColor: getComputedStyle(document.documentElement).getPropertyValue('--text').trim() };
    try{ chart.applyOptions({ layout }); }catch(e){ console.warn('Tema uygulama hatası', e); }
  });

  // drawing (best-effort; some builds may not provide coordinate helpers)
  let drawing=false, drawStart=null, drawSeries=null;
  document.getElementById('drawBtn').addEventListener('click', ()=>{ drawing=!drawing; document.getElementById('drawBtn').textContent = drawing? 'Çizim: ON':'Çizim: Trend'; chartContainer.classList.toggle('draw-cursor', drawing); });

  function getTimeFromPixel(x){
    try{ if(chart && chart.timeScale && typeof chart.timeScale().coordinateToTime === 'function') return chart.timeScale().coordinateToTime(x); }
    catch(e){}
    try{ if(chart && chart.timeScale && typeof chart.timeScale().coordinateToLogical === 'function') return chart.timeScale().coordinateToLogical(x); }catch(e){}
    return null;
  }
  function getPriceFromPixel(y){
    try{ if(chart && chart.priceScale && typeof chart.priceScale === 'function' && typeof chart.priceScale('right').coordinateToPrice === 'function') return chart.priceScale('right').coordinateToPrice(y); }
    catch(e){}
    try{ if(chart && chart.priceScale && typeof chart.priceScale === 'object' && typeof chart.priceScale.coordinateToPrice === 'function') return chart.priceScale.coordinateToPrice(y); }catch(e){}
    return null;
  }

  chartContainer.addEventListener('pointerdown', (e)=>{
    if(!drawing) return; const rect = chartContainer.getBoundingClientRect(); const x = e.clientX - rect.left; const y = e.clientY - rect.top; const time = getTimeFromPixel(x); const price = getPriceFromPixel(y); if(time==null || price==null){ setStatus('Çizim desteklenmiyor (kullanılan kütüphane koordinat desteği yok).'); drawing=false; document.getElementById('drawBtn').textContent='Çizim: Trend'; chartContainer.classList.remove('draw-cursor'); return; }
    drawStart = { time: Math.floor(time), price }; drawSeries = chart.addLineSeries({ color: 'rgba(255,165,0,0.9)', lineWidth: 2 }); drawSeries.setData([ { time: drawStart.time, value: drawStart.price } ]);
  });
  chartContainer.addEventListener('pointerup', (e)=>{
    if(!drawing || !drawStart) return; const rect = chartContainer.getBoundingClientRect(); const x = e.clientX - rect.left; const y = e.clientY - rect.top; const time = getTimeFromPixel(x); const price = getPriceFromPixel(y); if(time==null || price==null){ setStatus('Çizim tamamlanamadı'); drawStart=null; drawing=false; return; }
    const pts = [ { time: drawStart.time, value: drawStart.price }, { time: Math.floor(time), value: price } ]; try{ drawSeries.setData(pts); }catch(e){ console.warn('Çizim serisi güncellenemedi', e); }
    drawStart=null; drawSeries=null; drawing=false; document.getElementById('drawBtn').textContent='Çizim: Trend'; chartContainer.classList.remove('draw-cursor');
  });

  // initial load
  (async ()=>{ try{ await loadAndDraw(); }catch(e){ console.error(e); setStatus('Başlangıçta veri alınamadı — "Canlı Bağlan" butonuna basın.'); } })();

  // Resize handling (best-effort)
  window.addEventListener('resize', ()=>{ try{ if(typeof chart.resize === 'function') chart.resize(chartContainer.clientWidth, chartContainer.clientHeight-140); if(indicatorSubChart && typeof indicatorSubChart.resize === 'function') indicatorSubChart.resize(chartContainer.clientWidth, 120); }catch(e){ console.warn('Resize hatası', e); } });

  </script>
</body>
</html>
