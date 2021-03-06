# توضیح مقاله TransUNet

 ## نام مقاله :

 ##  TransUNet: Transformers Make Strong Encoders for Medical Image Segmentation

## چکیده مقاله :

قطعه‌بندی تصاویر پزشکی یک پیش نیاز ضروری برای توسعه سیستم های مراقبت های بهداشتی، به ویژه برای تشخیص بیماری و برنامه ریزی درمان است. در کارهای مختلف قطعه‌بندی تصاویر پزشکی، معماری U شکل، که به نام U-Net نیز شناخته می‌شود، به استاندارد واقعی تبدیل شده و به موفقیت چشمگیری دست یافته است. با این حال، به دلیل محلی بودن ذاتی عملیات کانولوشن، U-Net به طور کلی محدودیت‌هایی را در مدل‌سازی صریح وابستگی دوربرد نشان می‌دهد.

ترانسفورمرها که برای پیش‌بینی دنباله به دنباله طراحی شده‌اند، به عنوان معماری‌های جایگزین با مکانیسم‌های ذاتی خود توجهی ظاهر شده‌اند، اما در نتیجه توانایی‌های محلی‌سازی محدود به دلیل جزئیات سطح پایین ناکافی است.

در این مقاله، ما TransUNet را پیشنهاد می‌کنیم، که هم شایستگی Transformers و هم U-Net را دارد، به عنوان یک جایگزین قوی برای قطعه‌بندی تصویر پزشکی. از یک طرف، ترانسفورمر وصله‌های تصویر نشانه‌گذاری شده از نقشه ویژگی(feature map) شبکه عصبی کانولوشن (CNN) را به عنوان دنباله ورودی برای استخراج context رمزگذاری می‌کند.

از سوی دیگر، رمزگشا ویژگی‌های کدگذاری شده را upsample می‌کند که بعد با نقشه‌های ویژگی CNN با وضوح بالا ترکیب می‌شوند تا محلی‌سازی دقیق را امکان‌پذیر کند.

ما استدلال می‌کنیم که ترانسفورمرها می‌توانند به عنوان رمزگذار قوی برای تسکهای قطعه‌بندی تصویر پزشکی، با ترکیب U-Net برای بهبود جزئیات دقیق‌تر با بازیابی اطلاعات مکانی محلی، عمل کنند. TransUNet نسبت به روش‌های مختلف رقابتی در کاربردهای مختلف پزشکی از جمله تقسیم‌بندی چند عضوی (multi organ) و تقسیم‌بندی قلبی، عملکرد برتری دارد.

## ۱. مقدمه :

شبکه‌های عصبی کانولوشنال (CNN)، به ویژه شبکه‌های کاملاً کانولوشنال (FCNs) ، در قطعه‌بندی تصاویر پزشکی غالب شده‌اند. در میان انواع مختلف، U-Net، که از یک شبکه رمزگذار-رمزگشا متقارن با skip connection برای افزایش حفظ جزئیات تشکیل شده است، به انتخاب واقعی تبدیل شده است.

بر اساس این خط رویکرد، موفقیت فوق‌العاده‌ای در طیف وسیعی از کاربردهای پزشکی مانند قطعه‌بندی قلب از رزونانس مغناطیسی (MR)، بخش‌بندی اندام از توموگرافی کامپیوتری و تقسیم‌بندی پولیپ از ویدئوهای کولونوسکوپی به دست آمده است.

علیرغم قدرت بازنمایی استثنایی، رویکردهای مبتنی بر CNN معمولاً محدودیت‌هایی را برای مدل‌سازی رابطه صریح دوربرد نشان می‌دهند، به دلیل محلی بودن ذاتی عملیات کانولوشن. بنابراین، این معماری‌ها عموماً عملکرد ضعیفی دارند، به‌ویژه برای ساختارهای هدف که تنوع زیادی بین بیماران از نظر بافت، شکل و اندازه نشان می‌دهند. برای غلبه بر این محدودیت، مطالعات موجود پیشنهاد می‌کنند که مکانیسم‌های خودتوجهی self-attention بر اساس ویژگی‌های CNN ایجاد شود. 

از سوی دیگر، ترانسفورمرها که برای پیش‌بینی دنباله به دنباله طراحی شده‌اند، به عنوان معماری‌های جایگزینی ظاهر شده‌اند که عملگرهای پیچیدگی توزیع را به طور کامل به کار می‌گیرند و به جای آن، صرفاً به مکانیسم‌های توجه متکی هستند. برخلاف روش‌های قبلی مبتنی بر CNN، ترانسفورمرها نه تنها در مدل‌سازی global context قدرتمند هستند، بلکه قابلیت انتقال برتر را برای وظایف پایین‌دستی تحت پیش‌آموزش در مقیاس بزرگ نشان می‌دهند. (؟) این موفقیت به طور گسترده در زمینه ترجمه ماشینی و پردازش زبان طبیعی مشاهده شده است (NLP).

اخیراً، تلاش‌ها برای کارهای مختلف تشخیص تصویر با عملکردهای پیشرفته مطابقت داشته یا حتی از آنها فراتر رفته است.

در این مقاله، ما اولین مطالعه را ارائه می کنیم که پتانسیل ترانسفورماتورها را در زمینه قطعه بندی تصویر پزشکی بررسی می کند. با این حال، جالب توجه است، ما متوجه شدیم که یک استفاده ساده (یعنی استفاده از یک ترانسفورماتور برای رمزگذاری وصله‌های تصویر توکن‌شده، و سپس نمونه‌برداری مستقیم از نمایش‌های ویژگی پنهان به یک خروجی متراکم با وضوح کامل) نمی‌تواند نتیجه رضایت‌بخشی ایجاد کند.

دلیل این امر این است که ترانسفورمرها ورودی را به عنوان دنباله های 1 بعدی در نظر می گیرند و به طور انحصاری بر مدل سازی global context در تمام مراحل تمرکز می کنند، بنابراین منجر به ویژگی های با وضوح پایین می شود که فاقد اطلاعات محلی سازی دقیق هستند. و این اطلاعات را نمی توان به طور موثر با نمونه برداری مستقیم upsampling با وضوح کامل بازیابی کرد، بنابراین منجر به یک نتیجه قطعه‌بندی درشت می شود.

از سوی دیگر، معماری‌های CNN (به عنوان مثال، U-Net) راهی برای استخراج نشانه‌های بصری سطح پایین فراهم می‌کنند که می‌تواند به خوبی چنین جزئیات فضایی spatial خوبی را برطرف کند.

برای این منظور، ما TransUNet، اولین چارچوب قطعه‌بندی تصویر پزشکی را پیشنهاد می‌کنیم که مکانیسم‌های توجه به خود را از منظر پیش‌بینی دنباله به دنباله ایجاد می‌کند.

برای جبران کاهش وضوح ویژگی که توسط Transformers ایجاد شده است، TransUNet از یک معماری ترکیبی CNN-Transformer استفاده می کند تا از اطلاعات مکانی spatial با وضوح بالا از ویژگی های CNN و global context رمزگذاری شده توسط Transformers استفاده کند. 

با الهام از طراحی معماری U شکل، ویژگی خودتوجهی کدگذاری شده توسط Transformers سپس نمونه‌برداری (upsample) می‌شود تا با ویژگی‌های مختلف CNN با وضوح بالا که از مسیر رمزگذاری حذف شده‌اند، ترکیب شود تا محلی‌سازی دقیق را امکان‌پذیر کند. ما نشان می‌دهیم که چنین طراحی به چارچوب ما اجازه می‌دهد تا مزایای ترانسفورمرها را حفظ کند و همچنین به تقسیم‌بندی تصویر پزشکی کمک کند. 

نتایج تجربی نشان می‌دهد که معماری مبتنی بر ترانسفورمر ما در مقایسه با روش توجه قبلی مبتنی بر CNN راه بهتری برای افزایش توجه به خود ارائه می‌دهد. علاوه بر این، مشاهده می‌کنیم که ادغام فشرده‌تر ویژگی‌های سطح پایین به طور کلی به دقت بخش‌بندی بهتر منجر می‌شود.  آزمایش‌های گسترده برتری روش ما را در برابر سایر روش‌های رقیب در وظایف مختلف تقسیم‌بندی تصویر پزشکی نشان می‌دهد.

## ۲. کارهای مشابه :

### ترکیب CNN ها با مکانیسم توجه به خود

مطالعات مختلف تلاش کرده‌اند تا مکانیسم‌های توجه به خود را با مدل‌سازی تعاملات global همه پیکسل‌ها بر اساس نقشه‌های ویژگی، در CNN‌ها ادغام کنند. به عنوان مثال، وانگ و همکاران. یک اپراتور غیر محلی طراحی کرد که می تواند به چندین لایه کانولوشن میانی متصل شود. Schlemper و همکاران بر اساس معماری U شکل رمزگذار-رمزگشا ساخته شده است، ماژول‌های attention gate را پیشنهاد کرد که در اتصالات پرش (skip connections) ادغام شده‌اند. متفاوت با این رویکردها، ما از ترانسفورمرها برای تعبیه global self attention در روش خود استفاده می کنیم.

### ترانسفورمرها

ترانسفورمرها برای اولین بار توسط تسک ترجمه ماشینی پیشنهاد شدند و در بسیاری از تسکهای NLP پیشرفته ترین تکنولوژی ها را ایجاد کردند. برای اینکه ترانسفورمرها برای کارهای بینایی کامپیوتری نیز قابل استفاده باشند، تغییرات متعددی انجام شده است. به عنوان مثال، پارمار و همکاران، self-attention را فقط در local neighbourhood برای هر پیکسل پرس و جو به جای global اعمال کردند. چایلد و همکاران ترانسفورمرهای Sparse را پیشنهاد کردند که از تقریب های مقیاس پذیر برای global attention استفاده می کنند. اخیرا Vision Transformer (ViT) با اعمال مستقیم ترانسفورمرها با global attention به تصاویر با اندازه کامل، به پیشرفته ترین طبقه بندی ImageNet دست یافته است.

تا آنجا که ما می دانیم، TransUNet پیشنهادی اولین چارچوب تقسیم بندی تصویر پزشکی مبتنی بر ترانسفورمر است که بر اساس ViT بسیار موفق ساخته شده است.

## ۳. روش :

با توجه به تصویر )x ∈ R^(H × W × C با وضوح فضایی(spatial) H × W و C تعداد کانال. 

H : Height

W : Width

C : Channels

هدف ما پیش‌بینی labelmap مربوط به پیکسل با اندازه H × W است. متداول ترین راه آموزش مستقیم یک CNN (به عنوان مثال، U-Net) است تا ابتدا تصاویر را در نمایش های ویژگی های سطح بالا رمزگذاری کند، که سپس به وضوح فضایی کامل رمزگشایی می شوند. 

برخلاف روش‌های موجود، روش ما مکانیسم‌های self-attention را با استفاده از ترانسفورمرها در طراحی رمزگذار معرفی می‌کند. 

ابتدا نحوه اعمال مستقیم ترانسفورمر برای رمزگذاری feature representation از decomposed image patches را در بخش ۳.۱ معرفی می کنیم. سپس، چارچوب کلی TransUNet در بخش ۳.۲ توضیح داده خواهد شد. 

### ۳.۱ : Transformer به عنوان Encoder

#### ترتیب بندی تصاویر

طبق ViT، ابتدا توکن‌سازی را با تغییر شکل ورودی x به دنباله‌ای از تکه‌های دوبعدی مسطح انجام می‌دهیم.

 xi ∈ R^(P^2 ×C)

اندازه هر patch برابر با P^2 = P × P است

i برابر است با ۱ تا N. 

N = W×H/(P^2)

N تعداد patch ها هست. ( یا همان input sequence length)



![transunet-arch](https://github.com/mnn59/unet/blob/main/transunet/images/transunet-arch.png)



#### Patch Embedding

ما patch های بردار xp را با استفاده از یک طرح ریزی خطی قابل آموزش در یک فضای embed شده D بعدی نگاشت می کنیم. برای رمزگذاری اطلاعات مکانی patch، جاسازی‌های موقعیت خاصیposition embedding  را یاد می‌گیریم که برای حفظ اطلاعات موقعیتی به شرح زیر به جاسازی‌های وصله اضافه می‌شوند:



![Screenshot 2022-07-14 041936](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-14%20041936.jpg)




رمزگذار ترانسفورمر از L لایه از بلوک های خودتوجهی چند سر (MSA) و پرسپترون چند لایه (MLP) تشکیل شده است.

بنابراین خروجی لایه l را می توان به صورت زیر نوشت: 



![Screenshot 2022-07-14 at 04-22-32 2102.04306v1.pdf](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-14%20at%2004-22-32%202102.04306v1.pdf.png)




LN = Layer Normalization

### ۳.۲ : توضیح Transunet

برای اهداف قطعه‌بندی، یک راه‌حل شهودی این است که به سادگی از نمایش ویژگی کدگذاری‌شده zL∈ R HW/ P2×D به وضوح کامل برای پیش‌بینی خروجی متراکم نمونه‌برداری کنیم. در اینجا برای بازیابی نظم مکانی (spatial order)، ابتدا باید اندازه ویژگی کدگذاری شده از HW/p2 به H/p*W/p تغییر شکل داده شود. ما از یک کانولوشن 1×1 برای کاهش اندازه کانال ویژگی تغییر شکل داده شده به تعداد کلاس، و سپس نقشه ویژگی به طور مستقیم به صورت دوخطی به وضوح کامل H × W برای پیش‌بینی نتیجه تقسیم‌بندی نهایی نمونه‌برداری می‌شود. در مقایسه‌های بعدی در بخش 4.3، ما این خط پایه upsampling ساده‌لوح را در طراحی رمزگشا به عنوان "هیچ" نشان می‌دهیم. (؟)

اگرچه ترکیب یک ترانسفورمر با نمونه برداری ساده از قبل عملکرد معقولی را به همراه دارد، همانطور که در بالا ذکر شد، این استراتژی استفاده بهینه از ترانسفورمرها در تقسیم بندی نیست، زیرا H /P × W/ P معمولاً بسیار کوچکتر از وضوح تصویر اصلی H × W است، بنابراین به ناچار منجر به یک از دست دادن جزئیات سطح پایین (به عنوان مثال، شکل و مرز اندام).

بنابراین، برای جبران چنین اتلاف اطلاعاتی، TransUNet از یک معماری ترکیبی CNN-Transformer به عنوان رمزگذار و همچنین یک نمونه‌برداری بالابر(upsampler) آبشاری برای فعال کردن محلی‌سازی دقیق استفاده می‌کند.

#### ترکیب CNN و Transformer به عنوان Encoder

به جای استفاده از ترانسفورماتور خالص به عنوان رمزگذار (بخش 3.1)، TransUNet از یک مدل ترکیبی CNN-Transformer استفاده می کند که در آن CNN برای اولین بار به عنوان استخراج کننده ویژگی برای تولید نقشه ویژگی برای ورودی استفاده می شود.

patch embedding بر روی patch های 1 × 1 استخراج شده از نقشه ویژگی CNN به جای تصاویر خام اعمال می شود. 

ما این طرح را انتخاب می کنیم. زیرا:

1) به ما امکان می دهد از نقشه های ویژگی CNN با وضوح بالا متوسط در مسیر رمزگشایی استفاده کنیم.
2) ما متوجه شدیم که رمزگذار ترکیبی CNN-Transformer بهتر از استفاده از یک ترانسفورماتور خالص به عنوان رمزگذار عمل می کند.

#### Cascade Upsampler

ما یک upsampler آبشاری (CUP) را معرفی می‌کنیم، که شامل چندین مرحله upsampling برای رمزگشایی ویژگی پنهان برای خروجی ماسک تقسیم‌بندی نهایی است.

پس ازreshape توالی ویژگی های پنهان از HW/P^2×D به H/P * W/P * D ما CUP را با آبشار کردن چندین بلوک upsampling برای رسیدن به وضوح کامل نمونه‌سازی می‌کنیم. که در آن هر بلوک متشکل از یک عملگر 2× upsampling، یک لایه کانولوشن 3×3 و یک لایه ReLU است.

ما می‌توانیم ببینیم که CUP همراه با رمزگذار هیبریدی یک معماری U شکل را تشکیل می‌دهند که امکان تجمیع ویژگی‌ها را در سطوح وضوح مختلف از طریق اتصالات پرش (skip connection) فراهم می‌کند.

 ## ۴. بحث و آزمایش :

### ۴.۱ : دیتاست و ارزیابی

Synapse multi-organ segmentation dataset. ما از این دیتاست چند عضوی استفاده میکنیم. ما از 30 عدد سی تی اسکن شکم در چالش برچسب زدن شکم چند اطلس MICCAI 2015، با 3779 تصویر سی تی بالینی شکم با کنتراست محوری استفاده می کنیم.

هر حجم CT شامل 85 ~ 198 برش 512 × 512 پیکسل، با وضوح فضایی وکسل ([0.54 ~ 0.54] × [0.98 ~ 0.98] × [2.5 ~ 5.0]) میلی متر 3 است.

طبق یک مقاله ای راجع به تصاویر ۳ بعدی در دیتاستها، ما میانگین DSC و میانگین فاصله هاسدورف (HD) را  بر روی 8 اندام شکمی (آئورت، کیسه صفرا، طحال، کلیه چپ، کلیه راست، کبد، پانکراس، معده با تقسیم تصادفی 18 مورد تمرینی (2212 برش محوری) و 12 مورد برای اعتبارسنجی گزارش می‌کنیم. 

#### Automated cardiac diagnosis challenge

چالش ACDC معاینات بیماران مختلف را که از اسکنرهای MRI به دست آورده اند جمع آوری می کند. تصاویر Cine MR در حبس نفس گرفته شد و یک سری برش های محور کوتاه قلب را از پایه تا راس بطن چپ با ضخامت برش 5 تا 8 میلی متر می پوشاند. وضوح فضایی درون صفحه محور کوتاه از 0.83 تا 1.75 mm2/pixel است.(?) هر اسکن بیمار به صورت دستی با ground truth برای بطن چپ (LV)، بطن راست (RV) و میوکارد (MYO) حاشیه نویسی می شود. 

ما میانگین DSC را با تقسیم تصادفی 70 مورد آموزشی train (1930 برش محوری)، 10 مورد برای اعتبارسنجی validation و 20 مورد برای آزمایش test گزارش می‌کنیم.

مقایسه بر روی مجموعه داده CT چند اندامی Synapse (متوسط Dice score و میانگین فاصله هاسدورف بر حسب میلی متر و درصد نمره تاس برای هر اندام).



![Screenshot 2022-07-14 at 05-08-53 2102.04306v1.pdf](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-14%20at%2005-08-53%202102.04306v1.pdf.png)



### ۴.۲ : جزئیات پیاده‌سازی

برای همه آزمایش‌ها، از افزایش داده‌های ساده، به عنوان مثال، چرخش تصادفی(random rotation) و چرخش(flipping) استفاده می‌کنیم.

برای رمزگذار مبتنی بر ترانسفورماتور خالص، ما به سادگی ViT را با 12 لایه ترانسفورماتور استفاده می کنیم. 

برای طراحی رمزگذار هیبریدی، ما ResNet-50 و ViT را که با عنوان "R50-ViT" مشخص می‌شود، در این مقاله ترکیب می‌کنیم.

تمام استخوان بندی ترانسفورمر (یعنی ViT) و ResNet-50 (که با R-50 مشخص می‌شود) در ImageNet از قبل آموزش داده شده‌اند. (pretrain)

resolution ورودی و اندازه پچ P به صورت 224×224 و 16 تنظیم شده است، مگر اینکه غیر از این مشخص شده باشد. بنابراین، ما باید چهار بلوک 2× upsampling را به صورت متوالی در CUP آبشاری کنیم تا به وضوح کامل برسیم.

و برای مدل ها با بهینه ساز SGD با نرخ یادگیری 0.01، تکانه 0.9 و کاهش وزن 1e-4 آموزش دیده اند. اندازه batch size  24 است و تعداد پیش‌فرض تکرارهای آموزشی iteration به ترتیب 20 هزار برای مجموعه داده ACDC و 14 هزار برای مجموعه داده Synapse است.

همه آزمایش‌ها با استفاده از یک واحد پردازش گرافیکی Nvidia RTX2080Ti انجام می‌شوند. تمام حجم های سه بعدی به صورت تکه به تکه استنباط می شوند و برش های دو بعدی پیش بینی شده در کنار هم قرار می گیرند تا پیش بینی سه بعدی را برای ارزیابی بازسازی کنند. 

### ۴.۳ : مقایسه با مدلهای مطرح state-of-the-arts

ما آزمایش‌های اصلی را بر روی مجموعه داده‌های تقسیم‌بندی چند اندامی Synapse با مقایسه TransUNet خود با چهار پیشرفته‌تر قبلی انجام می‌دهیم: 

1. VNet
2. Darr
3. UNet
4. AttnUnet

برای نشان دادن اثربخشی رمزگشای CUP خود، از **ViT** به عنوان **رمزگذار** استفاده می کنیم و نتایج را با استفاده از نمونه برداری ساده ("**None**") و **CUP** به عنوان **رمزگشا** مقایسه می کنیم. 

به همین ترتیب؛ برای نشان دادن اثربخشی طراحی رمزگذار هیبریدی خود، از **CUP** به عنوان **رمزگشا** استفاده می کنیم و نتایج را با استفاده از **ViT و R50-ViT** به عنوان **رمزگذار** مقایسه می کنیم.

برای اینکه مقایسه با خط پایه ViT-Hybrid (R50-ViT-CUP) و TransUNet ما منصفانه باشد، ما همچنین رمزگذار اصلی U-Net [12] و AttnUNet [10] را با ResNet-50 از پیش آموزش دیده ImageNet جایگزین می کنیم. 

نتایج از نظر DSC و میانگین فاصله هاسدورف (بر حسب میلی متر) در جدول 1 گزارش شده است.

(ر.ک به جدول بالا)

اولاً، می‌توانیم ببینیم که در مقایسه با ViT-None، ViT-CUP از نظر میانگین فاصله DSC و Hausdorff به ترتیب 6.36% و 3.50 میلی‌متر بهبود یافته است. این بهبود نشان می دهد که طراحی CUP ما استراتژی رمزگشایی بهتری نسبت به upsampling ارائه می دهد. 

به طور مشابه، در مقایسه با ViT-CUP، R50-ViT-CUP همچنین بهبود اضافی 3.43٪ در DSC و 3.24 میلی متر در فاصله Hausdorff را پیشنهاد می کند که اثربخشی کد ترکیبی ما را نشان می دهد.

TransUNet ما که بر پایه R50-ViT-CUP ساخته شده است و به اتصالات پرش(skip connectin) نیز مجهز است، بهترین نتیجه را در بین انواع مختلف مدل های مبتنی بر ترانسفورماتور به دست می آورد.

ثانیا، جدول 1 همچنین نشان می دهد که TransUNet پیشنهادی نسبت به هنرهای قبلی پیشرفت های قابل توجهی دارد، به عنوان مثال، افزایش عملکرد از 1.91٪ تا 8.67٪ با در نظر گرفتن میانگین DSC متغیر است.

به طور خاص، استفاده مستقیم از ترانسفورماتورها برای تقسیم بندی چند اندام نتایج معقولی به همراه دارد (67.86٪ DSC برای ViT-CUP)، اما نمی تواند با عملکرد U-Net یا attnUNet مطابقت داشته باشد. دلیل این امر این است که ترانسفورماتورها می توانند به خوبی معنایی سطح بالا را که برای طبقه بندی مطلوب است بگیرند اما عدم وجود نشانه های سطح پایین برای تقسیم بندی شکل ظریف تصاویر پزشکی را به تصویر می کشد. (؟)

از سوی دیگر، ترکیب ترانسفورماتورها با CNN، یعنی R50-ViT-CUP، عملکرد بهتری نسبت به V-Net و DARR دارد، اما همچنان نتایج پایین تری نسبت به R50-U-Net و R50-AttnUNe مبتنی بر CNN خالص دارد.

در نهایت، هنگامی که با ساختار U-Net از طریق اتصالات پرش ترکیب می‌شود، TransUNet پیشنهادی یک حالت جدید را ایجاد می‌کند که عملکرد بهتری از R50-ViT-CUP و بهترین R50-AttnUNet قبلی به ترتیب 6.19% و 1.91% دارد که نشان می‌دهد. توانایی قوی TransUNet برای یادگیری ویژگی های معنایی سطح بالا و همچنین جزئیات سطح پایین، که در تقسیم بندی تصاویر پزشکی بسیار مهم است. روند مشابهی را می توان برای میانگین فاصله هاسدورف نیز مشاهده کرد که مزایای TransUNet ما را نسبت به این رویکرد مبتنی بر CNN بیشتر نشان می دهد.

### ۴.۴ : مطالعه تحلیلی 

برای ارزیابی کامل چارچوب پیشنهادی TransUNet و اعتبارسنجی عملکرد تحت تنظیمات مختلف، انواع مطالعات فرسایشی انجام شد. از جمله: 1) تعداد اتصالات پرش. 2) وضوح ورودی؛ 3) توالی طول و اندازه پچ و 4) مقیاس بندی مدل

#### تعداد skip-connection ها

همانطور که در بالا توضیح داده شد، ادغام اتصالات پرش U-Net به بهبود جزئیات تقسیم بندی دقیق تر با بازیابی اطلاعات فضایی spatial سطح پایین کمک می کند. هدف از این آزمایش تأثیر افزودن تعداد مختلف اتصالات پرش در TransUNet است. 

با تغییر تعداد اتصالات پرش به 0 (R50-ViT-CUP)/1/3، (به چی؟) عملکرد تقسیم بندی در DSC متوسط در هر 8 اندام آزمایش در شکل 2 خلاصه شده است.



![image-20220714212522244](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-14%20212515.jpg)



توجه داشته باشید که در تنظیم "1-skip"، ما اتصال پرش skip connection را فقط در مقیاس وضوح 1/4 اضافه می کنیم. ما می توانیم ببینیم که افزودن اتصالات پرش بیشتر به طور کلی به عملکرد بخش بندی بهتر منجر می شود. 

بهترین میانگین DSC و HD با قرار دادن اتصالات پرش به هر سه مرحله میانی نمونه برداری CUP به جز لایه خروجی، یعنی در مقیاس های وضوح 1/2، 1/4 و 1/8 به دست می آید (نشان داده شده در شکل 1).

بنابراین، ما این پیکربندی را برای TransUNet خود اتخاذ می کنیم. 

همچنین شایان ذکر است که افزایش عملکرد اندام های کوچکتر (یعنی آئورت، کیسه صفرا، کلیه ها، پانکراس) بیشتر از اندام های بزرگتر (یعنی کبد، طحال، معده) است.

این نتایج شهود اولیه ما را از ادغام اتصالات پرش U-Net-مانند در طراحی ترانسفورمر تقویت می کند تا امکان یادگیری جزئیات دقیق سطح پایین را فراهم کند.

به عنوان یک مطالعه جالب، ما از ترانسفورمرهای افزودنی در اتصالات پرش استفاده می کنیم، مشابه، و متوجه می شویم که این نوع جدید اتصال پرش حتی می تواند عملکرد قطعه بندی را افزایش دهد. . با توجه به محدودیت حافظه GPU، ما از یک ترانسفورماتور سبک در اتصال پرش با وضوح 1/8 استفاده می کنیم در حالی که دو اتصال پرش دیگر را بدون تغییر نگه می داریم. . در نتیجه، این تغییر ساده منجر به افزایش عملکرد 1.4٪ DSC می شود. 



#### در مورد تأثیر وضوح ورودی

وضوح ورودی پیش فرض برای Transunet، 224×224 است. در اینجا، همانطور که در جدول 2 نشان داده شده است، ما همچنین نتایج آموزش TransUNet را بر روی 512×512 با وضوح بالا ارائه می کنیم.

![image-20220714213809843](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-14%20213803.jpg)



هنگامی که از 512×512 به عنوان ورودی استفاده می کنیم، همان اندازه وصله (یعنی 16) را حفظ می کنیم که منجر به یک طول دنباله تقریبی 5× بزرگتر برای ترانسفورمر می شود.

همانطور که نشان داده شد، افزایش طول دنباله موثر پیشرفت های قوی را نشان می دهد

برای TransUNet، تغییر مقیاس وضوح از 224×224 به 512×512 منجر به بهبود 6.88٪ در DSC متوسط می شود که هزینه محاسباتی بسیار بیشتری را به همراه دارد.

بنابراین، با توجه به هزینه محاسباتی، تمام مقایسه‌های تجربی در این مقاله با default resolution 224×224 برای نشان دادن اثربخشی TransUNet انجام می‌شود.



#### در مورد تاثیر اندازه پچ/طول دنباله

ما همچنین تأثیر اندازه پچ را بر TransUNet بررسی می کنیم. نتایج در جدول 3 خلاصه شده است. مشاهده می شود که عملکرد بخش بندی بالاتر و بهتر معمولاً با اندازه پچ کوچکتر به دست می آید. 

![Screenshot 2022-07-15 at 00-18-26 2102.04306.pdf](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-15%20at%2000-18-26%202102.04306.pdf.png)



توجه داشته باشید که طول دنباله ترانسفورماتور با مربع اندازه پچ نسبت معکوس دارد (به عنوان مثال، اندازه پچ 16 با طول دنباله 196 مطابقت دارد در حالی که اندازه پچ 32 دارای طول دنباله کوتاهتر 49 است)، بنابراین patch size کاهش می یابد (یا افزایش می یابد). طول توالی موثر) پیشرفت های قوی را نشان می دهد، زیرا ترانسفورماتور وابستگی های پیچیده تری را بین هر عنصر برای توالی های ورودی طولانی تر رمزگذاری می کند.

پس از تنظیم در ViT، ما از ۱۶×۱۶ به عنوان اندازه پچ پیش‌فرض در سراسر این مقاله استفاده می‌کنیم.

#### مقیاس‌بندی مدل

آخرین اما نه کم اهمیت ترین، ما مطالعه در اندازه های مختلف مدل TransUNet را ارائه می دهیم. به طور خاص، ما دو TransUNet مختلف را بررسی می کنیم پیکربندی ها، مدل های "Base" و "Large". 

برای مدل "Base"، اندازه پنهان D، تعداد لایه ها، اندازه MLP و تعداد هدها به ترتیب 12، 768، 3072 و 12 تنظیم شده است در حالی که این هایپرپارامترها برای مدل "large" 24 هستند. 1024، 4096، و 16.

Base :

- hidden size D = 12
- \# of layers = 768
- MLP size = 3072
- \# of heads = 12 

Large : 

- hidden size D = 24
- \# of layers = 1024
- MLP size = 4096
- \# of heads = 16

از جدول 4 نتیجه می گیریم که مدل بزرگتر منجر به عملکرد بهتر می شود. با در نظر گرفتن هزینه محاسبات، مدل "Base" را برای همه آزمایش ها اتخاذ می کنیم. 

![Screenshot 2022-07-15 at 00-34-10 2102.04306.pdf](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-15%20at%2000-34-10%202102.04306.pdf.png)



مقایسه کیفی رویکردهای مختلف با تجسم. از چپ به راست: (الف) Ground Truth، (ب) TransUNet، (ج) R50-ViT-CUP، (د) R50-AttnUNet، (ه) R50-U-Net. روش ما مثبت کاذب کمتری را پیش‌بینی می‌کند و اطلاعات دقیق‌تری را حفظ می‌کند.

![Screenshot 2022-07-15 at 00-35-37 2102.04306.pdf](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-15%20at%2000-35-37%202102.04306.pdf.png)

و اما این جدول از دیتاست ACDC : 

![image-20220715003639717](https://github.com/mnn59/unet/blob/main/transunet/images/Screenshot%202022-07-15%20003618.jpg)



### ۴.۵ : مصورسازی

همانطور که در شکل 3 نشان داده شده است، نتایج مقایسه کیفی را در مجموعه داده Synapse ارائه می دهیم.

مشاهده می‌شود که:

1) روش‌های مبتنی بر CNN خالص U-Net و AttnUNet به احتمال زیاد اندام‌ها را بیش از حد یا کم‌بخش‌بندی می‌کنند (به عنوان مثال، در ردیف دوم، طحال توسط AttnUNet بیش از حد قطعه‌بندی می‌شود در حالی که کمتر بخش بندی شده توسط UNet)، که نشان می دهد مدل های مبتنی بر ترانسفورماتور، به عنوان مثال، TransUNet ما یا R50-ViT-CUP قدرت قوی تری برای رمزگذاری زمینه های جهانی و تمایز معنایی دارد. 

2) نتایج در ردیف اول نشان می‌دهد که TransUNet ما در مقایسه با سایرین، موارد False Positive  کمتری را پیش‌بینی می‌کند، که نشان می‌دهد TransUNet نسبت به روش‌های دیگر در سرکوب آن پیش‌بینی‌های پر سر و صدا سودمندتر است.
3) برای مقایسه در مدل‌های مبتنی بر ترانسفورماتور، می‌توان مشاهده کرد که پیش‌بینی‌های R50-ViT-CUP نسبت به TransUNet در مورد مرز و شکل درشت‌تر هستند. (برای مثال پیش بینی پانکراس در ردیف دوم) . علاوه بر این، در ردیف سوم، TransUNet به درستی کلیه های چپ و راست را پیش بینی می کند در حالی که R50-ViT-CUP به اشتباه سوراخ داخلی کلیه چپ را پر می کند. 

این مشاهدات نشان می دهد که TransUNet قادر به تقسیم بندی دقیق تر و حفظ اطلاعات دقیق شکل است. دلیل آن این است که TransUNet از مزایای اطلاعات متنی سطح بالا و جزئیات سطح پایین برخوردار است، در حالی که R50-ViT-CUP صرفاً بر ویژگی های معنایی سطح بالا متکی است.

Transunet : 

- high-level global contextual information
- low-level details

R50-ViT-CUP : 

- high-level semantic fearures

این دوباره شهود اولیه ما را از ادغام اتصالات پرش مانند U-Net در طراحی ترانسفورماتور برای فعال کردن محلی سازی دقیق تأیید می کند.

### تعمیم به سایر مجموعه دادگان

برای نشان دادن توانایی تعمیم TransUNet ما، سایر روش‌های تصویربرداری را ارزیابی می‌کنیم، به عنوان مثال، مجموعه داده‌های MR ACDC با هدف تقسیم‌بندی خودکار قلب.

ما پیشرفت‌های ثابت TransUNet را نسبت به روش‌های خالص مبتنی بر CNN (R50-UNet و R50-AttnUnet) و سایر خطوط پایه مبتنی بر ترانسفورماتور (ViT-CUP و R50-ViT-CUP) مشاهده می‌کنیم که مشابه نتایج قبلی در مجموعه داده سیناپس CT است. 

### نتیجه‌گیری

ترانسفورماتورها به عنوان معماری هایی با مکانیسم های ذاتی self-attention قوی شناخته می شوند. در این مقاله، ما اولین مطالعه را برای بررسی استفاده از ترانسفورماتورها برای تقسیم‌بندی تصویر پزشکی عمومی ارائه می‌کنیم. 

برای استفاده کامل از قدرت ترانسفورماتورها، TransUNet پیشنهاد شد که نه تنها با در نظر گرفتن ویژگی های تصویر به عنوان دنباله، global context را رمزگذاری می کند، بلکه به خوبی از ویژگی های سطح پایین CNN از طریق یک طراحی معماری ترکیبی U شکل استفاده می کند. (؟)

به عنوان یک چارچوب جایگزین برای رویکردهای غالب مبتنی بر FCN برای تقسیم‌بندی تصویر پزشکی، TransUNet نسبت به روش‌های مختلف رقیب، از جمله روش‌های خودتوجهی مبتنی بر CNN، به عملکرد برتر دست می‌یابد.

#### سپاسگذاری‌ها

این کار توسط بنیاد Lustgarten برای تحقیقات سرطان پانکراس پشتیبانی شد.

## منابع

...



‍**`خارج از مقاله`**

[توابع زیان برای قطعه‌بندی تصاویر پزشکی](https://hooshio.com/%D8%AA%D9%88%D8%A7%D8%A8%D8%B9-%D8%B2%DB%8C%D8%A7%D9%86-%D8%A8%D8%B1%D8%A7%DB%8C-%D9%82%D8%B7%D8%B9%D9%87-%D8%A8%D9%86%D8%AF%DB%8C-%D8%AA%D8%B5%D8%A7%D9%88%DB%8C%D8%B1-%D9%BE%D8%B2%D8%B4%DA%A9/)

### توابع زیان ناحیه محور

**تابع زیان Dice** **:** این تابع مستقیماً ضریب dice را که رایج‌ترین معیار ارزیابی قطعه‌بندی تصویر است، بهینه‌سازی می‌کند.

**تابع زیان IoU** **(یا**  **زیان جاکارد تابع** **):** این تابع نیز همچون تابع زیان dice برای بهینه‌سازی مستقیم معیار قطعه‌بندی استفاده می‌شود.

### توابع زیان مرز محور

**Hتابع زیان فاصله هاسدروف (HD) Hausdorff distance (HD) loss:** هدف این تابع برآورد فاصله هاسدروف در خروجی محتمل شبکه CNN است تا به این  ترتیب یاد بگیرد که فاصله هاسدروف را مستقیماً کاهش دهد. فاصله هاسدروف یا  HD را می‌توان به کمک پارامتر انتقال فاصله  در تصویر واقعی و قطعه‌بندی‌شده برآورد کرد.

![ts-6](https://github.com/mnn59/unet/blob/main/transunet/images/ts-6.jpg)

[فیلم راجع به تفاوت dice و iou](https://mulloverthing.com/is-dice-coefficient-same-as-iou/)











