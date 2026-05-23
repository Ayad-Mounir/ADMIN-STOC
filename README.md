<div align="center">

<img src="./icon-512.png" alt="STOC Admin Logo" width="100" height="100" />

# 🔐 STOC Manager — لوحة المدير

**لوحة تحكم PWA لإدارة تراخيص تطبيق STOC**

[![PWA](https://img.shields.io/badge/PWA-Ready-blueviolet?style=for-the-badge&logo=pwa)](.)
[![Supabase](https://img.shields.io/badge/Supabase-Backend-3ECF8E?style=for-the-badge&logo=supabase)](.)
[![Version](https://img.shields.io/badge/Version-3.2.0-gold?style=for-the-badge)](.)
[![License](https://img.shields.io/badge/License-Proprietary-red?style=for-the-badge)](.)

[🔗 STOC PWA](https://github.com/Ayad-Mounir/STOC-PWA) · [🐛 الإبلاغ عن مشكلة](https://github.com/Ayad-Mounir/ADMIN-STOC/issues)

---

</div>

## 🏗️ نظرة عامة

**STOC Manager** هو لوح تحكم مخصص للمدير يعمل كـ PWA، مبني للتحكم الكامل في تراخيص تطبيق [STOC PWA](https://github.com/Ayad-Mounir/STOC-PWA). يتيح إنشاء شركات جديدة، توليد روابط وكودات QR للتفعيل، مراقبة حالة الاشتراكات، وإدارة الصلاحيات — كل ذلك عبر واجهة بسيطة من الهاتف.

---

## ✨ المميزات

### 📊 لوحة الإحصاءات
- عدد الشركات النشطة، المجمّدة، ومنتهية الصلاحية
- نظرة سريعة على حالة جميع التراخيص

### 🏢 إدارة الشركات
- بطاقة لكل شركة تعرض: الاسم، حالة الترخيص، تاريخ الانتهاء، عدد الأجهزة
- **بحث وفلترة** فوري في قائمة الشركات
- **تجميد / إلغاء تجميد** الترخيص بضغطة واحدة
- **تمديد الاشتراك** بفترات جاهزة أو تاريخ مخصص
- تعديل **عدد الأجهزة** المسموح بها لكل شركة
- حذف شركة مع تأكيد مزدوج

### 🔑 توليد روابط التفعيل
- **معالج خطوة بخطوة** لإعداد شركة جديدة:
  1. إنشاء مشروع Supabase للشركة
  2. تحقق تلقائي من صحة بيانات Supabase
  3. تحديد عدد الأجهزة والصلاحيات
  4. توليد رابط تفعيل + كود QR جاهز للطباعة
- **نسخ الرابط** أو **تحميل QR** مباشرة
- توقيع مشفر (SHA-256) لكل ترخيص

### 📡 Heartbeat — مراقبة الاتصال
- نظام Ping تلقائي يمنع توقف مشاريع Supabase (Free Tier)
- **Ping الكل** أو ping شركة بعينها يدوياً
- مؤشر حالة لكل شركة: متصل / منتهي الصلاحية / مجمّد

### 📲 PWA
- قابل للتثبيت على الهاتف والحاسوب
- شاشة قفل بكلمة مرور للمدير
- يعمل بدون إنترنت للعرض (مع sync عند الاتصال)

---

## 🛠️ التقنيات

| التقنية | الاستخدام |
|---------|-----------|
| **Vanilla JS** | بدون frameworks — سرعة قصوى |
| **Supabase JS v2** | قراءة وكتابة جدول `stoc_licenses` |
| **QRCode.js** | توليد كودات QR للتفعيل |
| **Web Crypto API** | توقيع التراخيص بـ SHA-256 |
| **Service Worker** | PWA caching |

### هيكل المشروع

```
ADMIN-STOC/
├── index.html      # التطبيق كاملاً (single-file app)
├── manifest.json   # إعدادات PWA
├── sw.js           # Service Worker
├── icon-192.png
└── icon-512.png
```

> التطبيق بأكمله في ملف `index.html` واحد — بدون dependencies أو build step.

---

## 🚀 التشغيل

### المتطلبات
- حساب Supabase يحتوي على جدول `stoc_licenses` (يُنشأ تلقائياً عند أول تشغيل)
- استضافة HTTPS (GitHub Pages، Netlify...)

### الإعداد

**1. ارفع التطبيق على استضافة HTTPS:**
```bash
git clone https://github.com/Ayad-Mounir/ADMIN-STOC.git
# ارفع على GitHub Pages أو أي استضافة ثابتة
```

**2. عند أول فتح:**
- أدخل **Supabase URL** و **Anon Key** الخاصين بمشروع STOC Admin
- التطبيق سيتحقق تلقائياً من جدول `stoc_licenses` وينشئه إن لم يكن موجوداً

**3. إنشاء جدول يدوياً (اختياري):**
```sql
CREATE TABLE IF NOT EXISTS stoc_licenses (
  code TEXT PRIMARY KEY,
  frozen BOOLEAN DEFAULT FALSE,
  expires TIMESTAMPTZ,
  last_ping TIMESTAMPTZ
);

ALTER TABLE stoc_licenses ENABLE ROW LEVEL SECURITY;

CREATE POLICY "anon_read_only" ON stoc_licenses
  FOR SELECT TO anon USING (true);

CREATE POLICY "admin_full_access" ON stoc_licenses
  FOR ALL USING (true);
```

---

## 📱 التثبيت كتطبيق

| الجهاز | الطريقة |
|--------|---------|
| **Android** | Chrome → ⋮ → "إضافة إلى الشاشة الرئيسية" |
| **iPhone/iPad** | Safari → مشاركة → "إضافة للشاشة الرئيسية" |
| **Windows/Mac** | Chrome/Edge → أيقونة التثبيت في شريط العنوان |

---

## 🔄 سير العمل الكامل

```
1. افتح STOC Manager
2. اضغط "+ إضافة شركة"
3. اتبع المعالج: Supabase → الأجهزة → التوقيع
4. أرسل رابط التفعيل أو كود QR للشركة
5. الشركة تفتح STOC PWA وتمسح الـ QR → تفعيل فوري ✅
```

---

## 🔐 الأمان

- شاشة قفل بكلمة مرور مشفرة (SHA-256) للمدير
- كل ترخيص موقّع رقمياً — لا يمكن تزويره
- التحقق من الترخيص يتم أونلاين عبر Supabase مع fallback أوفلاين

---

## 🔗 المشاريع المرتبطة

| المشروع | الوصف |
|---------|-------|
| [STOC PWA](https://github.com/Ayad-Mounir/STOC-PWA) | تطبيق إدارة المخزون (العميل) |
| **STOC Manager** | لوحة تحكم التراخيص (هذا المشروع) |

---

## 🤝 التواصل

- **GitHub:** [@Ayad-Mounir](https://github.com/Ayad-Mounir)
- **البريد الإلكتروني:** contact.ayad.mounir@gmail.com
- **واتساب:** [+212 6 53 86 76 67](https://wa.me/212653867667)

---

## 📄 الترخيص

هذا المشروع خاضع لترخيص خاص. جميع الحقوق محفوظة © 2025–2026 Ayad Mounir.

---

<div align="center">

**STOC Manager v3.2 — License Control Panel**

</div>
