<!DOCTYPE html>
<html lang="tr" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ErsinTeknik - Teknik Analiz Platformu</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Lightweight Charts by TradingView (Pinned to a specific version for stability) -->
    <script src="https://unpkg.com/lightweight-charts@4.0.1/dist/lightweight-charts.standalone.production.js"></script>

    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

    <style>
        html, body {
            height: 100%;
            margin: 0;
            padding: 0;
            overflow: hidden;
        }
        body {
            font-family: 'Inter', sans-serif;
            background-color: #131722;
            color: #d1d4dc;
        }
        .tool-button.active { background-color: #2962FF; color: white; }
        .timeframe-item.active-timeframe { background-color: #2962FF; color: white; }
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: #1e222d; }
        ::-webkit-scrollbar-thumb { background: #4a4e59; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #5a5e69; }
    </style>
</head>
<body class="flex flex-col h-full">

    <!-- Header -->
    <header class="flex items-center justify-between p-2 border-b border-gray-700 flex-shrink-0">
        <div class="flex items-center gap-4">
            <h1 class="text-xl font-bold text-white">ErsinTeknik</h1>
        </div>
        <div class="flex items-center gap-2">
            <button class="px-3 py-1.5 text-sm font-medium hover:bg-gray-700 rounded-md">Giriş Yap</button>
            <button class="px-3 py-1.5 text-sm font-medium bg-blue-600 hover:bg-blue-700 text-white rounded-md">Kayıt Ol</button>
            <button class="p-2 hover:bg-gray-700 rounded-md"><i data-lucide="user-circle-2" class="w-5 h-5"></i></button>
        </div>
    </header>

    <!-- Main Content -->
    <div class="flex flex-1 overflow-hidden">
        
        <!-- Sol Araç Çubuğu -->
        <nav class="flex flex-col items-center gap-1 p-2 border-r border-gray-700 bg-gray-900/30">
            <button class="tool-button p-2 rounded-md hover:bg-gray-700 active" title="İmleç"><i data-lucide="mouse-pointer-2" class="w-5 h-5"></i></button>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Trend Çizgisi"><i data-lucide="trending-up" class="w-5 h-5"></i></button>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Yatay Çizgi"><i data-lucide="minus" class="w-5 h-5"></i></button>
            <div class="flex-1"></div>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Ayarlar"><i data-lucide="settings-2" class="w-5 h-5"></i></button>
        </nav>

        <!-- Grafik ve Kontroller -->
        <main class="flex-1 flex flex-col min-w-0">
            <!-- Üst Kontrol Çubuğu -->
            <div class="flex items-center gap-2 p-2 border-b border-gray-700">
                <span id="symbol-name" class="font-bold text-lg">BTCUSDT</span>
                <div class="h-6 border-l border-gray-700 mx-2"></div>
                <!-- Zaman Aralığı Dropdown -->
                <div class="relative">
                    <button id="timeframe-button" class="flex items-center gap-1.5 px-2.5 py-1 text-sm rounded-md hover:bg-gray-700 bg-gray-800">
                        <span id="selected-timeframe">1D</span>
                        <i data-lucide="chevron-down" class="w-4 h-4"></i>
                    </button>
                    <div id="timeframe-dropdown" class="absolute left-0 mt-2 w-32 bg-gray-800 border border-gray-700 rounded-md shadow-lg z-10 hidden">
                        <a href="#" class="block px-4 py-2 text-sm text-gray-300 hover:bg-gray-700 timeframe-item" data-timeframe="15m">15 Dakika</a>
                        <a href="#" class="block px-4 py-2 text-sm text-gray-300 hover:bg-gray-700 timeframe-item" data-timeframe="1h">1 Saat</a>
                        <a href="#" class="block px-4 py-2 text-sm text-gray-300 hover:bg-gray-700 timeframe-item" data-timeframe="4h">4 Saat</a>
                        <a href="#" class="block px-4 py-2 text-sm text-gray-300 hover:bg-gray-700 timeframe-item active-timeframe" data-timeframe="1d">1 Gün</a>
                    </div>
                </div>
                <!-- Göstergeler Dropdown -->
                <div class="relative">
                    <button id="indicators-button" class="flex items-center gap-1.5 px-2.5 py-1 text-sm rounded-md hover:bg-gray-700">
                        <i data-lucide="flask-conical" class="w-4 h-4"></i>
                        Göstergeler
                    </button>
                    <div id="indicators-dropdown" class="absolute left-0 mt-2 w-56 bg-gray-800 border border-gray-700 rounded-md shadow-lg z-10 hidden">
                        <a href="#" data-indicator-id="rsi" class="indicator-item flex items-center justify-between px-4 py-2 text-sm text-gray-300 hover:bg-gray-700">
                            <span>Göreceli Güç Endeksi (RSI)</span>
                            <i data-lucide="check" class="w-4 h-4 text-blue-500 invisible"></i>
                        </a>
                        <a href="#" data-indicator-id="ma" class="indicator-item flex items-center justify-between px-4 py-2 text-sm text-gray-300 hover:bg-gray-700">
                            <span>Hareketli Ortalama (MA)</span>
                            <i data-lucide="check" class="w-4 h-4 text-blue-500 invisible"></i>
                        </a>
                    </div>
                </div>
                <div class="flex-1"></div>
                <button id="toggle-watchlist-button" class="p-2 hover:bg-gray-700 rounded-md border-l border-gray-700 ml-2 pl-4" title="İzleme Listesini Gizle">
                    <i data-lucide="panel-right-close" class="w-5 h-5"></i>
                </button>
            </div>
            <!-- Chart Container Wrapper -->
            <div id="chart-wrapper" class="flex-1 w-full h-full flex flex-col">
                <div id="chart-container" class="flex-1 w-full h-full"></div>
                <div id="rsi-chart-container" class="w-full h-1/3 border-t-2 border-gray-700 hidden"></div>
            </div>
        </main>

        <!-- Sağ Panel -->
        <aside id="watchlist-panel" class="w-72 border-l border-gray-700 flex flex-col">
            <div class="flex justify-between items-center p-3 border-b border-gray-700">
                <h2 class="text-lg font-semibold">İzleme Listesi</h2>
                <button id="add-symbol-button" class="p-1 rounded-md hover:bg-gray-700" title="İzleme Listesine Ekle">
                    <i data-lucide="plus" class="w-5 h-5"></i>
                </button>
            </div>
            <div class="flex-1 overflow-y-auto"><ul id="watchlist"></ul></div>
        </aside>
    </div>

    <!-- Sembol Ekleme Modalı -->
    <div id="add-symbol-modal" class="fixed inset-0 bg-black bg-opacity-70 flex items-center justify-center z-50 hidden">
        <div class="bg-gray-800 rounded-lg shadow-xl w-full max-w-md p-6">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-semibold">Sembol Ekle</h3>
                <button id="close-modal-button" class="p-1 rounded-full hover:bg-gray-700"><i data-lucide="x" class="w-5 h-5"></i></button>
            </div>
            <div>
                <div class="relative mb-4">
                     <i data-lucide="search" class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400"></i>
                    <input type="text" id="search-symbol-input" placeholder="Arama yap..." class="w-full bg-gray-900 border border-gray-700 rounded-md pl-9 pr-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500">
                </div>
                <div id="symbol-results-list" class="max-h-80 overflow-y-auto"></div>
            </div>
        </div>
    </div>


    <script>
        window.addEventListener('load', () => {
            lucide.createIcons();

            // --- GLOBAL DEĞİŞKENLER ---
            let chart;
            let candlestickSeries;
            let currentCandleData = [];
            let watchlistData = ['BTCUSDT', 'ETHUSDT', 'SOLUSDT', 'XRPUSDT'];
            let allSymbols = [];
            let binanceSocket;
            // Pane (alt panel) göstergeleri için referanslar
            let rsiChart;

            // --- MERKEZİ GÖSTERGE YÖNETİCİSİ ---
            const IndicatorManager = {
                library: {},
                activeIndicators: {},

                register(indicator) { this.library[indicator.id] = indicator; },

                toggle(indicatorId) {
                    const button = document.querySelector(`[data-indicator-id="${indicatorId}"]`);
                    const checkIcon = button.querySelector('i, svg');
                    
                    if (this.activeIndicators[indicatorId]) {
                        this.remove(indicatorId);
                        checkIcon.classList.add('invisible');
                    } else {
                        this.add(indicatorId);
                        checkIcon.classList.remove('invisible');
                    }
                },

                add(indicatorId) {
                    const indicator = this.library[indicatorId];
                    if (!indicator) return;

                    const series = indicator.init(chart);
                    const data = indicator.calculate(currentCandleData);
                    indicator.update(series, data);
                    this.activeIndicators[indicatorId] = { series, definition: indicator };
                },

                remove(indicatorId) {
                    const active = this.activeIndicators[indicatorId];
                    if (!active) return;
                    active.definition.remove(chart, active.series);
                    delete this.activeIndicators[indicatorId];
                },
                
                updateAllActive() {
                    for (const id in this.activeIndicators) {
                        const active = this.activeIndicators[id];
                        const newData = active.definition.calculate(currentCandleData);
                        active.definition.update(active.series, newData);
                    }
                }
            };

            // --- GÖSTERGE TANIMLAMALARI ---
            
            // 1. Hareketli Ortalama (MA)
            const maIndicator = {
                id: 'ma',
                name: 'Hareketli Ortalama (MA)',
                type: 'overlay', // Ana grafik üzerinde
                init: (chart) => chart.addLineSeries({
                    color: 'rgba(255, 165, 0, 0.8)',
                    lineWidth: 2,
                    lastValueVisible: false,
                    priceLineVisible: false,
                }),
                calculate: (data, settings = { period: 20 }) => {
                    const result = [];
                    for (let i = settings.period - 1; i < data.length; i++) {
                        let sum = 0;
                        for (let j = 0; j < settings.period; j++) sum += data[i - j].close;
                        result.push({ time: data[i].time, value: sum / settings.period });
                    }
                    return result;
                },
                update: (series, data) => series.setData(data),
                remove: (chart, series) => chart.removeSeries(series),
            };

            // 2. Göreceli Güç Endeksi (RSI)
            const rsiIndicator = {
                id: 'rsi',
                name: 'Göreceli Güç Endeksi (RSI)',
                type: 'pane', // Ayrı panelde
                init: (mainChart) => {
                    const rsiContainer = document.getElementById('rsi-chart-container');
                    rsiContainer.classList.remove('hidden');
                    
                    if (!rsiChart) {
                        rsiChart = LightweightCharts.createChart(rsiContainer, {
                            layout: { background: { color: '#131722' }, textColor: '#d1d4dc' },
                            grid: { vertLines: { visible: false }, horzLines: { color: 'rgba(255,255,255,0.1)' } },
                            timeScale: { visible: false },
                        });
                        mainChart.timeScale().subscribeVisibleTimeRangeChange(timeRange => {
                            if(timeRange && rsiChart) rsiChart.timeScale().setVisibleRange(timeRange);
                        });
                    }
                    
                    const rsiSeries = rsiChart.addLineSeries({ color: '#c771d2', lineWidth: 2 });
                    rsiSeries.createPriceLine({ price: 70, color: '#ef5350', lineWidth: 1, lineStyle: LightweightCharts.LineStyle.Dashed, axisLabelVisible: true, title: '70' });
                    rsiSeries.createPriceLine({ price: 30, color: '#26a69a', lineWidth: 1, lineStyle: LightweightCharts.LineStyle.Dashed, axisLabelVisible: true, title: '30' });
                    
                    requestAnimationFrame(() => window.dispatchEvent(new Event('resize')));
                    return rsiSeries;
                },
                calculate: (data, settings = { period: 14 }) => {
                    let result = [];
                    let changes = data.map((d, i) => i > 0 ? d.close - data[i - 1].close : 0);
                    let gains = changes.map(c => c > 0 ? c : 0);
                    let losses = changes.map(c => c < 0 ? Math.abs(c) : 0);
                    let avgGain = 0, avgLoss = 0;

                    for (let i = 0; i < data.length; i++) {
                        if (i < settings.period) {
                            result.push({ time: data[i].time, value: undefined });
                            avgGain += gains[i];
                            avgLoss += losses[i];
                            if (i === settings.period - 1) {
                                avgGain /= settings.period;
                                avgLoss /= settings.period;
                            }
                        } else {
                            avgGain = (avgGain * (settings.period - 1) + gains[i]) / settings.period;
                            avgLoss = (avgLoss * (settings.period - 1) + losses[i]) / settings.period;
                            if (avgLoss === 0) {
                                result.push({ time: data[i].time, value: 100 });
                            } else {
                                const rs = avgGain / avgLoss;
                                result.push({ time: data[i].time, value: 100 - (100 / (1 + rs)) });
                            }
                        }
                    }
                    return result;
                },
                update: (series, data) => series.setData(data),
                remove: (chart, series) => {
                    const rsiContainer = document.getElementById('rsi-chart-container');
                    rsiContainer.classList.add('hidden');
                    if (rsiChart) {
                        rsiChart.remove(); // Grafik nesnesini tamamen kaldır
                        rsiChart = null;   // Referansı temizle
                    }
                    requestAnimationFrame(() => window.dispatchEvent(new Event('resize')));
                },
            };

            // GÖSTERGELERİ KÜTÜPHANEYE KAYDET
            IndicatorManager.register(maIndicator);
            IndicatorManager.register(rsiIndicator);


            // --- GRAFİK YÖNETİMİ ---
            function initializeChart() {
                const chartContainer = document.getElementById('chart-container');
                if (!chartContainer || chartContainer.clientWidth === 0) {
                    setTimeout(initializeChart, 100);
                    return;
                }

                chart = LightweightCharts.createChart(chartContainer, {
                    layout: { background: { color: '#131722' }, textColor: '#d1d4dc' },
                    grid: { vertLines: { visible: false }, horzLines: { visible: false } },
                    timeScale: { borderColor: '#485c7b', timeVisible: true },
                });

                candlestickSeries = chart.addCandlestickSeries({
                    upColor: '#26a69a', downColor: '#ef5350', borderDownColor: '#ef5350',
                    borderUpColor: '#26a69a', wickDownColor: '#ef5350', wickUpColor: '#26a69a',
                });
                
                window.addEventListener('resize', () => {
                    chart.resize(chartContainer.clientWidth, chartContainer.clientHeight);
                    if(rsiChart) {
                        const rsiContainer = document.getElementById('rsi-chart-container');
                        rsiChart.resize(rsiContainer.clientWidth, rsiContainer.clientHeight);
                    }
                });
                
                updateChartForSymbol('BTCUSDT', '1d');
            }

            async function updateChartForSymbol(symbol, interval) {
                document.getElementById('symbol-name').textContent = symbol;
                try {
                    const response = await fetch(`https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=${interval}&limit=300`);
                    const data = await response.json();
                    
                    currentCandleData = data.map(d => ({
                        time: d[0] / 1000, open: parseFloat(d[1]), high: parseFloat(d[2]),
                        low: parseFloat(d[3]), close: parseFloat(d[4]),
                    }));

                    candlestickSeries.setData(currentCandleData);
                    IndicatorManager.updateAllActive();
                } catch (error) {
                    console.error('Grafik verisi alınamadı:', error);
                }
            }
            
            // --- İZLEME LİSTESİ YÖNETİMİ ---
            const watchlistEl = document.getElementById('watchlist');
            function renderWatchlist() {
                watchlistEl.innerHTML = ''; 
                watchlistData.forEach(symbol => {
                    const li = document.createElement('li');
                    li.id = `watchlist-${symbol}`;
                    li.className = 'flex justify-between items-center p-3 hover:bg-gray-800 cursor-pointer border-b border-gray-800';
                    li.innerHTML = `
                        <div><span class="font-medium">${symbol}</span></div>
                        <div class="text-right">
                            <span id="price-${symbol}" class="block">...</span>
                            <span id="change-${symbol}" class="block">...</span>
                        </div>`;
                    li.addEventListener('click', () => {
                        const currentInterval = document.querySelector('.timeframe-item.active-timeframe').dataset.timeframe;
                        updateChartForSymbol(symbol, currentInterval);
                    });
                    watchlistEl.appendChild(li);
                });
                subscribeToWatchlistStreams();
            }
            
            function subscribeToWatchlistStreams() {
                if (binanceSocket) binanceSocket.close();
                if (watchlistData.length === 0) return;

                const streams = watchlistData.map(s => `${s.toLowerCase()}@ticker`).join('/');
                binanceSocket = new WebSocket(`wss://stream.binance.com:9443/ws/${streams}`);

                binanceSocket.onmessage = function(event) {
                    const message = JSON.parse(event.data);
                    const symbol = message.s;
                    const priceEl = document.getElementById(`price-${symbol}`);
                    const changeEl = document.getElementById(`change-${symbol}`);

                    if (priceEl && changeEl) {
                        const price = parseFloat(message.c).toFixed(symbol === 'SHIBUSDT' ? 8 : 4);
                        const changePercent = parseFloat(message.P);
                        const isPositive = changePercent >= 0;

                        priceEl.textContent = price;
                        changeEl.textContent = `${isPositive ? '+' : ''}${changePercent.toFixed(2)}%`;
                        changeEl.className = `block ${isPositive ? 'text-green-500' : 'text-red-500'}`;
                    }
                }
            }

            // --- SEMBOL EKLEME MODALI YÖNETİMİ ---
            const modal = document.getElementById('add-symbol-modal');
            async function fetchAllSymbols() {
                try {
                    const response = await fetch('https://api.binance.com/api/v3/exchangeInfo');
                    const data = await response.json();
                    allSymbols = data.symbols
                        .filter(s => s.quoteAsset === 'USDT' && s.status === 'TRADING')
                        .map(s => s.symbol);
                } catch (error) { console.error("Sembol listesi alınamadı:", error); }
            }
            function renderAvailableSymbols(filter = '') {
                const resultsList = document.getElementById('symbol-results-list');
                const filtered = allSymbols.filter(s => s.toLowerCase().includes(filter.toLowerCase()));
                resultsList.innerHTML = '';
                filtered.slice(0, 100).forEach(symbol => {
                    const item = document.createElement('div');
                    item.className = 'p-3 hover:bg-gray-700 cursor-pointer rounded-md';
                    item.textContent = symbol;
                    item.addEventListener('click', () => {
                        if (!watchlistData.includes(symbol)) {
                            watchlistData.push(symbol);
                            renderWatchlist();
                        }
                        modal.classList.add('hidden');
                    });
                    resultsList.appendChild(item);
                });
            }
            
            // --- EVENT LISTENERS ---
            document.getElementById('add-symbol-button').addEventListener('click', () => {
                modal.classList.remove('hidden');
                document.getElementById('search-symbol-input').value = '';
                renderAvailableSymbols();
                document.getElementById('search-symbol-input').focus();
            });
            document.getElementById('close-modal-button').addEventListener('click', () => modal.classList.add('hidden'));
            modal.addEventListener('click', e => { if (e.target === modal) modal.classList.add('hidden'); });
            document.getElementById('search-symbol-input').addEventListener('input', (e) => renderAvailableSymbols(e.target.value));

            document.getElementById('toggle-watchlist-button').addEventListener('click', (e) => {
                const panel = document.getElementById('watchlist-panel');
                const isHidden = panel.classList.toggle('hidden');
                const button = e.currentTarget;
                button.innerHTML = `<i data-lucide="${isHidden ? 'panel-right-open' : 'panel-right-close'}" class="w-5 h-5"></i>`;
                button.title = isHidden ? 'İzleme Listesini Göster' : 'İzleme Listesini Gizle';
                lucide.createIcons();
                requestAnimationFrame(() => window.dispatchEvent(new Event('resize')));
            });

            const timeframeDropdown = document.getElementById('timeframe-dropdown');
            document.getElementById('timeframe-button').addEventListener('click', e => {
                e.stopPropagation();
                timeframeDropdown.classList.toggle('hidden');
                document.getElementById('indicators-dropdown').classList.add('hidden');
            });
            document.querySelectorAll('.timeframe-item').forEach(item => {
                item.addEventListener('click', e => {
                    e.preventDefault();
                    document.querySelector('.timeframe-item.active-timeframe').classList.remove('active-timeframe');
                    item.classList.add('active-timeframe');
                    document.getElementById('selected-timeframe').textContent = item.dataset.timeframe.toUpperCase();
                    timeframeDropdown.classList.add('hidden');
                    updateChartForSymbol(document.getElementById('symbol-name').textContent, item.dataset.timeframe);
                });
            });

            const indicatorsDropdown = document.getElementById('indicators-dropdown');
            document.getElementById('indicators-button').addEventListener('click', e => {
                e.stopPropagation();
                indicatorsDropdown.classList.toggle('hidden');
                timeframeDropdown.classList.add('hidden');
            });

            // YENİ GÖSTERGE EVENT LISTENER
            document.querySelectorAll('.indicator-item').forEach(button => {
                button.addEventListener('click', (e) => {
                    e.preventDefault();
                    IndicatorManager.toggle(button.dataset.indicatorId);
                    indicatorsDropdown.classList.add('hidden');
                });
            });

            window.addEventListener('click', () => {
                timeframeDropdown.classList.add('hidden');
                indicatorsDropdown.classList.add('hidden');
            });

            // --- UYGULAMAYI BAŞLAT ---
            initializeChart();
            renderWatchlist();
            fetchAllSymbols();
        });
    </script>
</body>
</html>

