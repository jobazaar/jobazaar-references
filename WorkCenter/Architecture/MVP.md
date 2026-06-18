# MVP Scope — المرحلة الأولى

**الهدف:** نظام شغال كامل على `wkc.jobazaar.net` في أسرع وقت.

---

## ما في الـ MVP ✅

### Auth & Team
- [x] Registration (أول مستخدم = Admin)
- [x] Invite System (Admin يدعو الفريق)
- [x] Login / Logout / JWT
- [x] 4 أدوار: admin، manager، member، viewer

### Projects & Sections
- [x] إنشاء وتعديل المشاريع
- [x] إضافة وترتيب الأقسام

### Tasks
- [x] إضافة وتعديل المهام
- [x] 5 حالات: todo → in_progress → review → done → cancelled
- [x] 4 أولويات: low، medium، high، urgent
- [x] تعيين لأعضاء الفريق
- [x] مهام فرعية
- [x] WhatsApp Sharing (wa.me link)

### Files
- [x] رفع الملفات (HTML، صور، PDF، ZIP)
- [x] 5 حالات: draft → review → needs_changes → approved → final
- [x] File Versioning (نسخ متعددة)

### Reviews & Comments
- [x] طلبات مراجعة
- [x] تعليقات على أي كيان

### Project Brain ⭐
- [x] Vision المشروع
- [x] ملاحظات مهمة
- [x] روابط وأكواد مرجعية
- [x] أسئلة مفتوحة

### Project Memory ⭐
- [x] بناء الذاكرة من الملفات
- [x] تحديث تلقائي بعد كل تحليل
- [x] حفظ نسخ متعددة

### AI Chat بسياق كامل ⭐
- [x] Chat داخل كل مشروع
- [x] Claude يقرأ Memory + Brain + Decisions قبل الجواب
- [x] تاريخ المحادثة محفوظ

### Smart Dashboard ⭐
- [x] المهام المعينة للمستخدم
- [x] المهام المتأخرة
- [x] بانتظار مراجعة
- [x] آخر نشاط
- [x] آخر تحليل Claude

### Global Search ⭐
- [x] بحث عبر: Tasks، Files، Comments، Decisions، Brain
- [x] فلترة بالنوع والمشروع

### Timeline ⭐
- [x] تاريخ المشروع والقسم كاملاً
- [x] كل حدث بتوقيته ومن نفّذه

### Claude Engine
- [x] تحليل ملفات
- [x] تحليل أقسام
- [x] توليد مهام تلقائياً
- [x] بناء Project Memory

### Decision Log
- [x] توثيق القرارات بالسبب

### Activity Log
- [x] سجل كل عملية (لا يُحذف)

### PWA
- [x] يُضاف لشاشة الموبايل كتطبيق

---

## ما ليس في الـ MVP ❌

| الميزة | الإصدار |
|--------|---------|
| Push Notifications | V2 |
| Bug Tracker مستقل | V2 |
| Approval Workflow متعدد | V2 |
| HTML Preview داخلي | V2 |
| Version Comparison (Diff) | V2 |
| Templates جاهزة | V2 |
| AI Memory كاملة | V2 |
| WhatsApp Business API | V2 |
| تطبيق APK | V3 |
| SaaS (Multi-tenant) | V3 |

---

## ترتيب التنفيذ

```
1. Auth + Registration + Invite
2. Projects + Sections
3. Tasks + WhatsApp
4. Files + Versioning
5. Project Brain
6. Claude Engine + Memory
7. AI Chat
8. Dashboard + Search + Timeline
9. Reviews + Comments
10. Decision Log + Activity Log
11. PWA + Deploy
```
