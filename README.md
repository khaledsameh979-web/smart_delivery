# 🚚 توصيل ذكي — Smart Delivery

منصة ويب لإدارة طلبات الشحن وتتبعها من الطلب حتى التسليم — مبنية بـ Django.

---

## 1️⃣ مقدمة المشروع (Project Overview)

**اسم المشروع:** توصيل ذكي (Smart Delivery)

**الفكرة الأساسية:** منصة ويب لإدارة شركة شحن/توصيل: تسجّل العملاء والسائقين والشحنات، وتوزّع الشحنات على السائقين، وتتابع حالة كل شحنة خطوة بخطوة (معلّقة → مُسنَدة → في الطريق → تم التسليم) مع سجل كامل يوثّق من غيّر كل حالة ومتى. وفوق ذلك مساعد ذكي (Chat Bot) داخل الموقع يقرأ بيانات النظام الحقيقية ويرد على أسئلة العميل عن شحناته.

**التقنيات المستخدمة (Tech Stack):**

| التقنية | الإصدار | الدور |
|---|---|---|
| Python | 3.12 | لغة البرمجة |
| Django | 6.1 | إطار العمل (MVT) |
| PostgreSQL | 17 | قاعدة البيانات |
| HTML / CSS | — | واجهات المستخدم (Templates + Static) |
| Git / GitHub | — | إدارة الإصدارات |

---

## 2️⃣ مميزات المشروع (Features)

- 👤 **تسجيل بدور:** الزائر يسجل عميل أو سائق (مع نوع مركبته) — والموزع يُضاف إدارياً فقط
- 🔐 **توجيه ذكي:** بعد الدخول كل مستخدم يوصل لشاشة دوره مباشرة
- 📦 **طلب شحنة:** نموذج كامل (عناوين، مركبة، ميعاد، ملاحظات) + رقم تتبع فريد UUID
- 🗺️ **تتبع عام:** أي شخص صاحب رقم التتبع يشاهد الحالة + خط زمني كامل لتغييراتها
- 📋 **شحناتي:** لوحة العميل — كل شحناته بإحصائيات وفلاتر حسب الحالة
- 🚚 **لوحة السائق:** الشحنات المسندة له فقط + أزرار تحديث ذكية تتغير حسب الحالة
- 🎯 **لوحة التوزيع:** الشحنات المعلّقة كلها + إسنادها لسائق بضغطة واحدة
- 🤖 **مساعد ذكي:** فقاعة دردشة + صفحة محادثة، بيرد على رقم الشحنة بحالتها الحقيقية من قاعدة البيانات (مع فحص ملكية)
- 📜 **سجل مساءلة:** كل تغيير حالة يُسجَّل (من → إلى + مين غيّر + إمتى) — سجّل قبل ما تغيّر
- 🛡️ **حماية بالأدوار:** كل شاشة محمية في الخادم (Server-side) لا في الواجهة

---

## 3️⃣ دليل التثبيت والتشغيل المحلي (Getting Started)

### المتطلبات الأساسية (Prerequisites)

- Python 3.12+
- PostgreSQL
- Git

### خطوات التشغيل (Step-by-Step)

```bash
# 1) استنساخ المشروع
git clone https://github.com/khaledsameh979-web/smart_delivery.git
cd smart_delivery

# 2) إنشاء بيئة افتراضية وتفعيلها
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # Linux/Mac

# 3) تثبيت المكتبات
pip install django psycopg2-binary

# 4) إنشاء قاعدة بيانات في PostgreSQL ثم ضبط الإعدادات
#    افتح mysite/settings.py وحدّث قيمة DATABASES:
#    NAME = 'smart_delivery_db'
#    USER = 'postgres'
#    PASSWORD = <كلمة مرور قاعدة بياناتك — لا تضعها في الكود أبداً>
#    HOST = '127.0.0.1' / PORT = '5432'

# 5) إنشاء الجداول
python manage.py migrate

# 6) إنشاء حساب الإدارة
python manage.py createsuperuser

# 7) تشغيل السيرفر
python manage.py runserver
```

ثم افتح: **http://127.0.0.1:8000**

> 💡 **حسابات التجربة** الموجودة في قاعدة بيانات العرض:
> زبون `mona` — سائق `omar` — موزع `dispatch1` (كلمت المرور تجريبية `demo12345`)
> وهذه الحسابات تُنشأ محلياً ولا تُنشر مع الكود.

---

## 4️⃣ هيكل المشروع (Project Structure)

```
smart_delivery/
├── manage.py                  # أداة إدارة المشروع
├── mysite/                    # إعدادات المشروع
│   ├── settings.py            # القاعدة، AUTH_USER_MODEL، LOGIN_URL
│   └── urls.py                # توجيه /admin و include للتطبيق
└── delivery/                  # التطبيق الأساسي
    ├── models.py              # النماذج الستة وعلاقاتها
    ├── views.py               # منطق كل صفحة (9 عروض)
    ├── forms.py               # نموذج الشحنة + نموذج التسجيل
    ├── urls.py                # خريطة روابط التطبيق
    ├── admin.py               # تسجيل الجداول في الأدمن
    ├── static/
    │   ├── css/style.css      # تنسيق الموقع
    │   └── js/chatbot.js      # واجهة الدردشة الحية
    └── templates/delivery/
        ├── base.html          # القالب الأب (نافبار + فوتر + ويدجت الشات)
        ├── home / login / register
        ├── request_shipment / track
        ├── driver_home.html   # لوحة السائق
        ├── customer_dashboard.html + customer_shipment_card.html  # شحناتي
        ├── dispatch_home.html # لوحة التوزيع
        └── chat_page.html     # صفحة المساعد الذكي
```

---

## 5️⃣ توثيق الـ API (API Documentation)

| الطريقة | الرابط | من يُسمح له؟ | البيانات المرسلة | الاستجابة |
|---|---|---|---|---|
| `POST` | `/api/chat/` | مسجل دخول | `message` (نص الرسالة) | `{"reply": "..."}` JSON |
| `POST` | `/register/` | زائر | `username, email, name, phone, password1, password2, role, vehicle_type` | تحويل حسب الدور |
| `POST` | `/login/` | زائر | `username, password` | تحويل حسب الدور |
| `POST` | `/request/` | عميل | `pickup_address, dropoff_address, vehicle_type, notes, scheduled_date` | تحويل لصفحة تتبع الشحنة الجديدة |
| `GET` | `/track/?number=` | الجميع | `number` (رقم التتبع) | صفحة HTML بالحالة والخط الزمني |
| `GET` | `/dashboard/?status=` | عميل | `status` (فلتر اختياري) | صفحة HTML بشحناته |
| `POST` | `/driver/` | سائق | `delivery_id, new_status` (من: IN_TRANSIT / DELIVERED / DELAYED) | تحديث الحالة + تسجيل في السجل |
| `POST` | `/dispatch/` | موزع | `delivery_id, driver_id` | إسناد الشحنة + تسجيل في السجل |

**مثال — سؤال المساعد الذكي عن شحنة:**

```bash
curl -X POST http://127.0.0.1:8000/api/chat/ \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "message=29ea6f72-1bb5-4ff0-94b0-6941f045e1c2"
```

```json
{"reply": "شحنتك (29ea6f72) حالتها الآن: Assigned — الميعاد: 2026-09-28 18:00"}
```

---

## 6️⃣ طريقة المساهمة (Contributing Guidelines)

بما أن المشروع بدأ كعمل جماعي، نتّبع الآتي:

1. **Clone لا ZIP:** اعمل `git clone` للمستودع — لا تنزّل ملف ZIP (وإلا ستحصل على مجلدات متداخلة ونسخة قديمة)
2. **اسحب أحدث نسخة قبل الشغل:** `git pull` قبل أي تعديل
3. **كل ميزة في Commit واحد واضح:** مثال: `Add driver screen: role guard + status buttons`
4. **لا ترفع أبداً:** ملفات البيئة (`venv/`) أو كلمات المرور أو نسخ قاعدة البيانات
5. **جرّب قبل الرفع:** شغّل السيرفر وتأكد أن كل الصفحات تعمل بعد تعديلاتك
6. عند إضافة ميزة كبيرة، ناقشها مع الفريق أولاً لتفادي تعارض الشغل

---

© 2026 فريق توصيل ذكي — مشروع تخرج
