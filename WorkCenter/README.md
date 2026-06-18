# JOBAZAAR Work Center (WKC)

> **Single Source of Truth** — أي تعديل يبدأ من هنا أولاً قبل أي كود.

**Subdomain:** `wkc.jobazaar.net`  
**Version:** 3.0  
**Last Updated:** June 2026  
**Status:** Architecture Phase — Pre-Development

---

## ما هو WKC؟

JOBAZAAR Work Center ليس Task Manager عادي.  
هو **AI Development Workspace** — نظام يمنح كل مشروع **ذاكرة حية** تتذكر كل ما حدث فيه.

```
أنت ترفع ملفات المشروع
    ↓
Claude يقرأ ويحفظ Memory
    ↓
بعد أسبوع تسأل: "وين وصلنا؟"
    ↓
Claude يجيب بدون أن تشرح شيئاً
```

---

## هيكل الوثائق

```
WorkCenter/
├── README.md                    ← هذا الملف
├── Architecture/
│   ├── WorkCenter_Architecture_v3.md   ← نظرة عامة كاملة
│   ├── Database.md              ← قاعدة البيانات والعلاقات
│   ├── APIs.md                  ← جميع الـ APIs
│   ├── Permissions.md           ← نظام الصلاحيات
│   ├── ProjectMemory.md         ← Project Memory Engine
│   ├── ClaudeIntegration.md     ← Claude AI Integration
│   ├── MVP.md                   ← نطاق المرحلة الأولى
│   └── Roadmap.md               ← خارطة الطريق
├── Decisions/
│   ├── Decision-001.md          ← workspace.jobazaar.net → wkc
│   ├── Decision-002.md          ← SQLite أولاً → PostgreSQL لاحقاً
│   └── Decision-003.md          ← wa.me بدلاً من WhatsApp API
└── UI/
    ├── Wireframes/              ← رسومات الواجهات
    ├── Screens/                 ← سكرين شوتس مرجعية
    └── UX-Notes.md              ← ملاحظات UX
```

---

## القواعد الثابتة

| القاعدة | التفاصيل |
|---------|----------|
| Single Source of Truth | هذه الوثائق هي المرجع — لا الكود |
| Markdown أولاً | التعديلات هنا قبل أي كود |
| No Breaking Changes | أي تغيير جذري يحتاج Decision جديد |
| Claude يقرأ من هنا | كل محادثة جديدة تبدأ بجلب هذه الملفات |

---

## الفريق

| الدور | المسؤولية |
|-------|----------|
| Admin (Ahmad) | القرارات النهائية، الاعتماد |
| Manager | إدارة المشاريع والأقسام |
| Member | التنفيذ والمراجعة |
| Viewer | العرض فقط |

---

## الوضع الحالي

- [x] Architecture v3.0 معتمدة
- [ ] Database Schema منفّذ
- [ ] Backend جاهز
- [ ] Frontend جاهز
- [ ] Deploy على wkc.jobazaar.net
