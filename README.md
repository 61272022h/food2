<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>75元營養午餐政策與區域供應鏈模擬器</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Chart.js for Budget Breakdown Chart -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;600;700;900&display=swap');
        body {
            font-family: 'Noto Sans TC', sans-serif;
            background-color: #fcfaf7;
            color: #3d3535;
        }
        .card-shadow {
            box-shadow: 0 10px 25px -5px rgba(217, 119, 6, 0.12), 0 8px 10px -6px rgba(0, 0, 0, 0.04);
        }
        /* 自訂漂亮的 Slider 樣式 */
        input[type=range] {
            -webkit-appearance: none;
            width: 100%;
            background: transparent;
        }
        input[type=range]:focus {
            outline: none;
        }
        input[type=range]::-webkit-slider-runnable-track {
            width: 100%;
            height: 8px;
            cursor: pointer;
            background: #f1f5f9;
            border-radius: 999px;
            border: 1px solid #e2e8f0;
        }
        input[type=range]::-webkit-slider-thumb {
            height: 20px;
            width: 20px;
            border-radius: 50%;
            background: #d97706;
            cursor: pointer;
            -webkit-appearance: none;
            margin-top: -6px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
            transition: transform 0.1s ease;
        }
        input[type=range]::-webkit-slider-thumb:hover {
            transform: scale(1.15);
        }
    </style>
</head>
<body class="min-h-screen pb-16">

    <!-- 頂部標題列 -->
    <header class="bg-gradient-to-r from-amber-700 via-amber-600 to-amber-700 text-white py-8 px-4 shadow-lg text-center relative overflow-hidden">
        <div class="absolute -left-6 -top-6 opacity-15 text-8xl"><i class="fa-solid fa-utensils"></i></div>
        <div class="absolute -right-6 -bottom-6 opacity-15 text-8xl"><i class="fa-solid fa-truck-ramp-box"></i></div>
        <div class="max-w-5xl mx-auto relative z-10">
            <span class="inline-block bg-amber-800/80 text-amber-100 text-xs md:text-sm px-3.5 py-1 rounded-full font-bold mb-3 border border-amber-400/30">
                🏫 新北市學校午餐政策專題：75元預算與區域供應鏈動態模擬
            </span>
            <h1 class="text-2xl md:text-4xl font-black tracking-tight">🍱 75元午餐預算與產地運送黑幕模擬器</h1>
            <p class="text-amber-100 text-xs md:text-sm mt-2 max-w-2xl mx-auto leading-relaxed">
                當固定成本、產地距離（物流損耗）與極端氣候菜價交織時，75元預算真正能買到多少原型食材？
            </p>
        </div>
    </header>

    <main class="max-w-6xl mx-auto px-4 mt-8 space-y-8">

        <!-- 75元預算剝落與區域供應鏈模擬器 -->
        <section class="bg-white rounded-3xl p-6 md:p-8 border-2 border-amber-200/80 card-shadow">
            
            <!-- 上方情境切換區：區域物流與天氣變動 -->
            <div class="bg-amber-50/80 p-5 rounded-2xl border border-amber-200 mb-8 space-y-4">
                
                <!-- 區域切換 -->
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-3 border-b border-amber-200/60 pb-3">
                    <h3 class="text-xs md:text-sm font-extrabold text-amber-950 flex items-center gap-2">
                        <i class="fa-solid fa-earth-tw text-amber-600"></i> 1. 選擇學校所在區域（產地運送與區域物流）：
                    </h3>
                    <div id="region-selector" class="grid grid-cols-2 sm:grid-cols-4 gap-2">
                        <button onclick="setRegion('north')" id="btn-reg-north" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-600 bg-amber-600 text-white shadow-sm transition">
                            🏢 都會區 (新北/台北)
                        </button>
                        <button onclick="setRegion('central-south')" id="btn-reg-central-south" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition">
                            🌾 蔬菜主產地 (中南部)
                        </button>
                        <button onclick="setRegion('east')" id="btn-reg-east" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition">
                            ⛰️ 跨山遠途 (宜花東)
                        </button>
                        <button onclick="setRegion('remote')" id="btn-reg-remote" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition">
                            🏝️ 偏鄉離島 (船運航運)
                        </button>
                    </div>
                </div>

                <!-- 變動成本/氣候情境切換 -->
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-3">
                    <h3 class="text-xs md:text-sm font-extrabold text-amber-950 flex items-center gap-2">
                        <i class="fa-solid fa-cloud-sun-rain text-amber-600"></i> 2. 選擇市場變動情境（變動成本與天候）：
                    </h3>
                    <div id="weather-selector" class="grid grid-cols-3 gap-2">
                        <button onclick="setWeather('normal')" id="btn-wea-normal" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-600 bg-amber-600 text-white shadow-sm transition">
                            ☀️ 風調雨順 (正常期)
                        </button>
                        <button onclick="setWeather('typhoon')" id="btn-wea-typhoon" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition">
                            🌀 颱風暴雨 (菜價飆漲)
                        </button>
                        <button onclick="setWeather('subsidized')" id="btn-wea-subsidized" class="px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition">
                            🥦 專案推動 (契作/補助)
                        </button>
                    </div>
                </div>
            </div>

            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-start">
                
                <!-- 左側：成本細項調整滑桿 (佔7欄) -->
                <div class="lg:col-span-7 space-y-4">
                    <div class="flex items-center justify-between">
                        <h3 class="text-xs font-extrabold text-amber-900 uppercase tracking-wider flex items-center gap-1.5">
                            <i class="fa-solid fa-sliders text-amber-600"></i> 固定與變動成本控制項 (單位：元/人)：
                        </h3>
                        <span class="text-[11px] text-gray-400">拉動滑桿調整參數</span>
                    </div>

                    <div class="space-y-4 bg-amber-50/30 p-5 rounded-2xl border border-amber-100">
                        <!-- 1. 人事廚工成本 -->
                        <div class="space-y-1">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-gray-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-user-group text-blue-500 w-4"></i> 1. 人事薪資與廚工勞健保
                                </span>
                                <span class="font-extrabold text-gray-800 bg-white px-2 py-0.5 rounded border"><span id="val-labor">22</span> 元</span>
                            </div>
                            <input type="range" id="slider-labor" min="10" max="35" value="22" oninput="calculateBudget()">
                        </div>

                        <!-- 2. 區域產地運送與冷鏈物流費 (變動/區域性) -->
                        <div class="space-y-1">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-gray-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-truck-fast text-amber-600 w-4"></i> 2. 產地跨區運送與冷鏈損耗 (地區影響)
                                </span>
                                <span class="font-extrabold text-amber-800 bg-white px-2 py-0.5 rounded border border-amber-200"><span id="val-logistics">5</span> 元</span>
                            </div>
                            <input type="range" id="slider-logistics" min="2" max="15" value="5" oninput="calculateBudget()">
                            <p class="text-[10px] text-gray-400">包含中南部蔬菜北運或東送之長途高溫損耗、油資與冷鏈倉儲費用。</p>
                        </div>

                        <!-- 3. 檢驗與衛生安全 -->
                        <div class="space-y-1">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-gray-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-vial-circle-check text-emerald-500 w-4"></i> 3. 食物檢驗與快篩稽查
                                </span>
                                <span class="font-extrabold text-gray-800 bg-white px-2 py-0.5 rounded border"><span id="val-inspect">5</span> 元</span>
                            </div>
                            <input type="range" id="slider-inspect" min="2" max="10" value="5" oninput="calculateBudget()">
                        </div>

                        <!-- 4. 水電瓦斯與廠房耗損 -->
                        <div class="space-y-1">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-gray-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-bolt text-yellow-500 w-4"></i> 4. 廠房水電瓦斯與設備維修
                                </span>
                                <span class="font-extrabold text-gray-800 bg-white px-2 py-0.5 rounded border"><span id="val-util">7</span> 元</span>
                            </div>
                            <input type="range" id="slider-util" min="3" max="12" value="7" oninput="calculateBudget()">
                        </div>

                        <!-- 5. 團膳廠商利潤與風險保險 -->
                        <div class="space-y-1">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-gray-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-chart-line text-purple-500 w-4"></i> 5. 團膳廠商合理營運利潤
                                </span>
                                <span class="font-extrabold text-gray-800 bg-white px-2 py-0.5 rounded border"><span id="val-profit">6</span> 元</span>
                            </div>
                            <input type="range" id="slider-profit" min="2" max="12" value="6" oninput="calculateBudget()">
                        </div>

                        <!-- 6. 政策需求：水果與乳品 -->
                        <div class="space-y-1">
                            <div class="flex justify-between items-center text-xs">
                                <span class="font-bold text-gray-700 flex items-center gap-1.5">
                                    <i class="fa-solid fa-apple-whole text-red-500 w-4"></i> 6. 政策規定水果/乳品搭配
                                </span>
                                <span class="font-extrabold text-gray-800 bg-white px-2 py-0.5 rounded border"><span id="val-fruit">8</span> 元</span>
                            </div>
                            <input type="range" id="slider-fruit" min="4" max="15" value="8" oninput="calculateBudget()">
                        </div>
                    </div>
                </div>

                <!-- 右側：即時數據與餐盤評估 (佔5欄) -->
                <div class="lg:col-span-5 space-y-6">
                    
                    <!-- 圓餅圖區 -->
                    <div class="bg-gray-50 p-5 rounded-2xl border border-gray-200 text-center relative">
                        <h4 class="text-xs font-bold text-gray-600 mb-3">預算扣除比例分析 (Budget Allocation)</h4>
                        <div class="w-52 h-52 mx-auto relative">
                            <canvas id="budgetChart"></canvas>
                        </div>
                    </div>

                    <!-- 實際剩餘菜價大數字 -->
                    <div id="result-card" class="bg-gradient-to-br from-amber-600 to-amber-700 text-white p-6 rounded-2xl shadow-md text-center transition-all duration-300">
                        <span class="text-xs font-bold text-amber-100 uppercase tracking-widest block">殘餘買菜預算</span>
                        <div class="text-5xl font-black my-1 text-white tracking-tight">
                            NT$ <span id="val-food-result">22</span>
                        </div>
                        <div class="text-xs text-amber-100 border-t border-amber-500/50 pt-2.5 mt-2.5 space-y-1">
                            <p>符合新北政策（3菜1湯，除以 4）每道菜僅剩：<strong id="per-dish-price" class="underline text-amber-200">5.50</strong> 元</p>
                            <p class="text-[11px] opacity-90">天候與變動成本調整後「實質購買力」：<strong id="real-buying-power" class="text-white">100%</strong></p>
                        </div>
                    </div>

                    <!-- 菜色模擬分析評語 -->
                    <div class="bg-white p-5 rounded-2xl border-2 border-amber-200 shadow-sm text-xs space-y-2.5">
                        <div class="flex items-center gap-2 font-bold text-amber-900 border-b pb-2">
                            <i class="fa-solid fa-triangle-exclamation text-amber-600 text-sm"></i>
                            <span>區域與供應鏈餐盤診斷：</span>
                        </div>
                        <p id="budget-evaluation-text" class="text-gray-700 leading-relaxed font-medium">
                            模擬計算中...
                        </p>
                    </div>

                </div>

            </div>
        </section>

        <!-- 說明專區 -->
        <section class="bg-amber-50/50 p-6 rounded-2xl border border-amber-200/60 text-xs text-gray-600 space-y-2">
            <h4 class="font-bold text-amber-900 flex items-center gap-1.5 text-sm">
                <i class="fa-solid fa-circle-info text-amber-600"></i> 政策專題說明：
            </h4>
            <p>• <strong>固定成本（Fixed Costs）</strong>：包含廚工薪資、水電瓦斯、基本衛生檢驗、廠商合理營運利潤以及新北政策所規定的每週水果與乳品配置，此類費用屬於剛性支出，極難壓縮。</p>
            <p>• <strong>變動成本與區域物流（Variable & Logistics Costs）</strong>：蔬菜與蛋白質食材高度依賴中南部產地運送，冷鏈運輸、油價以及氣候暴雨（如颱風）會瞬間擠壓剩餘食材預算。當殘餘食材費過低時，廠商為維持不虧損，往往只能被迫選用加工品或廉價勾芡菜色。</p>
        </section>

    </main>

    <script>
        // 區域與氣候變數狀態
        let currentRegion = 'north'; // north, central-south, east, remote
        let currentWeather = 'normal'; // normal, typhoon, subsidized
        let chartInstance = null;

        const regionConfig = {
            'north': { name: '都會區 (新北/台北)', logistics: 5, desc: '經由果菜批發市場轉運，長途運輸成本中等。' },
            'central-south': { name: '蔬菜主產地 (中南部)', logistics: 2, desc: '鄰近產地契作，長途運輸與冷鏈損耗費用最低。' },
            'east': { name: '跨山遠途 (宜花東)', logistics: 8, desc: '需跨越山脈，運送時間長且高溫冷鏈損耗顯著提升。' },
            'remote': { name: '偏鄉離島', logistics: 12, desc: '依賴海運或空運，極易受天候風浪影響，物流成本極高。' }
        };

        const weatherConfig = {
            'normal': { multiplier: 1.0, label: '風調雨順', desc: '菜價平穩正常。' },
            'typhoon': { multiplier: 0.6, label: '颱風暴雨', desc: '菜價飆漲 40% 以上，食物採購力嚴重打折！' },
            'subsidized': { multiplier: 1.2, label: '專案補助', desc: '獲得政府有機蔬菜與在地契作專案津貼補助。' }
        };

        function setRegion(regionKey) {
            currentRegion = regionKey;
            document.querySelectorAll('#region-selector button').forEach(btn => {
                btn.className = "px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition";
            });
            document.getElementById(`btn-reg-${regionKey}`).className = "px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-600 bg-amber-600 text-white shadow-sm transition";

            // 自動微調物流費用 Slider
            document.getElementById('slider-logistics').value = regionConfig[regionKey].logistics;
            calculateBudget();
        }

        function setWeather(weatherKey) {
            currentWeather = weatherKey;
            document.querySelectorAll('#weather-selector button').forEach(btn => {
                btn.className = "px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-200 bg-white text-gray-700 hover:bg-amber-100 transition";
            });
            document.getElementById(`btn-wea-${weatherKey}`).className = "px-3 py-1.5 rounded-xl text-xs font-bold border border-amber-600 bg-amber-600 text-white shadow-sm transition";

            calculateBudget();
        }

        function calculateBudget() {
            const labor = parseInt(document.getElementById('slider-labor').value);
            const logistics = parseInt(document.getElementById('slider-logistics').value);
            const inspect = parseInt(document.getElementById('slider-inspect').value);
            const util = parseInt(document.getElementById('slider-util').value);
            const profit = parseInt(document.getElementById('slider-profit').value);
            const fruit = parseInt(document.getElementById('slider-fruit').value);

            document.getElementById('val-labor').textContent = labor;
            document.getElementById('val-logistics').textContent = logistics;
            document.getElementById('val-inspect').textContent = inspect;
            document.getElementById('val-util').textContent = util;
            document.getElementById('val-profit').textContent = profit;
            document.getElementById('val-fruit').textContent = fruit;

            // 計算殘餘食材費
            const fixedCosts = labor + logistics + inspect + util + profit + fruit;
            const rawFoodBudget = Math.max(0, 75 - fixedCosts);
            const perDish = (rawFoodBudget / 4).toFixed(2); // 3菜1湯共4道

            // 結合天候/氣候係數算出的「實質購買力」
            const weatherMultiplier = weatherConfig[currentWeather].multiplier;
            const realBuyingPowerVal = (rawFoodBudget * weatherMultiplier).toFixed(1);
            const buyingPowerPercentage = Math.round(weatherMultiplier * 100);

            document.getElementById('val-food-result').textContent = rawFoodBudget;
            document.getElementById('per-dish-price').textContent = perDish;
            document.getElementById('real-buying-power').textContent = `${buyingPowerPercentage}% (折合實質價值約 ${realBuyingPowerVal} 元)`;

            // 更新評估與警告
            const evalEl = document.getElementById('budget-evaluation-text');
            const resultCard = document.getElementById('result-card');

            let evalMessage = `📍 <strong>區域現況 [${regionConfig[currentRegion].name}]</strong>：${regionConfig[currentRegion].desc}<br>`;
            evalMessage += `🌤️ <strong>市場變動 [${weatherConfig[currentWeather].label}]</strong>：${weatherConfig[currentWeather].desc}<br><br>`;

            if (rawFoodBudget <= 18 || realBuyingPowerVal <= 15) {
                resultCard.className = "bg-gradient-to-br from-red-600 to-red-800 text-white p-6 rounded-2xl shadow-md text-center transition-all duration-300 animate-pulse";
                evalMessage += "🚨 <strong>食材採購力徹底崩潰！</strong>：因固定成本過高或颱風天菜價暴漲，每道菜實際採購金額低於 4 元。團膳廠商被迫全面使用冷凍三色豆、勾芡濃湯與高比例加工丸子，否則將陷入虧損極限。";
            } else if (rawFoodBudget <= 24) {
                resultCard.className = "bg-gradient-to-br from-amber-600 to-amber-700 text-white p-6 rounded-2xl shadow-md text-center transition-all duration-300";
                evalMessage += "⚠️ <strong>寫實壓迫區（19 - 24元）</strong>：雖然勉強維持3菜1湯，但為了填飽學生肚子，菜單中難免頻繁出現螢光咖哩、電話線海帶結與高比例太白粉勾芡。";
            } else {
                resultCard.className = "bg-gradient-to-br from-emerald-600 to-teal-700 text-white p-6 rounded-2xl shadow-md text-center transition-all duration-300";
                evalMessage += "✨ <strong>相對理想狀態（> 24元）</strong>：食材費相對充裕，且若加上政府補助，能有效採購產地直送的原型蔬菜與優質蛋白質。";
            }

            evalEl.innerHTML = evalMessage;

            // 更新 Chart.js 圖表
            if (chartInstance) {
                chartInstance.data.datasets[0].data = [rawFoodBudget, labor, logistics, inspect, util, profit, fruit];
                chartInstance.update();
            }
        }

        function initChart() {
            const ctx = document.getElementById('budgetChart').getContext('2d');
            chartInstance = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['實際食材費', '人事廚工', '產地物流運送', '檢驗快篩', '水電廠房', '廠商利潤', '水果乳品'],
                    datasets: [{
                        data: [22, 22, 5, 5, 7, 6, 8],
                        backgroundColor: [
                            '#f59e0b', // 食材費
                            '#3b82f6', // 人事
                            '#d97706', // 產地物流
                            '#10b981', // 檢驗
                            '#eab308', // 水電
                            '#a855f7', // 利潤
                            '#ef4444'  // 水果
                        ],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: { legend: { display: false } },
                    cutout: '60%'
                }
            });
        }

        window.onload = function() {
            initChart();
            calculateBudget();
        };
    </script>
</body>
</html>
