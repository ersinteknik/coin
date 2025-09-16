<!DOCTYPE html>
<html lang="tr" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ErsinTeknik - Finansal Analiz Platformu</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #030712; /* bg-gray-950 */
        }
        .hero-bg {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            z-index: -1;
            overflow: hidden;
        }
        .glow-effect {
            position: absolute;
            width: 800px;
            height: 800px;
            background: radial-gradient(circle, rgba(29, 78, 216, 0.3) 0%, rgba(29, 78, 216, 0) 60%);
            animation: pulse-glow 10s infinite alternate;
        }
        @keyframes pulse-glow {
            from {
                transform: scale(0.8);
                opacity: 0.7;
            }
            to {
                transform: scale(1.2);
                opacity: 1;
            }
        }
    </style>
</head>
<body class="text-white">

    <div class="relative min-h-screen flex flex-col items-center justify-center text-center overflow-hidden px-4">
        
        <div class="hero-bg">
            <div class="glow-effect top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2"></div>
            <svg width="100%" height="100%" xmlns="http://www.w3.org/2000/svg" class="absolute inset-0 w-full h-full">
                <defs>
                    <pattern id="candlestick" patternUnits="userSpaceOnUse" width="120" height="180" patternTransform="scale(1.2) rotate(10)">
                        <line x1="20" y1="20" x2="20" y2="70" stroke="#16a34a" stroke-width="2"/>
                        <rect x="16" y="35" width="8" height="25" fill="#16a34a"/>
                        <line x1="60" y1="60" x2="60" y2="110" stroke="#dc2626" stroke-width="2"/>
                        <rect x="56" y="75" width="8" height="25" fill="#dc2626"/>
                         <line x1="90" y1="30" x2="90" y2="80" stroke="#16a34a" stroke-width="2"/>
                        <rect x="86" y="45" width="8" height="25" fill="#16a34a"/>
                    </pattern>
                </defs>
                <rect width="100%" height="100%" fill="url(#candlestick)" opacity="0.07"/>
            </svg>
        </div>
        
        <header class="absolute top-0 left-0 right-0 p-6">
            <nav class="flex justify-between items-center max-w-7xl mx-auto">
                <span class="text-2xl font-bold">ErsinTeknik</span>
            </nav>
        </header>

        <main class="z-10">
            <h1 class="text-4xl md:text-6xl font-black tracking-tighter mb-4 leading-tight">
                Finansal Piyasaların Derinliklerine Dalın
            </h1>
            <p class="text-lg md:text-xl text-gray-400 max-w-3xl mx-auto mb-8">
                Gerçek zamanlı veriler, güçlü analiz araçları ve sezgisel arayüz ile stratejilerinizi bir sonraki seviyeye taşıyın.
            </p>
            <a href="chart.html" class="bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-8 rounded-lg text-lg transition-transform transform hover:scale-105 inline-block">
                Platforma Git
            </a>
        </main>
        
        <footer class="absolute bottom-0 left-0 right-0 p-6">
             <div class="max-w-7xl mx-auto text-center text-gray-500 text-sm">
                &copy; 2025 ErsinTeknik. Tüm hakları saklıdır.
             </div>
        </footer>

    </div>

</body>
</html>

