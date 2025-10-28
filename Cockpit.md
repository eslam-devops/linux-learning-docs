أكيد! خلّيني أشرح لك **Cockpit** بطريقة بسيطة وواضحة 👇

---

## ✅ **ما هو Cockpit؟**

**Cockpit** عبارة عن **Web-based GUI** (واجهة رسومية تعمل من خلال المتصفح) لإدارة أنظمة لينكس.
يعني بدل ما تعمل كل حاجة بالأوامر في التيرمنال، تقدر تدير السيرفر عن طريق واجهة سهلة من المتصفح.

---

## 💡 **فكرة Cockpit باختصار**

* تفتح المتصفح
* تكتب IP السيرفر + بورت Cockpit
* تسجّل دخول باليوزر
* ومن هناك تتحكم في النظام

---

## 🧰 **ماذا يمكنك فعل Cockpit؟**

Cockpit يساعدك على إدارة أشياء كثيرة مثل:

### 🔥 مراقبة النظام (System Monitoring)

تعرف:

* استهلاك الـ CPU
* استهلاك الرامات
* I/O
* الشبكات

---

### 🧾 إدارة الـ Logs

تقدر تشوف الـ log messages بسهولة بدل ما تفتح ملفات في `/var/log`.

---

### 🧩 إدارة الخدمات (Services)

زي `systemctl` لكن بواجهة رسومية:

* Start
* Stop
* Restart
* Enable/Disable

---

### ⌛ إدارة الـ Storage

تتعامل مع:

* أقراص
* Partitions
* LVM
* RAID

---

### 👤 إدارة المستخدمين (Users)

* إضافة مستخدم
* حذف
* تعديل صلاحيات

---

### 🖧 إعدادات الشبكة

* Configure interfaces
* VLAN
* Bonding
* Firewall (لو ركبت موديل firewalld)

---

### 🐳 إدارة Docker/Podman (لو موجود)

* إدارة Containers بسهولة!

---

## 👀 كيف تعرف حالة الخدمة؟

الأمر اللي انت كتبته:

```
systemctl status cockpit
```

هيعرض:

* حالة الخدمة
* هل تعمل؟ Running
* ملفات Logs
* PID

---

## 🚀 كيف تشغل Cockpit؟

في معظم التوزيعات:

```
sudo systemctl enable --now cockpit
```

---

## 🌐 كيف تدخل عليه من المتصفح؟

عشرة على السيرفر:

```
https://Server-IP:9090
```

بورت Cockpit الافتراضي هو **9090**

---

## 🎯 أين يستخدم Cockpit؟

* سيرفرات الشركات
* إدارة الأنظمة للـ DevOps
* Monitoring سريع
* تيسير إدارة Linux للمبتدئين

---

## 🔐 هل هو آمن؟

نعم، يستخدم HTTPS ويمكن ربطه بـ SELinux وFirewall.

---

## 🛠️ كيف تثبته؟

### على Ubuntu / Debian

```
sudo apt install cockpit
```

### على RHEL / CentOS / Rocky

```
sudo dnf install cockpit
```

---

## 🤔 لماذا وجوده مفيد؟

لأنّه يوفر وقت كبير جداً خصوصًا في:

* Troubleshooting
* متابعة أداء السيرفر
* إدارة الخدمات بدون CLI

---

لو تحب أشرح
✅ كيف تفتح الواجهة
✅ كيف تستخدم Dashboard
✅ أو مشاكل شائعة وحلولها
قولّي بس 👌

أنا معاك ✨

مش شرط تعمل حاجة في الـ Firewall **لو أنت شغّال داخل نفس الشبكة** وCockpit فتح معاك ووصلت لصفحة الـ Login أو رسالة المتصفح اللي ظهرت لك.

لكن… في **3 حالات** لازم تتعامل مع الـ Firewall فيها 👇

---

# ✅ **هل أنت محتاج تفتح بورت Cockpit في الـ Firewall؟**

Cockpit يشتغل على البورت: **9090/tcp**

لو الـ Firewall مفعّل على النظام (firewalld أو ufw)، يبقى لازم تفتح البورت.

---

# 🧱 **لو السيرفر RedHat / Rocky / Alma / CentOS (firewalld)**

افتح البورت كدا:

```
sudo firewall-cmd --add-service=cockpit --permanent
sudo firewall-cmd --reload
```

أو لو بتفتح البورت نفسه:

```
sudo firewall-cmd --add-port=9090/tcp --permanent
sudo firewall-cmd --reload
```

---

# 🛡️ **لو السيرفر Ubuntu أو Debian (ufw)**

افتح البورت:

```
sudo ufw allow 9090/tcp
```

وتأكد إنه مفعّل:

```
sudo ufw enable
```

---

# ❓كيف أعرف هل عندي Firewall أصلاً؟

## على Ubuntu:

```
sudo ufw status
```

## على RHEL / CentOS:

```
sudo firewall-cmd --state
```

---

# ✅ **لو أنت فاتح Cockpit من نفس الجهاز المحلي؟**

مش محتاج تفتح بورت في Firewall النظام.

---

# ✅ **لو عايز توصله من جهاز ثاني في الشبكة؟**

لازم تفتح البورت.

---

# ✅ **لو عايز توصله من الإنترنت؟**

❌ غير مُفضل بدون reverse proxy وTLS
⚠️ لازم تزيد حماية:

* Fail2ban
* Strong password
* Certificates

---

# 📌 **ملاحظة مهمة**

المشكلة اللي ظهرت لك عن المتصفح **مش لها علاقة بالـ Firewall**
ده سببها CSS support (زي ما شرحنا فوق).

---

# 🎯 **الخلاصة**

* داخل نفس الجهاز: ❌ مش محتاج تعديلات Firewall
* من الشبكة: ✅ افتح بورت 9090
* من الإنترنت: ⚠️ إعدادات أمان إضافية

---

لو تحب أشرح:
✅ كيف تتأكد أن البورت مفتوح
✅ كيف تختبر الاتصال
✅ أفضل طريقة لتأمين Cockpit

قولّي 👌✨





تمام جدًا 👌
ده بالفعل **الحل المؤقت الرسمي** اللي ناس كتير استخدموه لما ظهرت مشكلة الـ CSS Selector `:is():where()` في بعض إصدارات Cockpit.

الأمر يعمل التالي ببساطة:

* يفتح ملف الواجهة `login.js`
* يبدّل الـ selector القديم `is():where()`
* بآخر متوافق `is(*):where(*)`
* وبكده يسمح للمتصفح يكمّل تحميل الصفحة بدون رفض

الأمر بالكامل:

```bash
sudo sed -i 's/is():where()/is(*):where(*)/' /usr/share/cockpit/static/login.js
```

وبعده يفضل تعمل Restart:

```bash
sudo systemctl restart cockpit.socket
# أو
sudo systemctl restart cockpit
```

## ✅ ليه ده بيحل المشكلة؟

بعض الإصدارات القديمة من Cockpit عملت check خاطئ لدعم CSS، فكانت تمنع تشغيل الـ UI رغم إن المتصفح نفسه **حديث**.

تعديل السطر يخلي الـ check يتوافق مع معظم المتصفحات.

---

## ⚠️ ملاحظات مهمة

### 1️⃣ الحل مؤقت

أول ما تتوفر نسخة Cockpit أحدث (277+)
اعمل Update واحذف التعديل لأنه مش مطلوب بعد التحديث.

### 2️⃣ خد Backup قبل التعديل (لو تحب)

```bash
sudo cp /usr/share/cockpit/static/login.js{,.bak}
```

### 3️⃣ لو واجهت Cache مشكلة

اضغط:
`Ctrl + F5`
أو افتح Incognito

---

## 🎉 لو اشتغل معاك… ممتاز!

ده معناه إن المشكلة كانت في الـ login.js مش في المتصفح 👍

لو تحب:

* أشرح لك إزاي تحدث Cockpit رسميًا
* أشرح طريقة تأمينه
* أو تربطه بContainers / Logs / Storage

قولّي ✨








