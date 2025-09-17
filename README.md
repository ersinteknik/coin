<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Finansal Grafik</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lightweight Charts by TradingView (Specific Version) -->
    <script src="https://unpkg.com/lightweight-charts@3.8.0/dist/lightweight-charts.standalone.production.js"></script>
    <style>
        /* Sayfanın tamamını kaplaması ve kaydırma çubuklarını kaldırması için */
        html, body {
            height: 100%;
            margin: 0;
            overflow: hidden;
            font-family: 'Inter', sans-serif;
        }
        /* Google Fonts */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

        /* Ana içerik alanı için Grid Layout */
        #main-content {
            display: grid;
            /* Sütunlar: Grafik (genişleyebilir) | İzleme Listesi (sabit) */
            grid-template-columns: 1fr 18rem; /* 18rem = 288px */
            transition: grid-template-columns 0.3s ease-in-out;
        }

        /* İzleme listesi kapalıyken, ikinci sütunu sıfırla */
        #main-content.watchlist-is-closed {
            grid-template-columns: 1fr 0rem;
        }

        /* Küçülürken içeriğinin dışarı taşmasını engelle */
        #watchlist-panel {
            overflow: hidden;
        }
        
        #watchlist-toggle-btn {
            transition: right 0.3s ease-in-out;
            right: 18rem; /* Panel açıkken düğmenin konumu (288px) */
        }

        #main-content.watchlist-is-closed #watchlist-toggle-btn {
            right: 0; /* Panel kapalıyken düğmenin konumu */
        }
    </style>
</head>
<body class="bg-gray-900 text-white">

    <div id="app" class="flex flex-col h-screen">
        <!-- Header -->
        <header class="flex items-center p-4 border-b border-gray-700 bg-gray-800/50 flex-shrink-0">
            <!-- ... existing header content ... -->
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-cyan-400 mr-3" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M10 3a1 1 0 00-1 1v1.333a1 1 0 00.52.888l5 2.5a1 1 0 00.48-.888V5a1 1 0 00-1-1h-4zm-2 6.5a1 1 0 011-1h4a1 1 0 011 1v3.333a1 1 0 01-.52.888l-5 2.5a1 1 0 01-1.48-.888V9.5zM3.52 7.388a1 1 0 00-.48.888v5.224a1 1 0 001.48.888l5-2.5a1 1 0 00.52-.888V9.5a1 1 0 00-1-1h-4a1 1 0 00-1 .388z" clip-rule="evenodd" />
            </svg>
            <h1 class="text-xl font-bold">Piyasa Analizi</h1>
            <div class="ml-auto flex items-center space-x-2">
                <select id="symbol-select" class="bg-gray-700 border border-gray-600 rounded-md px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-cyan-500">
                    <option value="BTCUSD">BTC/USD</option>
                    <option value="ETHUSD">ETH/USD</option>
                </select>
                <div class="flex space-x-1 bg-gray-700 p-1 rounded-md">
                    <button class="timeframe-btn active" data-timeframe="1D">1G</button>
                    <button class="timeframe-btn" data-timeframe="4H">4S</button>
                    <button class="timeframe-btn" data-timeframe="1H">1S</button>
                </div>
                <button id="toggle-watchlist-header-btn" class="p-2 rounded-md hover:bg-gray-700 text-gray-400 hover:text-white focus:outline-none focus:ring-2 focus:ring-cyan-500" title="İzleme Listesi">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M16 4h2a2 2 0 012 2v12a2 2 0 01-2 2h-2m-6-16h2a2 2 0 012 2v12a2 2 0 01-2 2h-2m-6-16h2a2 2 0 012 2v12a2 2 0 01-2 2H4a2 2 0 01-2-2V6a2 2 0 012-2h2" />
                    </svg>
                </button>
            </div>
        </header>

        <!-- Main Content -->
        <main id="main-content" class="flex-grow relative">
            <!-- Chart Area -->
            <div id="chart-container" class="h-full">
                <!-- Chart will be rendered here -->
            </div>

            <!-- Watchlist Panel -->
            <aside id="watchlist-panel" class="bg-gray-800/70 border-l border-gray-700 flex flex-col">
                <div class="p-4 border-b border-gray-700 flex-shrink-0">
                    <h2 class="text-lg font-semibold">İzleme Listesi</h2>
                </div>
                <div class="flex-grow overflow-y-auto">
                    <!-- Watchlist items -->
                    <div class="p-2 space-y-1">
                        <!-- item 1 -->
                        <div class="flex justify-between items-center p-2 rounded-md hover:bg-gray-700 cursor-pointer">
                            <div>
                                <p class="font-bold">BTC/USD</p>
                                <p class="text-sm text-gray-400">Bitcoin</p>
                            </div>
                            <div class="text-right">
                                <p class="font-semibold">67,123.45</p>
                                <p class="text-sm text-green-500">+2.34%</p>
                            </div>
                        </div>
                        <!-- item 2 -->
                        <div class="flex justify-between items-center p-2 rounded-md hover:bg-gray-700 cursor-pointer">
                            <div>
                                <p class="font-bold">ETH/USD</p>
                                <p class="text-sm text-gray-400">Ethereum</p>
                            </div>
                            <div class="text-right">
                                <p class="font-semibold">3,540.12</p>
                                <p class="text-sm text-red-500">-1.12%</p>
                            </div>
                        </div>
                        <!-- item 3 -->
                        <div class="flex justify-between items-center p-2 rounded-md hover:bg-gray-700 cursor-pointer">
                            <div>
                                <p class="font-bold">SOL/USD</p>
                                <p class="text-sm text-gray-400">Solana</p>
                            </div>
                            <div class="text-right">
                                <p class="font-semibold">165.78</p>
                                <p class="text-sm text-green-500">+5.67%</p>
                            </div>
                        </div>
                         <!-- item 4 -->
                        <div class="flex justify-between items-center p-2 rounded-md hover:bg-gray-700 cursor-pointer">
                            <div>
                                <p class="font-bold">XRP/USD</p>
                                <p class="text-sm text-gray-400">Ripple</p>
                            </div>
                            <div class="text-right">
                                <p class="font-semibold">0.5231</p>
                                <p class="text-sm text-red-500">-0.25%</p>
                            </div>
                        </div>
                    </div>
                </div>
            </aside>
            
            <!-- Watchlist Toggle Button -->
            <button id="watchlist-toggle-btn" class="bg-gray-700 hover:bg-gray-600 text-white w-6 h-16 flex items-center justify-center rounded-l-md absolute top-1/2 -translate-y-1/2 z-10">
                <svg id="toggle-icon" xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />
                </svg>
            </button>
        </main>
    </div>

    <script>
        // Stil için özel Tailwind yapılandırması
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                }
            }
        }

        // Zaman dilimi butonları için stil
        const style = document.createElement('style');
        style.innerHTML = `
            .timeframe-btn {
                padding: 4px 12px;
                border-radius: 5px;
                font-size: 14px;
                font-weight: 500;
                color: #A0AEC0; /* gray-400 */
                transition: all 0.2s;
            }
            .timeframe-btn:hover {
                background-color: #4A5568; /* gray-600 */
                color: #FFFFFF;
            }
            .timeframe-btn.active {
                background-color: #2DD4BF; /* teal-400 */
                color: #1A202C; /* gray-800 */
            }
        `;
        document.head.appendChild(style);

        const chartContainer = document.getElementById('chart-container');
        const symbolSelect = document.getElementById('symbol-select');
        const timeframeButtons = document.querySelectorAll('.timeframe-btn');
        const watchlistPanel = document.getElementById('watchlist-panel');
        const watchlistToggleBtn = document.getElementById('watchlist-toggle-btn');
        const toggleIcon = document.getElementById('toggle-icon');
        const toggleWatchlistHeaderBtn = document.getElementById('toggle-watchlist-header-btn');
        const mainContent = document.getElementById('main-content');

        // Chart oluşturma
        const chart = LightweightCharts.createChart(chartContainer, {
            width: chartContainer.clientWidth,
            height: chartContainer.clientHeight,
            layout: {
                background: { color: '#111827' }, // gray-900
                textColor: 'rgba(255, 255, 255, 0.9)',
            },
            grid: {
                vertLines: { color: '#374151' }, // gray-700
                horzLines: { color: '#374151' }, // gray-700
            },
            crosshair: {
                mode: LightweightCharts.CrosshairMode.Normal,
            },
            rightPriceScale: {
                borderColor: '#4B5563', // gray-600
            },
            timeScale: {
                borderColor: '#4B5563', // gray-600
                timeVisible: true,
                secondsVisible: false,
            },
        });

        // Mum grafiği serisi ekleme
        const candlestickSeries = chart.addCandlestickSeries({
            upColor: '#2DD4BF', // teal-400
            downColor: '#F472B6', // pink-400
            borderDownColor: '#F472B6',
            borderUpColor: '#2DD4BF',
            wickDownColor: '#F472B6',
            wickUpColor: '#2DD4BF',
        });

        // Örnek veri oluşturma fonksiyonu
        function generateSampleData(startDate, numPoints) {
            const data = [];
            let price = 30000;
            let date = new Date(startDate);

            for (let i = 0; i < numPoints; i++) {
                const open = price;
                const high = open + Math.random() * 500;
                const low = open - Math.random() * 500;
                const close = low + Math.random() * (high - low);
                
                data.push({
                    time: date.getTime() / 1000,
                    open,
                    high,
                    low,
                    close,
                });
                
                price = close + (Math.random() - 0.5) * 800;
                date.setDate(date.getDate() + 1);
            }
            return data;
        }

        // Farklı semboller için veri haritası
        const dataMap = {
            'BTCUSD': generateSampleData('2023-01-01', 200),
            'ETHUSD': generateSampleData('2023-01-01', 200).map(d => ({ ...d, open: d.open/20, high: d.high/20, low: d.low/20, close: d.close/20})),
        };

        // Grafiği güncelleyen fonksiyon
        function updateChart() {
            const selectedSymbol = symbolSelect.value;
            const data = dataMap[selectedSymbol];
            candlestickSeries.setData(data);
            chart.timeScale().fitContent();
        }

        // Sembol değiştirildiğinde grafiği güncelle
        symbolSelect.addEventListener('change', updateChart);
        
        // Zaman dilimi butonları için olay dinleyicileri
        timeframeButtons.forEach(button => {
            button.addEventListener('click', () => {
                document.querySelector('.timeframe-btn.active').classList.remove('active');
                button.classList.add('active');
                console.log(`Zaman dilimi değişti: ${button.dataset.timeframe}`);
            });
        });

        // İzleme listesini aç/kapat
        let isWatchlistOpen = true;
        
        function toggleWatchlistPanel() {
            isWatchlistOpen = !isWatchlistOpen;
            mainContent.classList.toggle('watchlist-is-closed');

            if (isWatchlistOpen) {
                // Panel açıkken sağa bakan ok
                toggleIcon.innerHTML = `<path fill-rule="evenodd" d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z" clip-rule="evenodd" />`;
            } else {
                // Panel kapalıyken sola bakan ok
                toggleIcon.innerHTML = `<path fill-rule="evenodd" d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z" clip-rule="evenodd" />`;
            }
            // Manuel yeniden boyutlandırma kaldırıldı.
            // Bu iş artık tamamen aşağıdaki ResizeObserver tarafından yönetiliyor.
        }

        watchlistToggleBtn.addEventListener('click', toggleWatchlistPanel);
        toggleWatchlistHeaderBtn.addEventListener('click', toggleWatchlistPanel);

        // Grafik boyutunu pencereye göre ayarla
        const resizeObserver = new ResizeObserver(entries => {
            const { width, height } = entries[0].contentRect;
            // Genişlik veya yükseklik sıfır değilse grafiği yeniden boyutlandır
            if (width > 0 && height > 0) {
                chart.applyOptions({ width, height });
                chart.timeScale().fitContent();
            }
        });
        resizeObserver.observe(chartContainer);

        // Başlangıçta grafiği yükle
        updateChart();

    </script>
</body>
</html>


