Now I have sufficient information. Let me create the comprehensive answer in Egyptian Arabic with emojis, detailed explanations, and references to the images/tables I created.

# شرح TuneD وأولويات العمليات في لينكس 🐧⚙️## مقدمة شاملة 📚بسم الله الرحمن الرحيم، النهاردة 🌅 هنتكلم بالتفصيل الممل 📖 عن موضوعين مهمين جداً في إدارة أنظمة لينكس 💻: الأول هو خدمة **TuneD** اللي بتحسّن أداء النظام 🚀، والتاني هو إزاي نتحكم في **أولويات العمليات** (Process Priority) باستخدام الأوامر `nice` و `renice` 🎯.

المواضيع دي مهمة جداً 🔥 لأي حد بيشتغل System Administrator أو بيدير سيرفرات 🖥️، لأن فهمك ليها هيخليك تقدر تضبط السيرفر بتاعك بشكل أحسن وتحصل على أداء ممتاز 💪.## الجزء الأول: خدمة TuneD 🎛️### ما هي خدمة TuneD؟ 🤔**TuneD** هي خدمة (Service/Daemon) 🔧 موجودة في أنظمة Red Hat Enterprise Linux (RHEL) 🎩 من الإصدار 8 وما بعده، وكمان في CentOS وFedora وOracle Linux 🐧. الخدمة دي وظيفتها إنها تراقب النظام 👀 وتحسّن أداءه ⚡ على حسب نوع الشغل (Workload) اللي بتشتغله.[1][2][3]

#### كيف تعمل TuneD؟ ⚙️

الخدمة دي بتشتغل عن طريق حاجة اسمها **البروفايلات** (Profiles) 📋. كل بروفايل فيه مجموعة إعدادات معينة 🔩 بتضبط:

- **المعالج** (CPU) 🧠: زي إعدادات الـ Governor والـ Frequency Scaling
- **القرص الصلب** (Disk) 💾: زي إعدادات الـ I/O Scheduler  
- **الشبكة** (Network) 🌐: زي إعدادات الـ Network Stack Parameters
- **الذاكرة** (Memory) 🧮: زي إعدادات الـ Swappiness وإعدادات الكيرنل[4][5]

#### نوعين من الضبط (Tuning) 📊

TuneD بتقدم نوعين من الضبط:[6][7]

**1️⃣ Static Tuning (ضبط ثابت):**
- بيطبّق الإعدادات مرة واحدة ✅ لما تفعّل البروفايل
- الإعدادات بتفضل ثابتة 🔒 مش بتتغير
- ده النوع الافتراضي 📌 في TuneD
- مناسب للسيرفرات اللي شغلها ثابت ومتوقع 🏢

**2️⃣ Dynamic Tuning (ضبط ديناميكي):**
- بيراقب النظام باستمرار 🔄 كل فترة (كل 10 ثواني افتراضياً)
- بيغيّر الإعدادات 🔀 على حسب الاستخدام الفعلي
- لازم تفعّله يدوياً عن طريق تعديل `/etc/tuned/tuned-main.conf`
- بيقيس 3 حاجات: **CPU** 🧠، **Disk** 💽، **Network** 🌐[7][6]

### البروفايلات المتاحة في TuneD 📂#### أشهر البروفايلات 🌟

**🔵 balanced** - البروفايل المتوازن:
- ده البروفايل الافتراضي 🏠
- بيعمل توازن ⚖️ بين الأداء واستهلاك الطاقة
- مناسب للاستخدام العام 👍
- بيستخدم Auto-scaling للمعالج[8][9]

**🟢 powersave** - توفير الطاقة:
- بيوفّر الطاقة لأقصى حد ممكن 🔋
- مناسب للابتوبات 💻 والأجهزة اللي بتشتغل بالبطارية
- بيقلل سرعة المعالج 🐌
- بيوقّف خدمات مش ضرورية[9][8]

**🔴 throughput-performance** - أداء عالي:
- بيعطي أعلى أداء ممكن 🚀
- بيزوّد سرعة المعالج والقرص والشبكة
- بيستهلك طاقة أكتر ⚡
- مناسب للسيرفرات عالية الأداء 🏭
- بيستخدم CPU Governor = Performance[8][9]

**🟡 latency-performance** - أقل زمن استجابة:
- بيقلل التأخير (Latency) لأقل حد ممكن ⏱️
- مناسب للتطبيقات الحساسة للوقت (Real-time) ⚡
- بيقفل المعالج على Low C-states 🔒
- استهلاك طاقة عالي جداً 🔥[9][8]

**🟣 virtual-guest** - للأجهزة الافتراضية:
- محسّن للعمل داخل Virtual Machine 🖥️
- مناسب لما يكون السيرفر ضيف (Guest) على Hypervisor
- بيحسّن أداء الـ VM 📈[8][9]

**🟠 virtual-host** - لاستضافة VMs:
- محسّن لاستضافة أجهزة افتراضية متعددة 🏢
- مناسب للسيرفرات اللي بتشغّل VMs (Hypervisor)
- بيحسّن إدارة الموارد للـ VMs 🎯[9][8]

**🔵 network-latency** و **network-throughput**:
- مخصصين لتحسين أداء الشبكة 🌐
- الأول بيقلل التأخير والتاني بيزود سرعة نقل البيانات 📊[8][9]

### أوامر TuneD الأساسية 💻#### 1️⃣ تثبيت وتشغيل TuneD 📥

```bash
# تثبيت TuneD
sudo yum install tuned
# أو
sudo dnf install tuned

# تفعيل الخدمة للبدء التلقائي
sudo systemctl enable tuned

# تشغيل الخدمة
sudo systemctl start tuned

# أو تفعيل وتشغيل في نفس الوقت
sudo systemctl enable --now tuned

# التحقق من حالة الخدمة
sudo systemctl status tuned
```

**ملحوظة هامة 📝:** الخدمة لازم تكون Active و Enabled عشان تشتغل صح ✅.[2][3][10]

#### 2️⃣ عرض البروفايلات المتاحة 📋

```bash
# عرض كل البروفايلات
tuned-adm list
```

الأمر ده هيعرض لك قائمة 📜 بكل البروفايلات المثبتة على النظام، وفي الآخر هيقول لك البروفايل النشط حالياً 🎯.[3][10][9]

#### 3️⃣ معرفة البروفايل النشط 🔍

```bash
# عرض البروفايل النشط
tuned-adm active
```

الناتج هيكون زي: `Current active profile: virtual-guest` 📌.[10][9][3]

#### 4️⃣ الحصول على توصية 💡

```bash
# اطلب من TuneD يوصي بالبروفايل المناسب
tuned-adm recommend
```

TuneD هيحلل النظام 🔬 ويقترح البروفايل الأنسب على حسب نوع الجهاز (سيرفر، VM، Desktop) 🖥️.[9][3][10]

#### 5️⃣ تفعيل بروفايل معين ⚡

```bash
# تفعيل بروفايل واحد
sudo tuned-adm profile throughput-performance

# التحقق من التفعيل
tuned-adm active
```

الإعدادات بتتطبق فوراً ⚡ من غير ما تحتاج Reboot 🔄.[3][10][9]

#### 6️⃣ دمج أكتر من بروفايل 🔀

```bash
# دمج بروفايلين مع بعض
sudo tuned-adm profile virtual-guest powersave

# أو دمج 3 بروفايلات
sudo tuned-adm profile throughput-performance network-latency virtual-guest
```

لو في تعارض ❌ بين الإعدادات، البروفايل الأخير بياخد الأولوية 🥇.[8][9][3]

#### 7️⃣ إيقاف كل البروفايلات 🛑

```bash
# إيقاف TuneD
sudo tuned-adm off

# التحقق
tuned-adm active
# الناتج: No current active profile
```

ده بيرجع النظام للإعدادات الافتراضية 🔙.[10][9][3]

#### 8️⃣ التحقق من تطبيق البروفايل ✅

```bash
# التحقق من صحة التطبيق
sudo tuned-adm verify
```

الأمر ده بيتأكد إن الإعدادات الحالية بتطابق البروفايل المفعّل 🎯. لو في مشكلة هيقول لك `Verification failed` ❌.[8][9][3]

### ملفات الإعدادات 📁#### 1️⃣ ملف الإعدادات الرئيسي 🔧

الملف الرئيسي موجود في: `/etc/tuned/tuned-main.conf` 📂

محتوى الملف:[11][7]
```ini
# هل تستخدم daemon؟
daemon = 1

# تفعيل Dynamic Tuning (افتراضياً = 0 يعني Static)
dynamic_tuning = 0

# كل كام ثانية يفحص النظام
sleep_interval = 1

# كل كام ثانية يحدّث الإعدادات (لازم مضاعف sleep_interval)
update_interval = 10

# تفعيل خاصية التوصية
recommend_command = 1
```

**لتفعيل Dynamic Tuning:**
1. افتح الملف: `sudo nano /etc/tuned/tuned-main.conf`
2. غيّر `dynamic_tuning = 0` لـ `dynamic_tuning = 1`
3. احفظ واعمل Restart للخدمة: `sudo systemctl restart tuned` 🔄[6][7]

#### 2️⃣ مواقع البروفايلات 📍

البروفايلات المثبتة موجودة في مكانين:[4][12][11]

**🔸 البروفايلات الافتراضية:**
`/usr/lib/tuned/` - دي البروفايلات اللي جاية مع النظام 📦

**🔸 البروفايلات المخصصة:**
`/etc/tuned/` - دي البروفايلات اللي انت بتعملها ✏️

**ملحوظة مهمة:** 📌 المجلد `/etc/tuned/` ليه أولوية أعلى، يعني لو في بروفايل بنفس الاسم في المكانين، اللي في `/etc/tuned/` هو اللي هيشتغل.[12][4]

#### 3️⃣ هيكل ملف البروفايل 📄

كل بروفايل عبارة عن مجلد فيه ملف `tuned.conf`. مثال من بروفايل `balanced`:[11][6]

```ini
[main]
# ورث الإعدادات من بروفايل تاني
include=powersave

[cpu]
# ضبط CPU Governor
governor=conservative
energy_perf_bias=normal

[disk]
# إعدادات القرص الصلب
alpm=medium_power

[sysctl]
# تعديل قيم الكيرنل
vm.dirty_ratio = 40
vm.swappiness = 10
```

### تثبيت بروفايلات إضافية 🆕TuneD بيسمح لك تنزّل بروفايلات خاصة بتطبيقات معينة زي Oracle Database أو MS SQL Server 🗄️:[9][13]

```bash
# البحث عن بروفايلات إضافية
dnf search tuned-profiles

# تثبيت بروفايل Oracle
sudo dnf install tuned-profiles-oracle

# تثبيت بروفايل MS SQL
sudo dnf install tuned-profiles-mssql

# تفعيل البروفايل
sudo tuned-adm profile oracle
```

البروفايلات دي بتكون محسّنة خصيصاً 🎯 للتطبيقات دي وبتحسّن الأداء بشكل ملحوظ 📈.[13][9]

### إنشاء بروفايل مخصص 🛠️لو عايز تعمل بروفايل خاص بيك:[4][6]

```bash
# 1. إنشاء مجلد للبروفايل
sudo mkdir /etc/tuned/my-custom-profile

# 2. إنشاء ملف الإعدادات
sudo nano /etc/tuned/my-custom-profile/tuned.conf
```

محتوى الملف:
```ini
[main]
# وَرِّث من بروفايل موجود
include=throughput-performance

[cpu]
governor=performance
energy_perf_bias=performance

[sysctl]
# إعدادات مخصصة للكيرنل
vm.swappiness=5
net.core.rmem_max=134217728
net.core.wmem_max=134217728
```

```bash
# 3. تفعيل البروفايل
sudo tuned-adm profile my-custom-profile

# 4. التحقق
tuned-adm verify
```

***

## الجزء الثاني: أولويات العمليات - Nice و Renice 🎯### فهم نظام الأولويات 🧠في لينكس، كل عملية (Process) ليها **أولوية** (Priority) 🏆 بتحدد قد إيه العملية دي مهمة وتستحق تاخد وقت من المعالج (CPU Time) ⏰.[14][15][16]

#### مفهوم Nice Value 🎚️

الـ **Nice Value** هو رقم من `-20` لـ `+19` بيحدد "مدى لطف" العملية مع العمليات التانية:[15][16][14]

**🔴 Nice Value سالب (-20 لـ -1):**
- أولوية **عالية جداً** 🔥
- العملية "مش لطيفة" (Not Nice) - يعني بتاخد CPU كتير
- بس **root** بس اللي يقدر يستخدمها 👑
- مناسبة للعمليات الحرجة والمهمة جداً ⚠️

**🟢 Nice Value = صفر (0):**
- الأولوية **العادية** (Default) 📌
- كل العمليات بتبدأ بـ Nice = 0 افتراضياً
- أي مستخدم يقدر يستخدمها 👤[16][14][15]

**🔵 Nice Value موجب (+1 لـ +19):**
- أولوية **منخفضة** 📉
- العملية "لطيفة" (Nice) - يعني بتسيب CPU للعمليات التانية
- أي مستخدم يقدر يستخدمها 👥
- مناسبة للعمليات اللي مش مستعجلة (Background Jobs) 🐌[14][15][16]

#### علاقة Nice بالـ Priority (PRI) 🔗

الـ **Priority** (PRI) هو الرقم اللي الكيرنل بيستخدمه فعلياً في الـ Scheduling ⚙️. العلاقة بينهم:[15][16]

```
PRI = 20 + Nice Value
```



**مثال توضيحي:**
- لو Nice = -20 → PRI = 20 + (-20) = **0** (أعلى أولوية) 🥇
- لو Nice = 0 → PRI = 20 + 0 = **20** (أولوية عادية) 🥈
- لو Nice = +19 → PRI = 20 + 19 = **39** (أقل أولوية) 🥉

**قاعدة مهمة:** 📏 كل ما الـ PRI أقل، كل ما الأولوية أعلى! ⬆️[16][14][15]

### عرض الأولويات للعمليات 👀#### استخدام أمر `ps` 📊

```bash
# عرض معلومات العمليات مع الأولوية
ps -l

# عرض بتنسيق مخصص
ps -eo pid,ni,pri,comm

# عرض عملية معينة
ps -l | grep firefox
```

الأعمدة المهمة 📋:
- **NI**: Nice Value
- **PRI**: Priority (الأولوية الفعلية)
- **PID**: رقم العملية[17][18][19]

#### استخدام أمر `top` 📈

```bash
# فتح top
top
```

في واجهة `top` هتلاقي:
- عمود **NI**: Nice value لكل عملية
- عمود **PR**: Priority value
- تقدر تضغط `r` عشان تغيّر Nice value من جوا top مباشرة! 🎮[18][17]

### أمر nice - بدء عملية بأولوية محددة 🚀الأمر `nice` بيستخدم لبدء عملية جديدة بـ Nice value معين:[14][20][18]

#### الصيغة الأساسية:
```bash
nice [OPTIONS] COMMAND
```

#### أمثلة عملية 💡

**مثال 1️⃣: بدء عملية بأولوية منخفضة**
```bash
# بدون تحديد قيمة (افتراضياً +10)
nice firefox

# مع تحديد قيمة +15 (أولوية منخفضة جداً)
nice -n 15 backup.sh

# أو بدون -n
nice -15 backup.sh
```

ده مناسب للعمليات اللي مش مستعجلة 🐌 زي الـ Backup أو الـ Compression 📦.[20][18][21]

**مثال 2️⃣: بدء عملية بأولوية عالية (يحتاج root)**
```bash
# أولوية عالية (-10)
sudo nice -n -10 critical-app

# أولوية عالية جداً (-20)
sudo nice -n -20 realtime-process
```

⚠️ **تحذير:** استخدام قيم سالبة يحتاج صلاحيات root! وممكن يأثر على استقرار النظام لو استخدمته غلط.[18][21][20]

**مثال 3️⃣: عملية كبيرة في الخلفية**
```bash
# ضغط ملف كبير بأولوية منخفضة جداً
nice -n 19 tar -czf backup.tar.gz /home/user/Documents/

# عمل compile بأولوية منخفضة
nice -n 10 make -j4
```

كده العملية هتشتغل 🏃 بس مش هتبطي النظام على المستخدمين التانيين 😊.[21][18]

### أمر renice - تغيير أولوية عملية جارية 🔄الأمر `renice` بيستخدم لتغيير Nice value لعملية شغالة بالفعل:[14][17][22]

#### الصيغة الأساسية:
```bash
renice [OPTIONS] -p PID
```



#### أمثلة عملية 💡

**مثال 1️⃣: تغيير أولوية عملية واحدة**
```bash
# 1. معرفة PID للعملية
ps aux | grep firefox
# أو
pgrep firefox

# 2. تغيير Nice value لـ +10 (تقليل الأولوية)
renice -n 10 -p 12345

# 3. التحقق من التغيير
ps -l -p 12345
```

الناتج هيكون: `12345 (process ID) old priority 0, new priority 10` ✅[17][22][19]

**مثال 2️⃣: زيادة أولوية عملية (يحتاج root)**
```bash
# زيادة الأولوية لـ -5
sudo renice -n -5 -p 12345
```

⚠️ **مهم:** المستخدم العادي يقدر يقلل الأولوية (يزود Nice value) بس، لكن زيادة الأولوية (تقليل Nice value) محتاجة root 👑.[22][19][17]

**مثال 3️⃣: تغيير أولوية كل عمليات مستخدم معين**
```bash
# تقليل أولوية كل عمليات المستخدم "john"
sudo renice -n 10 -u john

# التحقق
ps -lU john
```

ده مفيد لما تكون عايز تحد من استهلاك مستخدم معين للـ CPU 🎯.[19][17][22]

**مثال 4️⃣: تغيير أولوية كل عمليات مجموعة**
```bash
# تغيير أولوية كل عمليات المجموعة ذات GID = 1000
sudo renice -n 5 -g 1000

# التحقق
ps -ljg 1000
```

ده بيأثر على كل العمليات اللي تابعة لمجموعة معينة 👥.[17][19]

### استخدام top لتغيير Nice Value بشكل تفاعلي 🎮طريقة سهلة وتفاعلية:[18]

```bash
# 1. شغّل top
top

# 2. اضغط حرف 'r' (من renice)

# 3. اكتب PID للعملية اللي عايز تغيرها

# 4. اكتب Nice value الجديد

# 5. اضغط Enter
```

الطريقة دي عملية جداً 👍 وبتوريك التغيير فوراً في الواجهة ⚡.

### الفرق بين Nice و Priority 🔍**📌 ملخص سريع:**

| المصطلح | النطاق | من يحدده | الاستخدام |
|---------|--------|----------|-----------|
| **Nice Value** | -20 لـ +19 | المستخدم/Admin | لضبط الأولوية |
| **Priority (PRI)** | 0 لـ 139 | الكيرنل | للجدولة الفعلية |
| **Real-Time Priority** | 1 لـ 99 | root فقط | للعمليات الحرجة[23][24][25] |

**🎯 قواعد مهمة:**
1. Nice value بيأثر بس على Normal processes (SCHED_OTHER) ⚙️
2. Real-time processes (SCHED_FIFO, SCHED_RR) ليهم نظام أولويات مختلف تماماً 🔴
3. Real-time priority من 1 لـ 99 (99 = أعلى أولوية)[23][24][25]
4. Nice ما بيأثرش على Memory أو I/O - بس CPU فقط! 🧠[16]

### أمثلة عملية من الحياة الواقعية 🌍#### مثال 1: سيرفر backup ليلي 🌙

```bash
#!/bin/bash
# سكريبت Backup بأولوية منخفضة عشان ما يبطيش الشغل

# بدء الـ backup بـ nice value = 15
nice -n 15 tar -czf /backup/daily-$(date +%Y%m%d).tar.gz /data/

# رفع الملف للـ cloud بأولوية منخفضة
nice -n 15 aws s3 cp /backup/daily-*.tar.gz s3://my-bucket/
```

كده الـ Backup هيشتغل 🏃 بس مش هيأثر على المستخدمين 😊.[18][21]

#### مثال 2: Database Server محمّل 🗄️

```bash
# لو في عملية MySQL واخدة CPU كتير
# 1. لاقي الـ PID
pgrep -a mysql

# 2. قلل أولويتها شوية
sudo renice -n 5 -p $(pgrep mysql)

# 3. لو في query معين بطيء، زود أولويته
sudo renice -n -5 -p 98765
```

ده بيساعدك تتحكم في استهلاك الموارد بشكل ديناميكي 🎛️.[22][26]

#### مثال 3: Rendering أو Video Encoding 🎬

```bash
# عملية rendering طويلة بأولوية منخفضة
nice -n 10 ffmpeg -i input.mp4 -vcodec h264 output.mp4

# لو محتاج تسرّع العملية وسط الشغل
sudo renice -n -5 -p $(pgrep ffmpeg)
```

كده تقدر توازن بين سرعة الإنجاز واستخدام الموارد ⚖️.[21][18]

### ملاحظات وتحذيرات هامة ⚠️**🔴 تحذيرات مهمة جداً:**

1. **ما تستخدمش Nice values سالبة جداً (-15 أو أقل) إلا للضرورة القصوى!** 🚨
   - ممكن تسبب توقف النظام 🛑
   - ممكن تمنع Kernel threads من الشغل
   - ممكن تسبب Data corruption 💥[26][25]

2. **للمستخدم العادي:** 👤
   - يقدر يبدأ عمليات بـ Nice value من 0 لـ +19 فقط ✅
   - يقدر يزود Nice value لعملياته (يقلل أولويتها) ⬆️
   - **ما يقدرش** يقلل Nice value (يزود أولويتها) ❌[14][20][22]

3. **لـ root فقط:** 👑
   - يقدر يستخدم Nice values سالبة (-20 لـ -1) 🔓
   - يقدر يغير Nice value لأي عملية في النظام 🔧
   - يقدر يستخدم Real-time priorities (SCHED_FIFO, SCHED_RR) ⚡[20][22][14]

4. **Nice بيأثر على CPU time فقط:** 🧠
   - ما بيأثرش على Memory usage 🧮
   - ما بيأثرش على Disk I/O 💾
   - ما بيأثرش على Network bandwidth 🌐[16]

### التكامل بين TuneD و Nice/Renice 🤝**TuneD** و **Nice/Renice** بيشتغلوا على مستويات مختلفة:[1][8][14]

**🔸 TuneD:**
- بيشتغل على مستوى النظام كله 🖥️
- بيضبط إعدادات الكيرنل والهاردوير
- بيأثر على CPU Governor, I/O Scheduler, Network Stack
- مناسب للإعدادات الثابتة طويلة المدى 📅

**🔸 Nice/Renice:**
- بيشتغل على مستوى العملية الواحدة ⚙️
- بيغير أولوية CPU scheduling فقط
- مناسب للتحكم الديناميكي والفوري ⚡
- بيستخدم للعمليات المؤقتة 🕐

**💡 أفضل ممارسة:**
استخدم TuneD لضبط النظام عموماً 🌍، واستخدم Nice/Renice للتحكم في عمليات معينة حسب الحاجة 🎯.

### سيناريوهات عملية مُركّبة 🎭#### سيناريو 1: Web Server عليه Traffic عالي 🌐

```bash
# 1. استخدم TuneD profile مناسب
sudo tuned-adm profile network-latency

# 2. زود أولوية Apache/Nginx
sudo renice -n -5 -p $(pgrep httpd)

# 3. قلل أولوية الـ log rotation
nice -n 15 logrotate /etc/logrotate.conf
```

كده ضمنت استجابة سريعة للمستخدمين 🚀.[8][22]

#### سيناريو 2: Virtual Machine Host 🖥️

```bash
# 1. استخدم البروفايل المناسب
sudo tuned-adm profile virtual-host

# 2. زود أولوية QEMU/KVM processes
for pid in $(pgrep qemu); do
    sudo renice -n -10 -p $pid
done

# 3. قلل أولوية الـ monitoring tools
sudo renice -n 10 -u zabbix
```

ده بيحسّن أداء الـ VMs بشكل كبير 📈.[22][8]

#### سيناريو 3: Development Server 💻

```bash
# 1. بروفايل متوازن
sudo tuned-adm profile balanced

# 2. Compile كبير بأولوية منخفضة
nice -n 10 make -j$(nproc)

# 3. IDE بأولوية عادية
# (خليه على nice = 0)

# 4. Database للـ testing بأولوية منخفضة
sudo renice -n 5 -p $(pgrep postgres)
```

كده الـ development بيمشي smooth من غير ما يبطي النظام 😊.[18][8]

## خلاصة وملخص شامل 📝### النقاط الأساسية 🎯**عن TuneD:**
1. خدمة لتحسين أداء النظام تلقائياً ⚙️
2. بتشتغل بالبروفايلات المحددة مسبقاً 📋
3. فيها Static و Dynamic tuning 🔄
4. سهلة الاستخدام عن طريق `tuned-adm` 💻
5. البروفايلات موجودة في `/usr/lib/tuned/` و `/etc/tuned/` 📁[1][2][3][8][9]

**عن Nice/Renice:**
1. Nice value من -20 (أعلى أولوية) لـ +19 (أقل أولوية) 📊
2. `nice` لبدء عمليات جديدة بأولوية محددة 🚀
3. `renice` لتغيير أولوية عمليات جارية 🔄
4. المستخدم العادي ما يقدرش يزود الأولوية 👤
5. root بس اللي يقدر يستخدم قيم سالبة 👑[14][15][17][16][22]

### جدول مرجعي سريع 📋**أوامر TuneD الأساسية:**
```bash
tuned-adm list              # عرض البروفايلات
tuned-adm active            # البروفايل النشط
tuned-adm recommend         # توصية
tuned-adm profile <name>    # تفعيل بروفايل
tuned-adm off               # إيقاف
tuned-adm verify            # تحقق
```

**أوامر Nice/Renice الأساسية:**
```bash
nice -n 10 command          # بدء بأولوية منخفضة
nice -n -10 command         # بدء بأولوية عالية (root)
renice -n 5 -p PID          # تغيير أولوية عملية
renice -n 5 -u user         # تغيير لكل عمليات مستخدم
renice -n 5 -g GID          # تغيير لكل عمليات مجموعة
ps -l                       # عرض الأولويات
top                         # مراقبة تفاعلية
```

### موارد إضافية ومراجع 📚للمزيد من التفاصيل، راجع:
- Man pages: `man tuned`, `man tuned-adm`, `man nice`, `man renice`
- التوثيق الرسمي: Red Hat documentation for TuneD[1][2][8]
- ملفات الإعدادات في `/etc/tuned/`[11][6]

***

**🎉 خلصنا! 🎉**

الآن إنت عندك فهم شامل وكامل 💯 لـ TuneD وأولويات العمليات في لينكس! استخدم المعلومات دي بحكمة 🧠 عشان تحسّن أداء سيرفراتك وأنظمتك 🚀.

**تذكر دايماً:** ⚠️
- اختبر الإعدادات في بيئة تجريبية الأول 🧪
- راقب أداء النظام بعد التغييرات 📊  
- اعمل Backup قبل التعديلات المهمة 💾
- اقرأ التوثيق الرسمي دايماً 📖

**بالتوفيق! 🍀✨**

[1](https://tuned-project.org)
[2](https://rhcsa.guru/blog/?name=system-performance-tuning-rhel)
[3](https://docs.oracle.com/en/operating-systems/oracle-linux/9/tuned/tuned_command_reference.html)
[4](https://www.golinuxcloud.com/how-to-create-custom-tuned-profile-in-linux-rhel-centos-7/)
[5](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/performance_tuning_guide/sect-red_hat_enterprise_linux-performance_tuning_guide-performance_monitoring_tools-tuned_and_tuned_adm)
[6](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/monitoring_and_managing_system_status_and_performance/customizing-tuned-profiles_monitoring-and-managing-system-status-and-performance)
[7](https://docs.oracle.com/en/operating-systems/oracle-linux/9/tuned/reviewing_tuned_global_settings.html)
[8](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/monitoring_and_managing_system_status_and_performance/getting-started-with-tuned_monitoring-and-managing-system-status-and-performance)
[9](https://www.redhat.com/en/blog/linux-tuned-tuning-profiles)
[10](https://www.mankier.com/8/tuned-adm)
[11](https://man.archlinux.org/man/tuned.conf.5.en)
[12](https://docs.oracle.com/en/operating-systems/oracle-linux/7/monitoring/monitoring-WorkingWithTuned.html)
[13](https://infohub.delltechnologies.com/l/day-two-best-practices-11/red-hat-enterprise-linux-oracle-tuned-profile-3/)
[14](https://www.atlantic.net/vps-hosting/how-to-set-linux-process-priority-using-nice-and-renice-commands/)
[15](https://prepare.sh/articles/guide-to-process-priority-and-proc-filesystem)
[16](https://www.geeksforgeeks.org/linux-unix/priority-of-process-in-linux-nice-value/)
[17](https://pimylifeup.com/nice-renice-command-linux/)
[18](https://www.howtoforge.com/linux-nice-command/)
[19](https://www.geeksforgeeks.org/linux-unix/nice-and-renice-command-in-linux-with-examples/)
[20](https://cyberpanel.net/blog/nice-and-renice-command-in-linux)
[21](https://www.tecmint.com/set-linux-process-priority-using-nice-and-renice-commands/)
[22](https://www.liquidweb.com/blog/prioritize-processes-with-the-linux-nice-and-renice-commands/)
[23](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/8/html/optimizing_rhel_8_for_real_time_for_low_latency_operation/assembly_viewing-scheduling-priorities-of-running-threads_optimizing-rhel8-for-real-time-for-low-latency-operation)
[24](https://www.baeldung.com/linux/real-time-process-scheduling)
[25](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux_for_real_time/7/html/tuning_guide/setting_realtime_scheduler_priorities)
[26](https://www.redhat.com/en/blog/manipulate-process-priority)
[27](https://dpcvirtualtips.com/getting-started-with-tuned-on-rhel-8/)
[28](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/power_management_guide/tuned-adm)
[29](http://net-engine.github.io/rh-books/tuned-adm.html)
[30](https://documentation.suse.com/sles/15-SP7/html/SLES-all/cha-tuning-tuned.html)
[31](https://www.youtube.com/watch?v=oiMKrch4_PQ)
[32](https://support.hpe.com/hpesc/public/docDisplay?docId=sd00004353en_us&page=GUID-1041F932-B137-4FA2-BAF8-B70E212DC958.html&docLocale=en_US)
[33](https://notes.kodekloud.com/docs/Red-Hat-Certified-System-AdministratorRHCSA/Operate-Running-Systems/Manage-tuning-profiles)
[34](https://documentation.ubuntu.com/server/explanation/performance/perf-tune-tuned/)
[35](https://github.com/redhat-performance/tuned)
[36](https://www.ibm.com/docs/en/zos/3.1.0?topic=functions-nice-change-priority-process)
[37](https://www.scaler.com/topics/linux-nice/)
[38](https://blogs.oracle.com/linux/task-priority)
[39](https://labex.io/tutorials/linux-linux-renice-command-with-practical-examples-422885)
[40](https://linuxopsys.com/nice-command-in-linux-with-example)
[41](https://www.ibm.com/docs/en/aix/7.2.0?topic=processes-changing-priority-running-process-renice-command)
[42](https://www.youtube.com/watch?v=VjZKvkZQm1U)
[43](https://www.reddit.com/r/linux4noobs/comments/1gf5bhc/how_relevant_are_nice_and_renice_commands_in/)
[44](https://labex.io/tutorials/linux-how-to-check-if-a-performance-profile-is-active-in-linux-558892)
[45](https://caradas.com/dynamic-vs-static-adas-calibration/)
[46](https://documentation.ubuntu.com/real-time/latest/explanation/schedulers/)
[47](https://www.kernel.org/doc/html/v5.4/scheduler/sched-rt-group.html)
[48](https://www.youtube.com/watch?v=gy-WkL3FNow)
[49](https://stackoverflow.com/questions/8887531/which-real-time-priority-is-the-highest-priority-in-linux)
[50](https://www.eng-tips.com/threads/static-vs-dynamic-tuning-of-vfd-motor.269144/)
