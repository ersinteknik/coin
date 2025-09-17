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
    </style>
</head>
<body class="bg-gray-900 text-white">

    <div id="app" class="flex flex-col h-screen">
        <!-- Header -->
        <header class="flex items-center p-4 border-b border-gray-700 bg-gray-800/50">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-cyan-400 mr-3" viewBox="0 0 20 20" fill="currentColor">
              <path fill-rule="evenodd" d="M10 3a1 1 0 00-1 1v1.333a1 1 0 00.52.888l5 2.5a1 1 0 00.48-.888V5a1 1 0 00-1-1h-4zm-2 6.5a1 1 0 011-1h4a1 1 0 011 1v3.333a1 1 0 01-.52.888l-5 2.5a1 1 0 01-1.48-.888V9.5zM3.52 7.388a1 1 0 00-.48.888v5.224a1 1 0 001.48.888l5-2.5a1 1 0 00.52-.888V9.5a1 1 0 00-1-1h-4a1 1 0 00-1 .388z" clip-rule="evenodd" />
            </svg>
            <h1 class="text-xl font-bold">Piyasa Analizi</h1>
            <div class="ml-auto flex items-center space-x-4">
                <select id="symbol-select" class="bg-gray-700 border border-gray-600 rounded-md px-3 py-1.5 focus:outline-none focus:ring-2 focus:ring-cyan-500">
                    <option value="BTCUSD">BTC/USD</option>
                    <option value="ETHUSD">ETH/USD</option>
                </select>
                <div class="flex space-x-1 bg-gray-700 p-1 rounded-md">
                    <button class="timeframe-btn active" data-timeframe="1D">1G</button>
                    <button class="timeframe-btn" data-timeframe="4H">4S</button>
                    <button class="timeframe-btn" data-timeframe="1H">1S</button>
                </div>
            </div>
        </header>

        <!-- Main Content -->
        <main class="flex-grow flex">
            <!-- Chart Area -->
            <div id="chart-container" class="flex-grow h-full relative">
                <!-- Chart will be rendered here -->
            </div>
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
                // Aktif butonu güncelle
                document.querySelector('.timeframe-btn.active').classList.remove('active');
                button.classList.add('active');
                
                // Burada normalde yeni zaman dilimine göre veri çekilirdi.
                console.log(`Zaman dilimi değişti: ${button.dataset.timeframe}`);
            });
        });

        // Grafik boyutunu yeniden hesaplayan ve uygulayan merkezi fonksiyon
        function resizeChart() {
            if (chartContainer) {
                const { width, height } = chartContainer.getBoundingClientRect();
                if (width > 0 && height > 0) {
                    chart.applyOptions({ width, height });
                }
            }
        }

        // Grafik konteynerinin boyutu her değiştiğinde grafiği yeniden boyutlandır
        const resizeObserver = new ResizeObserver(resizeChart);
        resizeObserver.observe(chartContainer);

        // Başlangıçta grafiği yükle
        updateChart();

        // DÜZELTME: Tarayıcı penceresi yeniden boyutlandırıldığında da grafiği ayarla
        window.addEventListener('resize', resizeChart);

        // DÜZELTME: Sayfa ve tüm kaynaklar (CSS dahil) yüklendiğinde grafiğin
        // son bir kez doğru boyuta ayarlandığından emin ol. Bu, "küçük görünme"
        // sorununu çözer.
        window.addEventListener('load', resizeChart);

    </script>
</body>
</html>

