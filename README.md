<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مخطط عظم السمكة وعجلة ديمينغ: تحليل وحل مشاكل NOMA</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
            background-color: #F0F4F8;
        }
        .fishbone-container {
            position: relative;
            padding: 2rem 0;
            overflow-x: auto;
        }
        .spine {
            position: absolute;
            left: 0;
            right: 15%;
            top: 50%;
            height: 8px;
            background-color: #0369a1;
            transform: translateY(-50%);
        }
        .head {
            position: absolute;
            right: 0;
            top: 50%;
            transform: translateY(-50%);
            width: 15%;
            min-width: 180px;
            text-align: center;
            padding: 1rem;
            background-color: #0369a1;
            color: white;
            border-radius: 0.75rem;
            font-weight: 700;
            font-size: 1.1rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .bone {
            position: absolute;
            height: 6px;
            background-color: #0ea5e9;
            transform-origin: left center;
        }
        .bone-label {
            position: absolute;
            transform: translateX(-50%);
            background-color: #e0f2fe;
            color: #0c4a6e;
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-weight: 600;
            font-size: 1rem;
            white-space: nowrap;
        }
        .causes {
            position: absolute;
            bottom: 100%;
            width: 200px;
            font-size: 0.8rem;
            color: #475569;
        }
        .causes ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        .causes li {
            position: relative;
            padding-right: 15px;
            margin-bottom: 0.5rem;
        }
        .causes li::before {
            content: '';
            position: absolute;
            right: 0;
            top: 0.5em;
            width: 10px;
            height: 2px;
            background-color: #38bdf8;
        }
        
        .bone-top {
            transform: rotate(-45deg);
        }
        .label-top {
            bottom: 10px;
            left: 50%;
        }
        .causes-top {
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%) rotate(45deg);
            transform-origin: center bottom;
        }

        .bone-bottom {
            transform: rotate(45deg);
        }
        .label-bottom {
            top: 10px;
            left: 50%;
        }
        .causes-bottom {
            top: 20px;
            left: 50%;
            transform: translateX(-50%) rotate(-45deg);
            transform-origin: center top;
        }

        .pdca-cycle {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 2rem;
            margin-top: 3rem;
            position: relative;
        }
        .pdca-step {
            background-color: white;
            border-radius: 1rem;
            padding: 1.5rem;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            width: 90%;
            max-width: 400px;
            text-align: center;
            position: relative;
            z-index: 10;
        }
        .pdca-step h3 {
            font-size: 1.5rem;
            font-weight: 700;
            margin-bottom: 0.75rem;
        }
        .pdca-step.plan { border-top: 5px solid #0284c7; }
        .pdca-step.do { border-top: 5px solid #16a34a; }
        .pdca-step.check { border-top: 5px solid #f97316; }
        .pdca-step.act { border-top: 5px solid #6366f1; }

        .pdca-arrow {
            position: absolute;
            background-color: #94a3b8;
            width: 4px;
            height: 2rem;
            z-index: 5;
        }
        .pdca-arrow::after {
            content: '';
            position: absolute;
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-top: 12px solid #94a3b8;
            bottom: -12px;
            left: 50%;
            transform: translateX(-50%);
        }
        .pdca-arrow:nth-of-type(1) { top: 12.5%; left: calc(50% - 2px); }
        .pdca-arrow:nth-of-type(2) { top: 37.5%; left: calc(50% - 2px); }
        .pdca-arrow:nth-of-type(3) { top: 62.5%; left: calc(50% - 2px); }
        .pdca-arrow:nth-of-type(4) { top: 87.5%; left: calc(50% - 2px); }

        @media (min-width: 768px) {
            .pdca-cycle {
                flex-direction: row;
                flex-wrap: wrap;
                justify-content: center;
                gap: 4rem;
                height: 500px;
            }
            .pdca-step {
                width: calc(50% - 2rem);
                max-width: 300px;
            }
            .pdca-step.plan { order: 1; margin-top: 0; }
            .pdca-step.do { order: 2; margin-top: 0; }
            .pdca-step.check { order: 4; margin-top: 0; }
            .pdca-step.act { order: 3; margin-top: 0; }

            .pdca-arrow { display: none; }

            .pdca-cycle::before {
                content: '';
                position: absolute;
                border: 4px solid #94a3b8;
                border-radius: 50%;
                width: 450px;
                height: 450px;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                box-sizing: border-box;
            }
            .pdca-cycle::after {
                content: '';
                position: absolute;
                width: 0;
                height: 0;
                border-left: 15px solid transparent;
                border-right: 15px solid transparent;
                border-top: 20px solid #94a3b8;
                bottom: calc(50% - 225px);
                left: 50%;
                transform: translateX(-50%) rotate(135deg);
                z-index: 6;
            }
        }
    </style>
</head>
<body class="text-slate-800">

    <div class="container mx-auto px-4 py-8 md:py-12">
        <header class="text-center mb-12">
            <h1 class="text-4xl md:text-5xl font-black text-slate-800 mb-4">تشخيص تحديات NOMA</h1>
            <p class="text-lg md:text-xl text-slate-600 max-w-3xl mx-auto">تحليل الأسباب الجذرية لمشكلة "التعقيد الحسابي العالي" باستخدام مخطط عظم السمكة (إيشيكاوا)</p>
        </header>

        <div class="bg-white p-6 md:p-10 rounded-2xl shadow-xl mb-16">
            <h2 class="text-2xl md:text-3xl font-bold text-slate-800 mb-6 text-center">مخطط عظم السمكة: التعقيد الحسابي في NOMA</h2>
            <div class="fishbone-container h-[700px] md:h-[600px]">
                <div class="spine"></div>
                <div class="head">التعقيد الحسابي العالي في أجهزة الاستقبال (خاصة بسبب SIC)</div>

                <div class="absolute top-[50%] left-[10%] w-[20%] h-full">
                    <div class="bone bone-top w-full"></div>
                    <div class="bone-label label-top">العمليات ⚙️</div>
                    <div class="causes causes-top">
                        <ul>
                            <li>طبيعة عملية SIC المتسلسلة.</li>
                            <li>الحاجة لفك تشفير إشارات متعددة.</li>
                            <li>متطلبات التقدير الدقيق والمستمر للقناة (CSI).</li>
                            <li>خوارزميات تخصيص الطاقة المعقدة.</li>
                        </ul>
                    </div>
                </div>

                <div class="absolute top-[50%] left-[30%] w-[20%] h-full">
                    <div class="bone bone-bottom w-full"></div>
                    <div class="bone-label label-bottom">التكنولوجيا 💻</div>
                    <div class="causes causes-bottom">
                         <ul>
                            <li>قدرات المعالجة المحدودة للأجهزة الحالية.</li>
                            <li>الحاجة إلى محولات A/D عالية الدقة.</li>
                            <li>تصميمات الأجهزة غير المحسّنة لـ SIC.</li>
                            <li>استهلاك الطاقة العالي للمعالجة المعقدة.</li>
                        </ul>
                    </div>
                </div>

                <div class="absolute top-[50%] left-[50%] w-[20%] h-full">
                    <div class="bone bone-top w-full"></div>
                    <div class="bone-label label-top">الأفراد 👥</div>
                     <div class="causes causes-top">
                         <ul>
                            <li>نقص الخبرة في تصميم أنظمة SIC.</li>
                            <li>الحاجة لتدريب متخصص للمهندسين.</li>
                            <li>التحديات في تطوير برامج فعالة.</li>
                        </ul>
                    </div>
                </div>

                <div class="absolute top-[50%] left-[70%] w-[20%] h-full">
                    <div class="bone bone-bottom w-full"></div>
                    <div class="bone-label label-bottom">البيئة 📡</div>
                     <div class="causes causes-bottom">
                         <ul>
                            <li>ظروف القناة اللاسلكية المتغيرة.</li>
                            <li>ديناميكية حركة المستخدمين.</li>
                            <li>قيود الطيف المتاحة.</li>
                        </ul>
                    </div>
                </div>
                 
                <div class="absolute top-[50%] left-[20%] w-[20%] h-full -translate-y-[150px] md:-translate-y-[120px]">
                    <div class="bone bone-top w-full"></div>
                    <div class="bone-label label-top">القياس 📏</div>
                    <div class="causes causes-top">
                        <ul>
                            <li>صعوبة القياس الدقيق لـ CSI.</li>
                            <li>تحديات التحقق من أداء SIC.</li>
                            <li>عدم وجود مقاييس موحدة للتعقيد.</li>
                        </ul>
                    </div>
                </div>
                
                <div class="absolute top-[50%] left-[60%] w-[20%] h-full translate-y-[150px] md:translate-y-[120px]">
                    <div class="bone bone-bottom w-full"></div>
                    <div class="bone-label label-bottom">الموارد 📦</div>
                    <div class="causes causes-bottom">
                        <ul>
                            <li>توفر الموارد الحسابية (الشرائح).</li>
                            <li>معايير الصناعة التي قد لا تدعم التعقيد.</li>
                        </ul>
                    </div>
                </div>

            </div>
        </div>

        <section class="bg-white p-6 md:p-10 rounded-2xl shadow-xl mb-16">
            <h2 class="text-2xl md:text-3xl font-bold text-slate-800 mb-6 text-center">حل المشاكل باستخدام عجلة ديمينغ (PDCA)</h2>
            <p class="text-lg text-slate-600 max-w-3xl mx-auto text-center mb-8">تُعد عجلة ديمينغ منهجية تكرارية لتحسين العمليات وحل المشكلات. دعنا نطبقها على تحسين أداء إلغاء التداخل المتتالي (SIC) في NOMA.</p>

            <div class="pdca-cycle">
                <div class="pdca-step plan">
                    <h3>خطة (Plan) 📝</h3>
                    <p class="text-sm text-slate-600"><strong>تحديد المشكلة:</strong> انتشار الأخطاء في SIC يؤدي إلى تدهور الأداء.</p>
                    <p class="text-sm text-slate-600"><strong>الهدف:</strong> تقليل معدل الخطأ في SIC بنسبة 20% في سيناريوهات القناة الضعيفة.</p>
                    <p class="text-sm text-slate-600"><strong>الخطة:</strong> استخدام خوارزمية SIC محسّنة بالذكاء الاصطناعي ودمج ترميز تصحيح الأخطاء (FEC) أقوى.</p>
                </div>
                <div class="pdca-arrow"></div>
                <div class="pdca-step do">
                    <h3>نفذ (Do) 🛠️</h3>
                    <p class="text-sm text-slate-600"><strong>التنفيذ:</strong> تطوير نموذج أولي لنظام NOMA مع خوارزمية SIC الجديدة وFEC.</p>
                    <p class="text-sm text-slate-600"><strong>التجريب:</strong> إجراء محاكاة مكثفة أو تجارب معملية في بيئات قناة مختلفة لجمع البيانات.</p>
                    <p class="text-sm text-slate-600"><strong>التدريب:</strong> تدريب الفريق على استخدام الأدوات الجديدة وتحليل النتائج.</p>
                </div>
                <div class="pdca-arrow"></div>
                <div class="pdca-step check">
                    <h3>تحقق (Check) ✅</h3>
                    <p class="text-sm text-slate-600"><strong>القياس:</strong> جمع البيانات عن معدل الخطأ في البت (BER) والإنتاجية والتأخير.</p>
                    <p class="text-sm text-slate-600"><strong>المقارنة:</strong> مقارنة النتائج مع الأداء الأساسي والأهداف المحددة.</p>
                    <p class="text-sm text-slate-600"><strong>التحليل:</strong> هل تم تحقيق الهدف؟ ما هي العوامل المؤثرة؟ هل ظهرت مشاكل جديدة؟</p>
                </div>
                <div class="pdca-arrow"></div>
                <div class="pdca-step act">
                    <h3>تصرف (Act) 🔄</h3>
                    <p class="text-sm text-slate-600"><strong>التوحيد القياسي:</strong> إذا كانت النتائج إيجابية، قم بتوحيد الخوارزمية الجديدة.</p>
                    <p class="text-sm text-slate-600"><strong>التعديل:</strong> إذا لم يتم تحقيق الهدف، قم بتعديل الخطة أو استكشاف حلول بديلة (العودة إلى Plan).</p>
                    <p class="text-sm text-slate-600"><strong>التحسين المستمر:</strong> ابحث عن فرص للتحسين الإضافي أو تطبيق الحل على سيناريوهات أخرى.</p>
                </div>
            </div>
        </section>
        
        <footer class="text-center mt-10 text-slate-500">
             <p>هذا المخطط يوضح كيف أن مشكلة واحدة يمكن أن تنبع من تحديات متعددة في فئات مختلفة.</p>
             <p>مستوحى من تحليل المشكلات لتقنية NOMA.</p>
             <p>تصميم رئيس المهندسين: رائد ابراهيم خليل ابراهيم</p>
        </footer>
    </div>
</body>
</html>
# khaleel-ibrahkim-raed
