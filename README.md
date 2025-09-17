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
        body {
            font-family: 'Inter', sans-serif;
            background-color: #131722;
            color: #d1d4dc;
        }

        /* Aktif araç butonu stili */
        .tool-button.active {
            background-color: #2962FF;
            color: white;
        }
        
        /* Custom scrollbar stilleri */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1e222d;
        }
        ::-webkit-scrollbar-thumb {
            background: #4a4e59;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #5a5e69;
        }
    </style>
</head>
<body class="flex flex-col h-screen overflow-hidden">

    <!-- Header -->
    <header class="flex items-center justify-between p-2 border-b border-gray-700">
        <div class="flex items-center gap-4">
            <h1 class="text-xl font-bold text-white">ErsinTeknik</h1>
            <div class="relative">
                <i data-lucide="search" class="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400"></i>
                <input type="text" placeholder="Sembol Ara..." class="bg-gray-800 border border-gray-700 rounded-md pl-9 pr-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-blue-500 w-48">
            </div>
        </div>
        <div class="flex items-center gap-2">
            <button class="px-3 py-1.5 text-sm font-medium hover:bg-gray-700 rounded-md">Giriş Yap</button>
            <button class="px-3 py-1.5 text-sm font-medium bg-blue-600 hover:bg-blue-700 text-white rounded-md">Kayıt Ol</button>
            <button class="p-2 hover:bg-gray-700 rounded-md">
                <i data-lucide="user-circle-2" class="w-5 h-5"></i>
            </button>
        </div>
    </header>

    <!-- Main Content -->
    <div class="flex flex-1 overflow-hidden">
        
        <!-- Sol Araç Çubuğu -->
        <nav class="flex flex-col items-center gap-1 p-2 border-r border-gray-700 bg-gray-900/30">
            <button class="tool-button p-2 rounded-md hover:bg-gray-700 active" title="İmleç">
                <i data-lucide="mouse-pointer-2" class="w-5 h-5"></i>
            </button>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Trend Çizgisi">
                <i data-lucide="trending-up" class="w-5 h-5"></i>
            </button>
             <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Yatay Çizgi">
                <i data-lucide="minus" class="w-5 h-5"></i>
            </button>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Fibonacci Düzeltmesi">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M3 3v18h18"/><path d="M3 12h18"/><path d="M3 16h18"/><path d="M3 8h18"/><path d="M3 5h18"/></svg>
            </button>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Fırça">
                <i data-lucide="paintbrush-2" class="w-5 h-5"></i>
            </button>
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Metin">
                <i data-lucide="type" class="w-5 h-5"></i>
            </button>
            <div class="flex-1"></div> <!-- Spacer -->
            <button class="tool-button p-2 rounded-md hover:bg-gray-700" title="Ayarlar">
                <i data-lucide="settings-2" class="w-5 h-5"></i>
            </button>
        </nav>

        <!-- Grafik ve Kontroller -->
        <main class="flex-1 flex flex-col">
            <!-- Üst Kontrol Çubuğu -->
            <div class="flex items-center gap-2 p-2 border-b border-gray-700">
                <span id="symbol-name" class="font-bold text-lg">BTC/USDT</span>
                <div class="h-6 border-l border-gray-700 mx-2"></div>
                <button class="px-2.5 py-1 text-sm rounded-md hover:bg-gray-700 bg-gray-800">1D</button>
                <button class="px-2.5 py-1 text-sm rounded-md hover:bg-gray-700">4H</button>
                <button class="px-2.5 py-1 text-sm rounded-md hover:bg-gray-700">1H</button>
                <button class="px-2.5 py-1 text-sm rounded-md hover:bg-gray-700">15m</button>
                <button class="flex items-center gap-1.5 px-2.5 py-1 text-sm rounded-md hover:bg-gray-700">
                    <i data-lucide="bar-chart-3" class="w-4 h-4"></i>
                    Mum
                </button>
                <button class="flex items-center gap-1.5 px-2.5 py-1 text-sm rounded-md hover:bg-gray-700">
                    <i data-lucide="flask-conical" class="w-4 h-4"></i>
                    Göstergeler
                </button>
                <div class="flex-1"></div>
                <button class="p-2 hover:bg-gray-700 rounded-md">
                     <i data-lucide="camera" class="w-5 h-5"></i>
                </button>
                <button class="p-2 hover:bg-gray-700 rounded-md">
                    <i data-lucide="maximize" class="w-5 h-5"></i>
                </button>
            </div>
            <!-- Chart Container -->
            <div id="chart-container" class="flex-1 w-full h-full"></div>
        </main>

        <!-- Sağ Panel -->
        <aside class="w-72 border-l border-gray-700 flex flex-col">
            <div class="p-3 border-b border-gray-700">
                <h2 class="text-lg font-semibold">İzleme Listesi</h2>
            </div>
            <div class="flex-1 overflow-y-auto">
                <ul id="watchlist">
                    <!-- Watchlist items will be dynamically inserted here -->
                </ul>
            </div>
            <div class="p-3 border-t border-gray-700">
                <h3 class="font-semibold" id="asset-title">Bitcoin (BTC/USDT)</h3>
                <div class="flex justify-between items-end mt-2">
                    <span class="text-2xl font-bold text-green-500" id="asset-price">68,450.75</span>
                    <span class="text-md font-medium text-green-500" id="asset-change">+1,230.15 (+1.83%)</span>
                </div>
            </div>
        </aside>

    </div>

    <script>
        window.addEventListener('load', () => {
            lucide.createIcons();

            function initializeChart() {
                const chartContainer = document.getElementById('chart-container');
                
                // Kütüphanenin yüklenip yüklenmediğini ve konteynerin boyutlarının hazır olup olmadığını kontrol et
                if (typeof LightweightCharts === 'undefined' || typeof LightweightCharts.createChart !== 'function' || !chartContainer || chartContainer.clientWidth === 0 || chartContainer.clientHeight === 0) {
                    // Hazır değilse, kısa bir süre sonra tekrar dene
                    setTimeout(initializeChart, 100); 
                    return;
                }
                
                // Grafik oluşturma
                const chart = LightweightCharts.createChart(chartContainer, {
                    layout: {
                        background: { color: '#131722' },
                        textColor: '#d1d4dc',
                    },
                    grid: {
                        vertLines: { color: '#2a2e39' },
                        horzLines: { color: '#2a2e39' },
                    },
                    crosshair: {
                        mode: LightweightCharts.CrosshairMode.Normal,
                    },
                    timeScale: {
                        borderColor: '#485c7b',
                        timeVisible: true,
                        secondsVisible: false,
                    },
                    rightPriceScale: {
                        borderColor: '#485c7b',
                    },
                });

                // Mum grafiği serisi ekleme
                const candlestickSeries = chart.addCandlestickSeries({
                    upColor: '#26a69a',
                    downColor: '#ef5350',
                    borderDownColor: '#ef5350',
                    borderUpColor: '#26a69a',
                    wickDownColor: '#ef5350',
                    wickUpColor: '#26a69a',
                });

                // Hareketli Ortalama (MA) serisi ekleme
                const maSeries = chart.addLineSeries({
                    color: 'rgba(255, 165, 0, 0.8)',
                    lineWidth: 2,
                    lastValueVisible: false,
                    priceLineVisible: false,
                });

                // Hacim (Volume) serisi ekleme
                /* KALDIRILDI
                const volumeSeries = chart.addHistogramSeries({
                    color: '#26a69a',
                    priceFormat: {
                        type: 'volume',
                    },
                    priceScaleId: '', // Ayrı bir panele yerleştir
                    scaleMargins: {
                        top: 0.8, // Üstte %80 boşluk bırak
                        bottom: 0,
                    },
                });
                */

                // Örnek veri oluşturma
                function generateChartData() {
                    const data = [];
                    let time = new Date('2023-01-01').getTime() / 1000;
                    let price = 20000;
                    for (let i = 0; i < 200; i++) {
                        const open = price;
                        const close = open + (Math.random() - 0.5) * 500;
                        const high = Math.max(open, close) + Math.random() * 300;
                        const low = Math.min(open, close) - Math.random() * 300;
                        
                        data.push({ time, open, high, low, close });

                        price = close;
                        time += 24 * 60 * 60; // Günlük veri
                    }
                    return { candleData: data };
                }

                // Hareketli ortalama hesaplama
                function calculateMA(data, period) {
                    const maData = [];
                    for (let i = period - 1; i < data.length; i++) {
                        let sum = 0;
                        for (let j = 0; j < period; j++) {
                            sum += data[i - j].close;
                        }
                        maData.push({ time: data[i].time, value: sum / period });
                    }
                    return maData;
                }

                const { candleData } = generateChartData();
                const maData = calculateMA(candleData, 20); // 20 periyotluk MA

                candlestickSeries.setData(candleData);
                maSeries.setData(maData);

                // Grafik boyutunu pencereye göre ayarla
                window.addEventListener('resize', () => {
                    chart.applyOptions({
                        width: chartContainer.clientWidth,
                        height: chartContainer.clientHeight,
                    });
                });
            }

            // Grafik oluşturmayı başlat
            initializeChart();

            // İzleme listesi verileri
            const watchlistData = [
                { symbol: 'ETH/USDT', price: '3,540.12', change: '+2.10%', positive: true },
                { symbol: 'SOL/USDT', price: '165.80', change: '-0.55%', positive: false },
                { symbol: 'XRP/USDT', price: '0.4912', change: '+0.88%', positive: true },
                { symbol: 'DOGE/USDT', price: '0.1589', change: '-1.20%', positive: false },
                { symbol: 'AVAX/USDT', price: '36.45', change: '+3.50%', positive: true },
            ];

            const watchlistEl = document.getElementById('watchlist');
            watchlistData.forEach(item => {
                const li = document.createElement('li');
                li.className = 'flex justify-between items-center p-3 hover:bg-gray-800 cursor-pointer border-b border-gray-800';
                const changeColor = item.positive ? 'text-green-500' : 'text-red-500';
                li.innerHTML = `
                    <div>
                        <span class="font-medium">${item.symbol}</span>
                    </div>
                    <div class="text-right">
                        <span class="block">${item.price}</span>
                        <span class="${changeColor}">${item.change}</span>
                    </div>
                `;
                watchlistEl.appendChild(li);
            });
            
            // Sol araç çubuğu interaktivitesi
            const toolButtons = document.querySelectorAll('.tool-button');
            toolButtons.forEach(button => {
                button.addEventListener('click', () => {
                    toolButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                });
            });
        });
    </script>
</body>
</html>


