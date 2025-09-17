<!DOCTYPE html>
<html lang="tr" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kripto Grafik Analiz Platformu</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/lightweight-charts/dist/lightweight-charts.standalone.production.js"></script>
    <script src="https://unpkg.com/lucide@latest"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            overflow: hidden; /* Sayfanın kaymasını engelle */
        }
        /* Aktif buton stili */
        .interval-btn.active {
            background-color: #3b82f6; /* Tailwind blue-500 */
            color: white;
        }
        .indicator-btn.active {
            background-color: #4f46e5; /* Tailwind indigo-600 */
            color: white;
        }
        /* Tailwind dark mode için temel konfigürasyon */
        .dark .bg-gray-100 { background-color: #1f2937; }
        .dark .bg-white { background-color: #111827; }
        .dark .text-gray-800 { color: #d1d5db; }
        .dark .text-gray-600 { color: #9ca3af; }
        .dark .border-gray-200 { border-color: #374151; }
        .dark select {
            background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' fill='none' viewBox='0 0 20 20'%3e%3cpath stroke='%239ca3af' stroke-linecap='round' stroke-linejoin='round' stroke-width='1.5' d='M6 8l4 4 4-4'/%3e%3c/svg%3e");
        }
    </style>
</head>
<body class="bg-white dark:bg-gray-900 text-gray-800 dark:text-gray-200 transition-colors duration-300">

    <div id="app" class="flex flex-col h-screen">
        <!-- Üst Menü -->
        <header class="flex-shrink-0 bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-700 p-2 z-10">
            <div class="flex items-center justify-between flex-wrap gap-2">
                <div class="flex items-center gap-4">
                    <h1 class="text-xl font-bold text-blue-600 dark:text-blue-500">CryptoChart</h1>
                    <select id="coin-select" class="bg-gray-100 dark:bg-gray-800 border border-gray-300 dark:border-gray-600 rounded-md px-3 py-1.5 text-sm font-medium focus:outline-none focus:ring-2 focus:ring-blue-500">
                        <option value="BTCUSDT">BTC/USDT</option>
                        <option value="ETHUSDT">ETH/USDT</option>
                        <option value="BNBUSDT">BNB/USDT</option>
                        <option value="SOLUSDT">SOL/USDT</option>
                        <option value="XRPUSDT">XRP/USDT</option>
                        <option value="ADAUSDT">ADA/USDT</option>
                    </select>
                </div>
                
                <div id="interval-container" class="flex items-center gap-1 bg-gray-100 dark:bg-gray-800 p-1 rounded-md">
                    <button data-interval="1m" class="interval-btn px-2 py-1 text-sm rounded-md hover:bg-gray-200 dark:hover:bg-gray-700">1m</button>
                    <button data-interval="5m" class="interval-btn px-2 py-1 text-sm rounded-md hover:bg-gray-200 dark:hover:bg-gray-700">5m</button>
                    <button data-interval="15m" class="interval-btn px-2 py-1 text-sm rounded-md hover:bg-gray-200 dark:hover:bg-gray-700">15m</button>
                    <button data-interval="1h" class="interval-btn px-2 py-1 text-sm rounded-md hover:bg-gray-200 dark:hover:bg-gray-700 active">1h</button>
                    <button data-interval="4h" class="interval-btn px-2 py-1 text-sm rounded-md hover:bg-gray-200 dark:hover:bg-gray-700">4h</button>
                    <button data-interval="1d" class="interval-btn px-2 py-1 text-sm rounded-md hover:bg-gray-200 dark:hover:bg-gray-700">1D</button>
                </div>

                <div class="flex items-center gap-4">
                     <div id="indicator-container" class="flex items-center gap-1 bg-gray-100 dark:bg-gray-800 p-1 rounded-md">
                        <button data-indicator="SMA" data-period="20" class="indicator-btn px-2 py-1 text-sm rounded-md hover:bg-indigo-500 dark:hover:bg-indigo-500">SMA(20)</button>
                        <button data-indicator="EMA" data-period="50" class="indicator-btn px-2 py-1 text-sm rounded-md hover:bg-indigo-500 dark:hover:bg-indigo-500">EMA(50)</button>
                    </div>
                    <button id="theme-toggle" class="p-2 rounded-md hover:bg-gray-200 dark:hover:bg-gray-700">
                        <i data-lucide="sun" class="block dark:hidden"></i>
                        <i data-lucide="moon" class="hidden dark:block"></i>
                    </button>
                </div>
            </div>
        </header>

        <!-- Grafik Alanı -->
        <main class="flex-grow relative">
            <div id="chart-container" class="absolute top-0 left-0 w-full h-full"></div>
        </main>
    </div>

    <script>
        // --- DOM Elementleri ---
        const chartContainer = document.getElementById('chart-container');
        const coinSelect = document.getElementById('coin-select');
        const intervalContainer = document.getElementById('interval-container');
        const indicatorContainer = document.getElementById('indicator-container');
        const themeToggleBtn = document.getElementById('theme-toggle');
        const htmlElement = document.documentElement;

        // --- Grafik ve Veri Değişkenleri ---
        let chart;
        let candlestickSeries;
        let volumeSeries;
        let currentSymbol = 'BTCUSDT';
        let currentInterval = '1h';
        let historicalData = [];
        let indicatorSeries = new Map(); // Aktif indikatör serilerini saklamak için

        // --- Tema Ayarları ---
        const lightTheme = {
            chart: {
                layout: {
                    backgroundColor: '#FFFFFF',
                    textColor: '#1f2937',
                },
                grid: {
                    vertLines: { color: '#e5e7eb' },
                    horzLines: { color: '#e5e7eb' },
                },
            },
            series: {
                upColor: '#22c55e',
                downColor: '#ef4444',
                borderDownColor: '#ef4444',
                borderUpColor: '#22c55e',
                wickDownColor: '#ef4444',
                wickUpColor: '#22c55e',
            },
        };

        const darkTheme = {
            chart: {
                layout: {
                    backgroundColor: '#111827',
                    textColor: '#d1d5db',
                },
                grid: {
                    vertLines: { color: '#374151' },
                    horzLines: { color: '#374151' },
                },
            },
            series: {
                upColor: '#22c55e',
                downColor: '#ef4444',
                borderDownColor: '#ef4444',
                borderUpColor: '#22c55e',
                wickDownColor: '#ef4444',
                wickUpColor: '#22c55e',
            },
        };
        
        // --- Başlangıç Fonksiyonları ---

        // Grafik oluşturma ve yapılandırma
        function initializeChart() {
            // DOM elementinin boyutu tarayıcı tarafından hesaplanana kadar bekleme mekanizması.
            // Bu, özellikle yavaş render olan ortamlarda hatayı önler.
            if (!chartContainer || chartContainer.clientWidth === 0 || chartContainer.clientHeight === 0) {
                setTimeout(initializeChart, 50); // 50ms sonra tekrar dene
                return;
            }

            const currentTheme = htmlElement.classList.contains('dark') ? darkTheme : lightTheme;
            chart = LightweightCharts.createChart(chartContainer, {
                ...currentTheme.chart,
                width: chartContainer.clientWidth,
                height: chartContainer.clientHeight,
                timeScale: {
                    timeVisible: true,
                    secondsVisible: false,
                },
                 crosshair: {
                    mode: LightweightCharts.CrosshairMode.Normal,
                },
            });

            candlestickSeries = chart.addCandlestickSeries(currentTheme.series);
            volumeSeries = chart.addHistogramSeries({
                color: '#3866d6',
                priceFormat: {
                    type: 'volume',
                },
                priceScaleId: '', // Hacim göstergesini ayrı bir ölçekte gösterme
            });
            chart.priceScale('').applyOptions({
                scaleMargins: {
                    top: 0.8, // Hacim için alt boşluk
                    bottom: 0,
                },
            });

            // Grafik başarıyla başlatıldıktan sonra veriyi çek.
            fetchCandleData(currentSymbol, currentInterval);
        }
        
        // Binance API'sinden veri çekme
        async function fetchCandleData(symbol, interval) {
            try {
                const response = await fetch(`https://api.binance.com/api/v3/klines?symbol=${symbol}&interval=${interval}&limit=1000`);
                if (!response.ok) {
                    throw new Error(`Veri alınamadı: ${response.status}`);
                }
                const data = await response.json();
                
                // Veriyi Lightweight Charts formatına dönüştür
                const candleData = data.map(d => ({
                    time: d[0] / 1000,
                    open: parseFloat(d[1]),
                    high: parseFloat(d[2]),
                    low: parseFloat(d[3]),
                    close: parseFloat(d[4]),
                }));

                const volumeData = data.map(d => ({
                    time: d[0] / 1000,
                    value: parseFloat(d[5]),
                    color: parseFloat(d[4]) > parseFloat(d[1]) ? 'rgba(34, 197, 94, 0.5)' : 'rgba(239, 68, 68, 0.5)',
                }));

                historicalData = candleData; // İndikatör hesaplamaları için veriyi sakla
                candlestickSeries.setData(candleData);
                volumeSeries.setData(volumeData);
                chart.timeScale().fitContent();
                updateAllIndicators();

            } catch (error) {
                console.error("API Hatası:", error);
            }
        }
        
        // --- İndikatör Hesaplama Fonksiyonları ---

        // Basit Hareketli Ortalama (SMA)
        function calculateSMA(data, period) {
            let smaData = [];
            if (data.length < period) return smaData;
            for (let i = period - 1; i < data.length; i++) {
                let sum = 0;
                for (let j = 0; j < period; j++) {
                    sum += data[i - j].close;
                }
                smaData.push({
                    time: data[i].time,
                    value: sum / period,
                });
            }
            return smaData;
        }

        // Üssel Hareketli Ortalama (EMA)
        function calculateEMA(data, period) {
            let emaData = [];
            if (data.length < period) return emaData;
            let multiplier = 2 / (period + 1);
            // İlk EMA değeri, ilk periyodun SMA'sıdır
            let initialSum = 0;
            for (let i = 0; i < period; i++) {
                initialSum += data[i].close;
            }
            let prevEma = initialSum / period;
            
            emaData.push({ time: data[period - 1].time, value: prevEma });

            for (let i = period; i < data.length; i++) {
                const ema = (data[i].close - prevEma) * multiplier + prevEma;
                emaData.push({ time: data[i].time, value: ema });
                prevEma = ema;
            }
            return emaData;
        }


        // --- Olay Yöneticileri (Event Handlers) ---

        // Tema değiştirme
        function handleThemeToggle() {
            htmlElement.classList.toggle('dark');
            const isDark = htmlElement.classList.contains('dark');
            const newTheme = isDark ? darkTheme : lightTheme;
            chart.applyOptions(newTheme.chart);
            candlestickSeries.applyOptions(newTheme.series);
        }

        // Coin değiştirme
        function handleSymbolChange(event) {
            currentSymbol = event.target.value;
            fetchCandleData(currentSymbol, currentInterval);
        }

        // Zaman aralığı değiştirme
        function handleIntervalChange(event) {
            const target = event.target.closest('.interval-btn');
            if (!target) return;

            // Aktif stil yönetimi
            if (intervalContainer.querySelector('.active')) {
                intervalContainer.querySelector('.active').classList.remove('active');
            }
            target.classList.add('active');

            currentInterval = target.dataset.interval;
            fetchCandleData(currentSymbol, currentInterval);
        }
        
        // İndikatör Ekle/Kaldır
        function handleIndicatorToggle(event) {
            const target = event.target.closest('.indicator-btn');
            if (!target) return;
            
            target.classList.toggle('active');
            updateAllIndicators();
        }
        
        // Tüm aktif indikatörleri güncelle
        function updateAllIndicators() {
            // Önce mevcut tüm indikatör serilerini kaldır
            indicatorSeries.forEach(series => chart.removeSeries(series));
            indicatorSeries.clear();
            
            // "active" olan butonlara göre yeniden oluştur
            indicatorContainer.querySelectorAll('.indicator-btn.active').forEach(button => {
                 const indicatorName = button.dataset.indicator;
                 const period = parseInt(button.dataset.period);
                 const seriesId = `${indicatorName}_${period}`;

                 let indicatorData;
                 if (indicatorName === 'SMA') {
                    indicatorData = calculateSMA(historicalData, period);
                 } else if (indicatorName === 'EMA') {
                    indicatorData = calculateEMA(historicalData, period);
                 }

                 if(indicatorData && indicatorData.length > 0){
                    const color = indicatorName === 'SMA' ? '#f97316' : '#8b5cf6';
                    const lineSeries = chart.addLineSeries({
                        color: color,
                        lineWidth: 2,
                        priceLineVisible: false,
                        lastValueVisible: false,
                        crosshairMarkerVisible: false,
                    });
                    lineSeries.setData(indicatorData);
                    indicatorSeries.set(seriesId, lineSeries);
                 }
            });
        }

        // Pencere yeniden boyutlandırıldığında grafiği ayarla
        function handleResize() {
            if (chart) {
                chart.resize(chartContainer.clientWidth, chartContainer.clientHeight);
            }
        }

        // --- Ana Başlatma ---
        function init() {
            lucide.createIcons(); // Lucide ikonlarını oluştur
            
            // Grafik oluşturma sürecini başlat (bu fonksiyon içinde veri çekme de tetiklenecek)
            initializeChart();
            
            // Olay dinleyicilerini ekle
            themeToggleBtn.addEventListener('click', handleThemeToggle);
            coinSelect.addEventListener('change', handleSymbolChange);
            intervalContainer.addEventListener('click', handleIntervalChange);
            indicatorContainer.addEventListener('click', handleIndicatorToggle);
            window.addEventListener('resize', handleResize);
        }

        // Sayfa tamamen yüklendiğinde başlat
        window.addEventListener('load', init);
    </script>
</body>
</html>

