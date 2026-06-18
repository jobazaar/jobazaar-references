# Project Memory Engine

> أهم Module في النظام — هو ما يجعل WKC مختلفاً عن أي Task Manager.

---

## الفكرة الجوهرية

```
كل مشروع = ذاكرة حية
Claude يقرأ الذاكرة قبل أي سؤال
أنت لا تشرح شيئاً من الصفر
```

---

## ما تحتويه الذاكرة

```json
{
  "project_id": "uuid",
  "version": 4,
  "summary": "Plugin WordPress خاص بـ Jobazaar...",
  "completed_features": [
    "Vendor Registration Wizard",
    "Custom Dashboard",
    "Product Upload (4 steps)",
    "Store Verification (4 levels)"
  ],
  "pending_features": [
    "Activity Log",
    "Withdraw API",
    "Notifications System"
  ],
  "known_bugs": [
    "Camera upload fails on iOS 16"
  ],
  "technical_debt": [
    "Refactor vendor-api.php — too large"
  ],
  "architecture_notes": "Built on WooCommerce + Dokan Pro hooks only...",
  "last_analyzed_at": "2026-06-18T19:00:00Z"
}
```

---

## كيف تُبنى الذاكرة

### طريقة 1 — رفع ملفات المشروع

1. ترفع ZIP أو ملفات فردية
2. Claude يقرأ الكود
3. يستخرج المكونات، الميزات، المشاكل
4. يحفظ في `project_memory`

### طريقة 2 — تحديث يدوي

Admin أو Manager يضيف ملاحظات يدوياً في Project Brain.  
بعدها Claude يدمجها في الذاكرة.

### طريقة 3 — تحديث تلقائي

بعد كل تحليل Claude، يُحدَّث الـ Memory تلقائياً.

---

## كيف يستخدمها Claude في AI Chat

```
عند أي سؤال على المشروع:

1. جلب آخر memory من project_memory
2. جلب Brain notes من project_brain
3. جلب آخر 10 قرارات من decision_log
4. جلب المهام المفتوحة من tasks
5. بناء Context كامل
6. إرسال للـ Claude API مع السؤال
```

**النتيجة:** Claude يجيب كأنه يعرف المشروع منذ البداية.

---

## مثال عملي

**أنت تسأل:**
> وين وصلنا بـ jobazaar-core؟

**Claude يجيب (من الذاكرة):**
```
آخر تحديث: 18 يونيو 2026

تم إنجازه ✅
- Vendor Registration Wizard
- Custom Dashboard (14 screens)
- Product Upload Wizard
- Store Verification (4 levels)

بقي ⏳
- Activity Log
- Withdraw API
- Notifications

مشاكل معروفة ⚠️
- Camera upload على iOS 16

آخر قرار: استخدام WooCommerce hooks فقط
```

---

## Prompt بناء الذاكرة

```
// prompts/build_memory.txt

أنت محلل كود خبير. اقرأ هذا المشروع واستخرج:

1. ملخص المشروع (3-4 جمل)
2. الميزات المكتملة (قائمة)
3. الميزات الناقصة (قائمة)
4. الأخطاء المعروفة (قائمة)
5. الـ Technical Debt (قائمة)
6. ملاحظات معمارية مهمة

أجب بـ JSON فقط بالشكل التالي:
{
  "summary": "",
  "completed_features": [],
  "pending_features": [],
  "known_bugs": [],
  "technical_debt": [],
  "architecture_notes": ""
}
```

---

## Versioning للذاكرة

كل مرة تُبنى الذاكرة → تُحفظ نسخة جديدة.  
يمكن الرجوع لأي نسخة سابقة.

```
Memory v1 — June 10
Memory v2 — June 15
Memory v3 — June 18 (الحالية)
```

---

## الفرق بين Memory و Brain

| | Project Brain | Project Memory |
|--|--------------|---------------|
| من يكتبه | الإنسان | Claude (تلقائي) |
| المحتوى | Vision، قرارات، روابط | تحليل الكود، الميزات |
| متى يُحدَّث | يدوياً | بعد كل تحليل |
| الهدف | ذاكرة بشرية | ذاكرة تقنية |

**Claude يقرأ الاثنين معاً قبل أي جواب.**
