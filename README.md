<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اختبار شامل للصف الأول الثانوي</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Cairo', sans-serif;
        }
        .option-btn {
            transition: all 0.2s ease-in-out;
        }
        .option-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        }
        .correct {
            background-color: #22c55e !important; /* green-500 */
            color: white !important;
            border-color: #16a34a !important; /* green-600 */
        }
        .incorrect {
            background-color: #ef4444 !important; /* red-500 */
            color: white !important;
            border-color: #dc2626 !important; /* red-600 */
        }
    </style>
</head>
<body class="bg-gray-100 flex items-center justify-center min-h-screen">

    <div id="quiz-container" class="w-full max-w-3xl mx-auto bg-white rounded-2xl shadow-xl p-6 md:p-10">

        <!-- شاشة البداية -->
        <div id="start-screen" class="text-center">
            <h1 class="text-3xl md:text-4xl font-bold text-gray-800 mb-4">اختبار تقييم المستوى</h1>
            <p class="text-gray-600 text-lg mb-8">هذا الاختبار الشامل مصمم لطلاب الصف الأول الثانوي في مصر ويحتوي على 100 سؤال في مختلف المواد الدراسية.</p>
            <button onclick="startQuiz()" class="bg-blue-600 text-white font-bold py-3 px-10 rounded-lg text-xl hover:bg-blue-700 transition-colors duration-300 shadow-md">
                ابدأ الاختبار
            </button>
        </div>

        <!-- شاشة الاختبار -->
        <div id="quiz-screen" class="hidden">
            <div class="flex justify-between items-center mb-6">
                <h2 id="question-subject" class="text-xl font-bold text-blue-600"></h2>
                <div class="text-lg font-semibold text-gray-700">
                    <span id="question-counter"></span> / 100
                </div>
            </div>
            <div class="bg-gray-50 p-6 rounded-lg mb-6 min-h-[100px] flex items-center">
                <p id="question-text" class="text-xl md:text-2xl font-semibold text-gray-800"></p>
            </div>
            <div id="options-container" class="grid grid-cols-1 gap-4">
                <!-- سيتم إضافة الاختيارات هنا بواسطة جافاسكريبت -->
            </div>
             <div id="feedback" class="mt-4 text-center font-semibold"></div>
        </div>

        <!-- شاشة النتائج -->
        <div id="result-screen" class="hidden text-center">
            <h1 class="text-3xl md:text-4xl font-bold text-gray-800 mb-4">اكتمل الاختبار!</h1>
            <p class="text-gray-600 text-lg mb-6">لقد حصلت على:</p>
            <div class="bg-blue-100 border-4 border-blue-500 rounded-full w-48 h-48 mx-auto flex flex-col items-center justify-center mb-8 shadow-inner">
                <p id="score-text" class="text-5xl font-extrabold text-blue-600"></p>
                <p class="text-xl font-semibold text-blue-500">من 100</p>
            </div>
            <p id="result-message" class="text-xl font-medium text-gray-700 mb-8"></p>
            <button onclick="restartQuiz()" class="bg-gray-700 text-white font-bold py-3 px-10 rounded-lg text-xl hover:bg-gray-800 transition-colors duration-300 shadow-md">
                أعد الاختبار
            </button>
        </div>

    </div>

    <script>
        // --- قاعدة بيانات الأسئلة ---
        const questions = [
            // اللغة العربية (15 سؤال)
            { subject: 'اللغة العربية', question: 'ما هو إعراب كلمة "طالب" في جملة "كان الطالبُ مجتهداً"؟', options: ['اسم كان مرفوع', 'خبر كان منصوب', 'فاعل مرفوع'], answer: 0 },
            { subject: 'اللغة العربية', question: 'من شعراء العصر الجاهلي وصاحب معلقة "قفا نبك من ذكرى حبيب ومنزل"؟', options: ['عنترة بن شداد', 'امرؤ القيس', 'زهير بن أبي سلمى'], answer: 1 },
            { subject: 'اللغة العربية', question: 'أي من الكلمات التالية تعتبر من أخوات "إنّ"؟', options: ['أصبح', 'صار', 'لكنّ'], answer: 2 },
            { subject: 'اللغة العربية', question: 'ما نوع الأسلوب في "لا تهمل واجباتك"؟', options: ['أسلوب أمر', 'أسلوب نهي', 'أسلوب نفي'], answer: 1 },
            { subject: 'اللغة العربية', question: 'جمع كلمة "قاضي" جمعاً سالماً هو:', options: ['قضاة', 'قاضون', 'قواضٍ'], answer: 1 },
            { subject: 'اللغة العربية', question: 'الفعل "يكتبون" هو فعل مضارع مرفوع وعلامة رفعه:', options: ['الضمة', 'ثبوت النون', 'الواو'], answer: 1 },
            { subject: 'اللغة العربية', question: 'في البلاغة، "الجندي أسد" يعتبر:', options: ['استعارة', 'كناية', 'تشبيه بليغ'], answer: 2 },
            { subject: 'اللغة العربية', question: 'من هو كاتب "كليلة ودمنة" الذي ترجمه إلى العربية؟', options: ['الجاحظ', 'ابن المقفع', 'ابن خلدون'], answer: 1 },
            { subject: 'اللغة العربية', question: 'الفعل "استغفر" هو فعل:', options: ['ثلاثي مزيد بثلاثة أحرف', 'رباعي مجرد', 'خماسي'], answer: 0 },
            { subject: 'اللغة العربية', question: 'ما هو المصدر من الفعل "أكرم"؟', options: ['كرم', 'إكرام', 'كرامة'], answer: 1 },
            { subject: 'اللغة العربية', question: 'أي من الأفعال التالية يعتبر من الأفعال الخمسة؟', options: ['يدرس', 'يدرسان', 'درسوا'], answer: 1 },
            { subject: 'اللغة العربية', question: 'ما نوع "ما" في جملة "ما أجمل السماء!"؟', options: ['نافية', 'استفهامية', 'تعجبية'], answer: 2 },
            { subject: 'اللغة العربية', question: 'كلمة "عصا" اسم:', options: ['مقصور', 'منقوص', 'ممدود'], answer: 0 },
            { subject: 'اللغة العربية', question: 'ما هو الغرض البلاغي من الأمر في قول الشاعر "ألا أيها الليل الطويل ألا انجلِ"؟', options: ['الدعاء', 'التمني', 'النصح'], answer: 1 },
            { subject: 'اللغة العربية', question: 'إعراب "أخاك" في جملة "احترمت أخاك":', options: ['فاعل مرفوع بالألف', 'مفعول به منصوب بالألف', 'مضاف إليه مجرور بالياء'], answer: 1 },
            
            // اللغة الإنجليزية (15 سؤال)
            { subject: 'اللغة الإنجليزية', question: 'I haven\'t seen him ___ last Monday.', options: ['for', 'since', 'ago'], answer: 1 },
            { subject: 'اللغة الإنجليزية', question: 'If I ___ you, I would study harder.', options: ['am', 'was', 'were'], answer: 2 },
            { subject: 'اللغة الإنجليزية', question: 'This is the man ___ car was stolen.', options: ['who', 'which', 'whose'], answer: 2 },
            { subject: 'اللغة الإنجليزية', question: 'She is interested ___ learning new languages.', options: ['in', 'on', 'at'], answer: 0 },
            { subject: 'اللغة الإنجليزية', question: 'He used to ___ football when he was young.', options: ['play', 'playing', 'plays'], answer: 0 },
            { subject: 'اللغة الإنجليزية', question: 'The opposite of "ancient" is:', options: ['old', 'modern', 'historic'], answer: 1 },
            { subject: 'اللغة الإنجليزية', question: 'A person who writes books is called an ___', options: ['artist', 'author', 'actor'], answer: 1 },
            { subject: 'اللغة الإنجليزية', question: 'Look out! The bus ___.', options: ['is coming', 'comes', 'came'], answer: 0 },
            { subject: 'اللغة الإنجليزية', question: 'I need to buy a ___ of bread.', options: ['bar', 'bottle', 'loaf'], answer: 2 },
            { subject: 'اللغة الإنجليزية', question: 'He is much ___ than his brother.', options: ['tall', 'taller', 'the tallest'], answer: 1 },
            { subject: 'اللغة الإنجليزية', question: 'The film was boring, ___?', options: ['was it', 'wasn\'t it', 'did it'], answer: 1 },
            { subject: 'اللغة الإنجليزية', question: 'You ___ smoke in the hospital.', options: ['mustn\'t', 'don\'t have to', 'should'], answer: 0 },
            { subject: 'اللغة الإنجليزية', question: 'My friend suggested ___ to the cinema.', options: ['to go', 'go', 'going'], answer: 2 },
            { subject: 'اللغة الإنجليزية', question: 'The Pyramids ___ by the ancient Egyptians.', options: ['were built', 'built', 'build'], answer: 0 },
            { subject: 'اللغة الإنجليزية', question: 'What does "to see eye to eye" mean?', options: ['To look at someone', 'To agree with someone', 'To have good vision'], answer: 1 },
            
            // التاريخ (10 أسئلة)
            { subject: 'التاريخ', question: 'من هو الملك الذي وحد القطرين (مصر العليا والسفلى) عام 3200 ق.م؟', options: ['خوفو', 'نارمر (مينا)', 'رمسيس الثاني'], answer: 1 },
            { subject: 'التاريخ', question: 'أطلق على عصر الدولة القديمة في مصر اسم عصر...', options: ['بناة الأهرامات', 'الرخاء الاقتصادي', 'المجد الحربي'], answer: 0 },
            { subject: 'التاريخ', question: 'الحضارة التي قامت في بلاد العراق القديم هي الحضارة...', options: ['الفينيقية', 'الرومانية', 'البابلية'], answer: 2 },
            { subject: 'التاريخ', question: 'من هو قائد المسلمين في معركة حطين؟', options: ['خالد بن الوليد', 'صلاح الدين الأيوبي', 'طارق بن زياد'], answer: 1 },
            { subject: 'التاريخ', question: 'سقطت مدينة القسطنطينية على يد السلطان العثماني...', options: ['سليم الأول', 'محمد الفاتح', 'سليمان القانوني'], answer: 1 },
            { subject: 'التاريخ', question: 'قامت الحضارة اليونانية القديمة على أفكار فلاسفة كبار مثل...', options: ['كونفوشيوس', 'زرادشت', 'أرسطو'], answer: 2 },
            { subject: 'التاريخ', question: 'أول عاصمة لمصر في عصر الدولة القديمة كانت مدينة...', options: ['طيبة', 'منف', 'أون'], answer: 1 },
            { subject: 'التاريخ', question: 'الكتابة التي استخدمها المصريون القدماء لتسجيل النصوص الدينية هي الكتابة...', options: ['الهيراطيقية', 'الديموطيقية', 'الهيروغليفية'], answer: 2 },
            { subject: 'التاريخ', question: 'انتهى عصر الدولة الوسطى بغزو...', options: ['الآشوريين', 'الهكسوس', 'الفرس'], answer: 1 },
            { subject: 'التاريخ', question: 'من أشهر ملوك الدولة الحديثة الذي اهتم بالتجارة وأرسل بعثة إلى بلاد بونت؟', options: ['تحتمس الثالث', 'أمنحتب الرابع', 'حتشبسوت'], answer: 2 },

            // الجغرافيا (10 أسئلة)
            { subject: 'الجغرافيا', question: 'ما هو أطول نهر في العالم؟', options: ['نهر الأمازون', 'نهر النيل', 'نهر اليانغتسي'], answer: 1 },
            { subject: 'الجغرافيا', question: 'أكبر محيطات العالم من حيث المساحة هو المحيط...', options: ['الهادي', 'الأطلنطي', 'الهندي'], answer: 0 },
            { subject: 'الجغرافيا', question: 'يقع منخفض القطارة في أي صحراء بمصر؟', options: ['الصحراء الشرقية', 'شبه جزيرة سيناء', 'الصحراء الغربية'], answer: 2 },
            { subject: 'الجغرافيا', question: 'تحدث ظاهرة الليل والنهار بسبب...', options: ['دوران الأرض حول الشمس', 'دوران الأرض حول محورها', 'ميل محور الأرض'], answer: 1 },
            { subject: 'الجغرافيا', question: 'أعلى قمة جبلية في مصر هي قمة جبل...', options: ['موسى', 'سانت كاترين', 'علبة'], answer: 1 },
            { subject: 'الجغرافيا', question: 'ما هو الخط الوهمي الرئيسي الذي يقسم الكرة الأرضية إلى نصفين شمالي وجنوبي؟', options: ['خط جرينتش', 'مدار السرطان', 'خط الاستواء'], answer: 2 },
            { subject: 'الجغرافيا', question: 'الرياح التي تهب على مصر في فصل الربيع وتكون محملة بالرمال والأتربة هي رياح...', options: ['الخماسين', 'التجارية', 'الموسمية'], answer: 0 },
            { subject: 'الجغرافيا', question: 'تعتمد الزراعة في الواحات المصرية على...', options: ['مياه الأمطار', 'نهر النيل', 'المياه الجوفية'], answer: 2 },
            { subject: 'الجغرافيا', question: 'أي من البحيرات التالية تقع شمال مصر على البحر المتوسط؟', options: ['بحيرة ناصر', 'بحيرة قارون', 'بحيرة البرلس'], answer: 2 },
            { subject: 'الجغرافيا', question: 'ما هو التكوين الجيولوجي الذي ينتمي إليه معظم سطح مصر؟', options: ['الزمن الأركي', 'الزمن الثالث', 'الزمن الرابع'], answer: 1 },

            // الفلسفة (10 أسئلة)
            { subject: 'الفلسفة', question: 'من هو الفيلسوف اليوناني الذي قال "اعرف نفسك بنفسك"؟', options: ['أرسطو', 'أفلاطون', 'سقراط'], answer: 2 },
            { subject: 'الفلسفة', question: 'يعتبر التفكير الفلسفي تفكيراً...', options: ['خرافياً', 'نقدياً تأملياً', 'علمياً تجريبياً'], answer: 1 },
            { subject: 'الفلسفة', question: 'أول من استخدم كلمة "فلسفة" بمعناها الحرفي "حب الحكمة" هو...', options: ['طاليس', 'فيثاغورس', 'أفلاطون'], answer: 1 },
            { subject: 'الفلسفة', question: 'الشك الذي يهدف إلى الوصول لليقين هو شك...', options: ['مذهبي', 'منهجي', 'مطلق'], answer: 1 },
            { subject: 'الفلسفة', question: 'أي من أساليب التفكير التالية يعتمد على ربط الأحداث بأسباب غير حقيقية؟', options: ['الأسلوب الديني', 'الأسلوب الخرافي', 'الأسلوب العلمي'], answer: 1 },
            { subject: 'الفلسفة', question: 'يهتم مبحث "الأكسيولوجيا" في الفلسفة بدراسة...', options: ['الوجود', 'المعرفة', 'القيم'], answer: 2 },
            { subject: 'الفلسفة', question: 'المهارة التي تعني القدرة على تفكيك فكرة معقدة إلى عناصرها البسيطة هي مهارة...', options: ['التركيب', 'التحليل', 'التجريد'], answer: 1 },
            { subject: 'الفلسفة', question: 'من هو الفيلسوف الذي أسس "الأكاديمية" في أثينا؟', options: ['سقراط', 'أفلاطون', 'أرسطو'], answer: 1 },
            { subject: 'الفلسفة', question: 'التفكير الذي ينتج أفكاراً جديدة وغير مألوفة هو التفكير...', options: ['الناقد', 'الإبداعي', 'العلمي'], answer: 1 },
            { subject: 'الفلسفة', question: 'ما هو الخطأ المنطقي الذي يحدث عند مهاجمة الشخص بدلاً من مناقشة حجته؟', options: ['رجل القش', 'الشخصنة', 'الاحتكام إلى السلطة'], answer: 1 },

            // الفيزياء (10 أسئلة)
            { subject: 'الفيزياء', question: 'ما هي وحدة قياس القوة في النظام الدولي للوحدات (SI)؟', options: ['الجول', 'النيوتن', 'الواط'], answer: 1 },
            { subject: 'الفيزياء', question: 'صيغة أبعاد السرعة هي:', options: ['LT⁻¹', 'LT⁻²', 'MLT⁻²'], answer: 0 },
            { subject: 'الفيزياء', question: 'حاصل الضرب القياسي لمتجهين متعامدين يساوي...', options: ['واحد', 'صفر', 'قيمة عظمى'], answer: 1 },
            { subject: 'الفيزياء', question: 'ينص قانون نيوتن الأول على أن الجسم الساكن يبقى ساكناً والمتحرك يبقى متحركاً ما لم تؤثر عليه...', options: ['سرعة ثابتة', 'قوة خارجية', 'عجلة منتظمة'], answer: 1 },
            { subject: 'الفيزياء', question: 'الكمية التي يلزم لتعريفها معرفة مقدارها واتجاهها هي كمية...', options: ['قياسية', 'متجهة', 'أساسية'], answer: 1 },
            { subject: 'الفيزياء', question: 'عندما يسقط جسم سقوطاً حراً، فإن سرعته...', options: ['تتناقص', 'تظل ثابتة', 'تتزايد'], answer: 2 },
            { subject: 'الفيزياء', question: 'الخطأ النسبي هو النسبة بين الخطأ المطلق و...', options: ['القيمة المقاسة', 'القيمة الحقيقية', 'متوسط القيم'], answer: 1 },
            { subject: 'الفيزياء', question: 'قوة الفعل ورد الفعل حسب قانون نيوتن الثالث تكونان...', options: ['متساويتان في المقدار ومتضادتان في الاتجاه', 'متساويتان في المقدار وفي نفس الاتجاه', 'مختلفتان في المقدار ومتضادتان في الاتجاه'], answer: 0 },
            { subject: 'الفيزياء', question: 'إذا تحرك جسم في مسار دائري بسرعة ثابتة، فإنه يتحرك بعجلة...', options: ['صفرية', 'مركزية', 'تزايدية'], answer: 1 },
            { subject: 'الفيزياء', question: 'الطاقة لا تفنى ولا تستحدث من العدم، هذا هو قانون...', options: ['بقاء الكتلة', 'بقاء كمية الحركة', 'بقاء الطاقة'], answer: 2 },

            // الكيمياء (10 أسئلة)
            { subject: 'الكيمياء', question: 'ما هو الرمز الكيميائي لعنصر الصوديوم؟', options: ['S', 'So', 'Na'], answer: 2 },
            { subject: 'الكيمياء', question: 'الجسيمات ذات الشحنة السالبة في الذرة هي...', options: ['البروتونات', 'النيوترونات', 'الإلكترونات'], answer: 2 },
            { subject: 'الكيمياء', question: 'عدد أفوجادرو يساوي تقريباً...', options: ['6.02 x 10²³', '3.14 x 10¹⁰', '9.8 x 10⁵'], answer: 0 },
            { subject: 'الكيمياء', question: 'الرابطة التي تتكون بين الفلزات واللافلزات هي رابطة...', options: ['تساهمية', 'أيونية', 'فلزية'], answer: 1 },
            { subject: 'الكيمياء', question: 'المحلول الذي قيمة الأس الهيدروجيني (pH) له تساوي 7 هو محلول...', options: ['حمضي', 'قاعدي', 'متعادل'], answer: 2 },
            { subject: 'الكيمياء', question: 'ما هو اسم المركب CH₄؟', options: ['إيثان', 'ميثان', 'بروبان'], answer: 1 },
            { subject: 'الكيمياء', question: 'العنصر الذي عدده الذري 11 يقع في الدورة... والمجموعة...', options: ['الثالثة - 1A', 'الثانية - 2A', 'الرابعة - 1A'], answer: 0 },
            { subject: 'الكيمياء', question: 'تفاعل الحمض مع القاعدة لإنتاج ملح وماء يسمى تفاعل...', options: ['أكسدة', 'اختزال', 'تعادل'], answer: 2 },
            { subject: 'الكيمياء', question: 'أصغر جزء من المادة يمكن أن يوجد على حالة انفراد وتتضح فيه خواص المادة هو...', options: ['الذرة', 'الجزيء', 'العنصر'], answer: 1 },
            { subject: 'الكيمياء', question: 'ما هي الأداة المستخدمة في قياس حجوم السوائل بدقة في المعمل؟', options: ['الكأس الزجاجي', 'الدورق المخروطي', 'المخبار المدرج'], answer: 2 },

            // الأحياء (10 أسئلة)
            { subject: 'الأحياء', question: 'ما هي العضية المسؤولة عن إنتاج الطاقة في الخلية (بيت الطاقة)؟', options: ['النواة', 'الميتوكوندريا', 'الريبوسومات'], answer: 1 },
            { subject: 'الأحياء', question: 'أي من الجزيئات البيولوجية الكبيرة التالية يعتبر المصدر الرئيسي للطاقة؟', options: ['البروتينات', 'الدهون', 'الكربوهيدرات'], answer: 2 },
            { subject: 'الأحياء', question: 'النسيج المسؤول عن نقل الماء والأملاح من الجذر إلى الساق في النبات هو نسيج...', options: ['اللحاء', 'الخشب', 'الكامبيوم'], answer: 1 },
            { subject: 'الأحياء', question: 'تعتبر الإنزيمات مواد...', options: ['دهنية تسرع التفاعل', 'بروتينية تسرع التفاعل', 'كربوهيدراتية تبطئ التفاعل'], answer: 1 },
            { subject: 'الأحياء', question: 'يتم تخزين الجلوكوز الزائد في كبد وعضلات الإنسان على هيئة...', options: ['نشا', 'سليلوز', 'جليكوجين'], answer: 2 },
            { subject: 'الأحياء', question: 'العالم الذي وضع أسس نظرية الخلية هو...', options: ['مندل', 'شفان وشلايدن', 'دارون'], answer: 1 },
            { subject: 'الأحياء', question: 'أي مما يلي لا يوجد في الخلية الحيوانية؟', options: ['الغشاء البلازمي', 'الجدار الخلوي', 'السيتوبلازم'], answer: 1 },
            { subject: 'الأحياء', question: 'الوحدة البنائية للبروتين هي...', options: ['الحمض الدهني', 'الحمض الأميني', 'الجلوكوز'], answer: 1 },
            { subject: 'الأحياء', question: 'تحدث عملية البناء الضوئي في...', options: ['الميتوكوندريا', 'البلاستيدات الخضراء', 'جهاز جولجي'], answer: 1 },
            { subject: 'الأحياء', question: 'أي نوع من الانقسام الخلوي ينتج الأمشاج؟', options: ['الانقسام الميتوزي', 'الانقسام الميوزي', 'الانشطار الثنائي'], answer: 1 },

            // الرياضيات (10 أسئلة)
            { subject: 'الرياضيات', question: 'إذا كان جا(س) = جتا(٣٠)، فإن قيمة س الحادة تساوي:', options: ['٣٠', '٤٥', '٦٠'], answer: 2 },
            { subject: 'الرياضيات', question: 'مجموعة حل المعادلة س² + ٩ = ٠ في مجموعة الأعداد المركبة هي:', options: ['{٣، -٣}', '{٣ت، -٣ت}', 'فاي'], answer: 1 },
            { subject: 'الرياضيات', question: 'في المثلث أ ب ج، إذا كان أ ب = ٣ سم، ب ج = ٤ سم، فإن أ ج يمكن أن يساوي:', options: ['١ سم', '٥ سم', '٨ سم'], answer: 1 },
            { subject: 'الرياضيات', question: 'إذا تشابه مضلعان وكانت النسبة بين محيطيهما ٤ : ٩، فإن النسبة بين مساحتيهما هي:', options: ['٢ : ٣', '٤ : ٩', '١٦ : ٨١'], answer: 2 },
            { subject: 'الرياضيات', question: 'قيمة ت^٢٠ (حيث ت هو العدد التخيلي) تساوي:', options: ['-١', '١', 'ت'], answer: 1 },
            { subject: 'الرياضيات', question: 'جذرا المعادلة س² - ٥س + ٦ = ٠ هما:', options: ['٢، ٣', '-٢، -٣', '١، ٦'], answer: 0 },
            { subject: 'الرياضيات', question: 'الزاوية التي قياسها ٩٣٠ درجة تقع في الربع:', options: ['الأول', 'الثاني', 'الثالث'], answer: 2 },
            { subject: 'الرياضيات', question: 'إذا كان ل، م هما جذرا المعادلة س² - ٧س + ٥ = ٠، فإن ل + م يساوي:', options: ['-٧', '٥', '٧'], answer: 2 },
            { subject: 'الرياضيات', question: 'طول القوس في دائرة طول نصف قطرها ٦ سم ويقابل زاوية مركزية قياسها ٦٠ درجة يساوي:', options: ['٢π سم', '٣π سم', '٦π سم'], answer: 0 },
            { subject: 'الرياضيات', question: 'إذا كان المتجه أ = (٣، ٤)، فإن معيار المتجه أ يساوي:', options: ['٥', '٧', '٢٥'], answer: 0 },
        ];
        
        // --- متغيرات الاختبار ---
        let currentQuestionIndex = 0;
        let score = 0;
        let shuffledQuestions = [];

        // --- عناصر الواجهة ---
        const startScreen = document.getElementById('start-screen');
        const quizScreen = document.getElementById('quiz-screen');
        const resultScreen = document.getElementById('result-screen');
        const questionSubjectEl = document.getElementById('question-subject');
        const questionCounterEl = document.getElementById('question-counter');
        const questionTextEl = document.getElementById('question-text');
        const optionsContainer = document.getElementById('options-container');
        const scoreTextEl = document.getElementById('score-text');
        const resultMessageEl = document.getElementById('result-message');
        const feedbackEl = document.getElementById('feedback');

        // --- دوال الاختبار ---

        // دالة لخلط الأسئلة عشوائياً
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }

        // بدء الاختبار
        function startQuiz() {
            shuffledQuestions = shuffleArray([...questions]);
            currentQuestionIndex = 0;
            score = 0;
            startScreen.classList.add('hidden');
            resultScreen.classList.add('hidden');
            quizScreen.classList.remove('hidden');
            loadQuestion();
        }

        // تحميل السؤال الحالي
        function loadQuestion() {
            if (currentQuestionIndex < shuffledQuestions.length) {
                const currentQuestion = shuffledQuestions[currentQuestionIndex];
                questionSubjectEl.textContent = currentQuestion.subject;
                questionCounterEl.textContent = currentQuestionIndex + 1;
                questionTextEl.textContent = currentQuestion.question;
                
                optionsContainer.innerHTML = '';
                feedbackEl.innerHTML = '';

                currentQuestion.options.forEach((option, index) => {
                    const button = document.createElement('button');
                    button.textContent = option;
                    button.classList.add('option-btn', 'w-full', 'bg-white', 'border-2', 'border-gray-300', 'text-gray-700', 'font-semibold', 'p-4', 'rounded-lg', 'text-lg', 'text-right', 'hover:bg-blue-100', 'hover:border-blue-400');
                    button.onclick = () => selectAnswer(index, button);
                    optionsContainer.appendChild(button);
                });
            } else {
                showResult();
            }
        }

        // اختيار إجابة
        function selectAnswer(selectedIndex, selectedButton) {
            const currentQuestion = shuffledQuestions[currentQuestionIndex];
            const isCorrect = selectedIndex === currentQuestion.answer;
            
            // تعطيل جميع الأزرار بعد الاختيار
            const optionButtons = optionsContainer.querySelectorAll('.option-btn');
            optionButtons.forEach(btn => btn.disabled = true);

            if (isCorrect) {
                score++;
                selectedButton.classList.add('correct');
                feedbackEl.textContent = 'إجابة صحيحة!';
                feedbackEl.classList.add('text-green-600');
            } else {
                selectedButton.classList.add('incorrect');
                optionButtons[currentQuestion.answer].classList.add('correct');
                feedbackEl.textContent = 'إجابة خاطئة.';
                feedbackEl.classList.add('text-red-600');
            }

            // الانتقال للسؤال التالي بعد فترة قصيرة
            setTimeout(() => {
                currentQuestionIndex++;
                loadQuestion();
            }, 1500);
        }

        // عرض النتيجة
        function showResult() {
            quizScreen.classList.add('hidden');
            resultScreen.classList.remove('hidden');
            scoreTextEl.textContent = score;

            let message = '';
            const percentage = (score / shuffledQuestions.length) * 100;
            if (percentage >= 90) {
                message = 'ممتاز! مستوى رائع واستيعاب كامل للمواد.';
            } else if (percentage >= 75) {
                message = 'جيد جداً! لديك أساس قوي وتحتاج لبعض المراجعة البسيطة.';
            } else if (percentage >= 50) {
                message = 'مقبول. تحتاج إلى مراجعة بعض المواد لتقوية مستواك.';
            } else {
                message = 'تحتاج إلى بذل المزيد من الجهد والمراجعة المكثفة استعداداً للصف الثاني.';
            }
            resultMessageEl.textContent = message;
        }

        // إعادة الاختبار
        function restartQuiz() {
            startQuiz();
        }

    </script>
</body>
</html>
