<!DOCTYPE html>
<html lang="tr" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ProCharts - Finansal Piyasaların Geleceği</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    
    <!-- Google Fonts: Inter -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
    
    <!-- Lucide Icons CDN -->
    <script src="https://unpkg.com/lucide@latest"></script>

    <!-- Custom CSS for theme and animations -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0d1117;
            background-image: radial-gradient(circle at 1px 1px, rgba(255,255,255,0.05) 1px, transparent 0);
            background-size: 20px 20px;
        }

        .glassmorphism {
            background: rgba(17, 24, 39, 0.6);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .text-gradient {
            background-image: linear-gradient(90deg, #22d3ee, #a78bfa);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-fill-color: transparent;
        }

        .glow-effect {
            box-shadow: 0 0 15px rgba(34, 211, 238, 0.2), 0 0 30px rgba(167, 139, 250, 0.15);
        }
    </style>
</head>
<body class="text-gray-200">

    <!-- Header / Navigation -->
    <header id="header" class="glassmorphism fixed top-0 left-0 right-0 z-50 transition-all duration-300">
        <div class="container mx-auto px-6 py-4 flex justify-between items-center">
            <a href="#" class="text-2xl font-bold text-gradient">
                ProCharts
            </a>
            <nav class="hidden md:flex items-center space-x-8">
                <a href="#features" class="hover:text-cyan-400 transition-colors duration-300">Özellikler</a>
                <a href="#pricing" class="hover:text-cyan-400 transition-colors duration-300">Fiyatlandırma</a>
                <a href="#contact" class="hover:text-cyan-400 transition-colors duration-300">İletişim</a>
            </nav>
            <div class="hidden md:flex items-center space-x-4">
                <a href="#" class="px-4 py-2 rounded-md hover:bg-gray-700 transition-colors">Giriş Yap</a>
                <a href="#" class="bg-cyan-500 text-white px-4 py-2 rounded-md font-semibold hover:bg-cyan-600 transition-all duration-300 transform hover:scale-105">Ücretsiz Başla</a>
            </div>
            <!-- Mobile Menu Button -->
            <button id="menu-btn" class="md:hidden focus:outline-none">
                <i data-lucide="menu" class="w-6 h-6"></i>
            </button>
        </div>
        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden">
            <div class="px-2 pt-2 pb-3 space-y-1 sm:px-3 text-center">
                <a href="#features" class="block px-3 py-2 rounded-md text-base font-medium hover:bg-gray-700">Özellikler</a>
                <a href="#pricing" class="block px-3 py-2 rounded-md text-base font-medium hover:bg-gray-700">Fiyatlandırma</a>
                <a href="#contact" class="block px-3 py-2 rounded-md text-base font-medium hover:bg-gray-700">İletişim</a>
                <div class="border-t border-gray-700 my-4"></div>
                <a href="#" class="block w-full text-left px-3 py-2 rounded-md text-base font-medium hover:bg-gray-700">Giriş Yap</a>
                <a href="#" class="block w-full mt-2 bg-cyan-500 text-white px-3 py-2 rounded-md font-semibold hover:bg-cyan-600">Ücretsiz Başla</a>
            </div>
        </div>
    </header>

    <!-- Main Content -->
    <main>
        <!-- Hero Section -->
        <section class="min-h-screen flex items-center justify-center pt-24 pb-12">
            <div class="container mx-auto px-6 text-center">
                <div class="max-w-4xl mx-auto">
                    <h1 class="text-4xl md:text-6xl lg:text-7xl font-extrabold leading-tight mb-6">
                        <span class="text-gradient">Finansal Piyasaların</span> Geleceği, Sizin Kontrolünüzde.
                    </h1>
                    <p class="text-lg md:text-xl text-gray-400 max-w-2xl mx-auto mb-10">
                        En gelişmiş grafik araçları, anlık piyasa verileri ve sezgisel arayüz ile yatırımlarınıza profesyonel bir dokunuş katın.
                    </p>
                    <div class="flex justify-center items-center gap-4 flex-wrap">
                        <a href="#" class="bg-cyan-500 text-white px-8 py-3 rounded-lg font-bold text-lg hover:bg-cyan-600 transition-all duration-300 transform hover:scale-105 shadow-lg shadow-cyan-500/20">
                            Hemen Keşfet
                        </a>
                        <a href="#" class="border-2 border-gray-600 text-gray-300 px-8 py-3 rounded-lg font-bold text-lg hover:bg-gray-700 hover:border-gray-500 transition-all duration-300">
                            Daha Fazla Bilgi
                        </a>
                    </div>
                </div>

                <!-- Mockup Chart Image -->
                <div class="mt-20 max-w-6xl mx-auto p-4 rounded-xl glassmorphism glow-effect">
                    <img src="https://placehold.co/1200x600/0d1117/22d3ee?text=Gelişmiş+Grafik+Arayüzü" alt="Gelişmiş Grafik Arayüzü" class="rounded-lg w-full h-auto">
                </div>
            </div>
        </section>

        <!-- Features Section -->
        <section id="features" class="py-20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16">
                    <h2 class="text-3xl md:text-4xl font-bold">Neden ProCharts?</h2>
                    <p class="text-gray-400 mt-2">Piyasanın bir adım önünde olmanızı sağlayan araçlar.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    <!-- Feature 1 -->
                    <div class="glassmorphism rounded-xl p-8 text-center hover:border-cyan-400 transition-all duration-300 transform hover:-translate-y-2">
                        <div class="flex justify-center mb-4">
                            <div class="bg-cyan-500/10 p-4 rounded-full">
                                <i data-lucide="bar-chart-2" class="w-8 h-8 text-cyan-400"></i>
                            </div>
                        </div>
                        <h3 class="text-xl font-bold mb-2">Gelişmiş Grafik Araçları</h3>
                        <p class="text-gray-400">100'den fazla teknik gösterge ve çizim aracı ile kendi analizlerinizi oluşturun.</p>
                    </div>
                    <!-- Feature 2 -->
                    <div class="glassmorphism rounded-xl p-8 text-center hover:border-cyan-400 transition-all duration-300 transform hover:-translate-y-2">
                        <div class="flex justify-center mb-4">
                            <div class="bg-cyan-500/10 p-4 rounded-full">
                               <i data-lucide="zap" class="w-8 h-8 text-cyan-400"></i>
                            </div>
                        </div>
                        <h3 class="text-xl font-bold mb-2">Gerçek Zamanlı Veri</h3>
                        <p class="text-gray-400">Tüm küresel borsalardan anlık veri akışı ile piyasanın nabzını tutun.</p>
                    </div>
                    <!-- Feature 3 -->
                    <div class="glassmorphism rounded-xl p-8 text-center hover:border-cyan-400 transition-all duration-300 transform hover:-translate-y-2">
                        <div class="flex justify-center mb-4">
                             <div class="bg-cyan-500/10 p-4 rounded-full">
                                <i data-lucide="users" class="w-8 h-8 text-cyan-400"></i>
                            </div>
                        </div>
                        <h3 class="text-xl font-bold mb-2">Sosyal Ticaret Ağı</h3>
                        <p class="text-gray-400">Diğer yatırımcıların fikirlerini keşfedin, kendi analizlerinizi paylaşın.</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- Pricing Section -->
        <section id="pricing" class="py-20 bg-black/20">
            <div class="container mx-auto px-6">
                <div class="text-center mb-16">
                    <h2 class="text-3xl md:text-4xl font-bold">Size Uygun Planı Seçin</h2>
                    <p class="text-gray-400 mt-2">İhtiyaçlarınıza göre tasarlanmış esnek paketler.</p>
                </div>
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 max-w-5xl mx-auto">
                    <!-- Pricing Plan 1: Basic -->
                    <div class="glassmorphism rounded-xl p-8 flex flex-col">
                        <h3 class="text-2xl font-bold text-center">Basic</h3>
                        <p class="text-center text-gray-400 mt-2">Yeni başlayanlar için</p>
                        <div class="text-4xl font-extrabold text-center my-6">
                            Ücretsiz
                        </div>
                        <ul class="space-y-4 flex-grow">
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Temel Grafik Araçları</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Gecikmeli Piyasa Verileri</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>1 Adet İzleme Listesi</li>
                        </ul>
                        <a href="#" class="w-full mt-8 text-center bg-gray-600 text-white py-3 rounded-lg font-semibold hover:bg-gray-700 transition-colors">Planı Seç</a>
                    </div>
                    <!-- Pricing Plan 2: Pro (Highlighted) -->
                    <div class="glassmorphism rounded-xl p-8 flex flex-col border-2 border-cyan-500 transform scale-105 glow-effect">
                        <p class="text-center font-bold text-cyan-400">EN POPÜLER</p>
                        <h3 class="text-2xl font-bold text-center mt-1">Pro</h3>
                        <p class="text-center text-gray-400 mt-2">Aktif yatırımcılar için</p>
                        <div class="text-4xl font-extrabold text-center my-6">
                            ₺149<span class="text-lg font-medium text-gray-400">/ay</span>
                        </div>
                        <ul class="space-y-4 flex-grow">
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Tüm Gelişmiş Araçlar</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Gerçek Zamanlı Veriler</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>10 Adet İzleme Listesi</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Reklamsız Deneyim</li>
                        </ul>
                        <a href="#" class="w-full mt-8 text-center bg-cyan-500 text-white py-3 rounded-lg font-semibold hover:bg-cyan-600 transition-colors">Planı Seç</a>
                    </div>
                    <!-- Pricing Plan 3: Premium -->
                    <div class="glassmorphism rounded-xl p-8 flex flex-col">
                        <h3 class="text-2xl font-bold text-center">Premium</h3>
                        <p class="text-center text-gray-400 mt-2">Profesyoneller için</p>
                        <div class="text-4xl font-extrabold text-center my-6">
                            ₺299<span class="text-lg font-medium text-gray-400">/ay</span>
                        </div>
                        <ul class="space-y-4 flex-grow">
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Tüm Pro Özellikleri</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Öncelikli Destek</li>
                            <li class="flex items-center"><i data-lucide="check-circle" class="w-5 h-5 text-green-500 mr-2"></i>Sınırsız İzleme Listesi</li>
                        </ul>
                        <a href="#" class="w-full mt-8 text-center bg-gray-600 text-white py-3 rounded-lg font-semibold hover:bg-gray-700 transition-colors">Planı Seç</a>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <!-- Footer -->
    <footer id="contact" class="border-t border-gray-800">
        <div class="container mx-auto px-6 py-8">
            <div class="flex flex-col md:flex-row justify-between items-center">
                <div class="text-center md:text-left mb-4 md:mb-0">
                    <p class="text-lg font-bold text-gradient">ProCharts</p>
                    <p class="text-gray-500">&copy; 2025 ProCharts. Tüm hakları saklıdır.</p>
                </div>
                <div class="flex space-x-6">
                    <a href="#" class="text-gray-500 hover:text-cyan-400"><i data-lucide="twitter"></i></a>
                    <a href="#" class="text-gray-500 hover:text-cyan-400"><i data-lucide="github"></i></a>
                    <a href="#" class="text-gray-500 hover:text-cyan-400"><i data-lucide="linkedin"></i></a>
                </div>
            </div>
        </div>
    </footer>

    <script>
        // Lucide Icons'u başlatma
        lucide.createIcons();

        // Mobil menü toggle script'i
        const menuBtn = document.getElementById('menu-btn');
        const mobileMenu = document.getElementById('mobile-menu');

        menuBtn.addEventListener('click', () => {
            mobileMenu.classList.toggle('hidden');
        });

        // Sayfa içi linklere tıklandığında menüyü kapatma
        document.querySelectorAll('#mobile-menu a').forEach(link => {
            link.addEventListener('click', () => {
                mobileMenu.classList.add('hidden');
            });
        });
        
        // Header'ı scroll sırasında değiştirme
        const header = document.getElementById('header');
        window.addEventListener('scroll', () => {
            if (window.scrollY > 50) {
                header.classList.add('py-2');
                header.classList.remove('py-4');
            } else {
                header.classList.add('py-4');
                header.classList.remove('py-2');
            }
        });
    </script>
</body>
</html>
