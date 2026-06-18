# WorkCenter Architecture v3.0

**Subdomain:** `wkc.jobazaar.net`  
**Stack:** Node.js + Express + React + SQLite → PostgreSQL  
**AI:** Claude (Anthropic API) — Sonnet 4.6  
**Version:** 3.0 | June 2026

---

## نظرة عامة

WKC نظام Modular كامل. كل Module مستقل — التواصل بين الـ Modules يتم عبر API داخلية فقط.

```
┌─────────────────────────────────────────────┐
│              wkc.jobazaar.net               │
├──────────┬──────────┬───────────┬───────────┤
│  Auth    │ Projects │   Tasks   │   Files   │
├──────────┼──────────┼───────────┼───────────┤
│  Brain   │  Search  │ Dashboard │ Timeline  │
├──────────┼──────────┼───────────┼───────────┤
│  Claude  │  Memory  │ Decisions │ Activity  │
└──────────┴──────────┴───────────┴───────────┘
```

---

## الـ Modules

| # | Module | المسؤولية | MVP |
|---|--------|----------|-----|
| 1 | Auth | Registration، Login، JWT، Invite | ✅ |
| 2 | Projects | المشاريع والأقسام | ✅ |
| 3 | Tasks | المهام، الحالات، واتساب | ✅ |
| 4 | Files | رفع الملفات، النسخ، الحالات | ✅ |
| 5 | Reviews | المراجعات، التعليقات | ✅ |
| 6 | Project Brain | ذاكرة المشروع، Vision، القرارات | ✅ |
| 7 | AI Chat | Claude بسياق المشروع الكامل | ✅ |
| 8 | Dashboard | إحصائيات ذكية | ✅ |
| 9 | Global Search | بحث عبر كل الكيانات | ✅ |
| 10 | Timeline | تاريخ القسم والمشروع | ✅ |
| 11 | Claude Engine | التحليل، توليد المهام | ✅ |
| 12 | Memory Engine | حفظ وتحديث ذاكرة المشروع | ✅ (أساس) |
| 13 | Decision Log | القرارات الموثقة | ✅ |
| 14 | Activity Log | سجل كل عملية | ✅ |
| 15 | Notifications | Push Notifications | V2 |
| 16 | Bug Tracker | تتبع الأخطاء المستقل | V2 |
| 17 | Approval Workflow | اعتماد متعدد الأشخاص | V2 |
| 18 | Version Comparison | مقارنة النسخ | V2 |
| 19 | Templates | قوالب جاهزة للمراجعة | V2 |

---

## File Structure

```
wkc/
├── backend/
│   ├── modules/
│   │   ├── auth/
│   │   │   ├── auth.routes.js
│   │   │   ├── auth.controller.js
│   │   │   ├── auth.service.js
│   │   │   └── auth.middleware.js
│   │   ├── projects/
│   │   ├── tasks/
│   │   │   └── whatsapp.service.js
│   │   ├── files/
│   │   ├── reviews/
│   │   ├── brain/
│   │   │   ├── brain.routes.js
│   │   │   └── brain.service.js
│   │   ├── search/
│   │   │   └── search.service.js
│   │   ├── timeline/
│   │   ├── claude/
│   │   │   ├── claude.service.js
│   │   │   ├── memory.service.js
│   │   │   ├── queue.service.js
│   │   │   └── prompts/
│   │   │       ├── analyze_file.txt
│   │   │       ├── analyze_section.txt
│   │   │       ├── generate_tasks.txt
│   │   │       ├── build_memory.txt
│   │   │       └── answer_with_context.txt
│   │   ├── decisions/
│   │   └── activity/
│   ├── middleware/
│   │   ├── auth.js
│   │   ├── permissions.js
│   │   └── rateLimit.js
│   ├── database/
│   │   ├── schema.sql
│   │   └── migrations/
│   └── server.js
├── frontend/
│   ├── pages/
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   ├── Dashboard.jsx
│   │   ├── Projects.jsx
│   │   ├── ProjectDetail.jsx
│   │   ├── SectionDetail.jsx
│   │   ├── TaskDetail.jsx
│   │   ├── FileDetail.jsx
│   │   ├── ReviewDetail.jsx
│   │   ├── ProjectBrain.jsx
│   │   ├── Timeline.jsx
│   │   ├── Search.jsx
│   │   ├── Team.jsx
│   │   └── Settings.jsx
│   ├── components/
│   │   ├── AIChatPanel.jsx
│   │   ├── TaskCard.jsx
│   │   ├── FileCard.jsx
│   │   └── WhatsAppButton.jsx
│   ├── hooks/
│   ├── api/
│   └── manifest.json
├── uploads/
├── .env
└── docker-compose.yml
```

---

## Tech Stack

| الطبقة | التقنية | السبب |
|--------|---------|-------|
| Backend | Node.js + Express | سريع، مرن |
| Frontend | React + Tailwind | واجهة حديثة |
| Database | SQLite → PostgreSQL | بسيط للبداية |
| Auth | JWT + bcrypt | آمن وخفيف |
| AI | Anthropic Claude API | الأفضل للكود والتحليل |
| Storage | Local → S3 لاحقاً | بسيط للبداية |
| Deploy | Docker + Nginx | wkc.jobazaar.net |
| PWA | manifest.json + SW | تطبيق على الموبايل |

---

## Pages الكاملة

| # | الصفحة | الرابط |
|---|--------|--------|
| 1 | تسجيل حساب (أول مستخدم) | `/register` |
| 2 | تسجيل الدخول | `/login` |
| 3 | Dashboard الذكي | `/dashboard` |
| 4 | المشاريع | `/projects` |
| 5 | تفاصيل المشروع | `/projects/:id` |
| 6 | تفاصيل القسم | `/projects/:id/sections/:sid` |
| 7 | تفاصيل المهمة | `/tasks/:id` |
| 8 | تفاصيل الملف | `/files/:id` |
| 9 | المراجعة | `/reviews/:id` |
| 10 | Project Brain | `/projects/:id/brain` |
| 11 | Timeline | `/projects/:id/timeline` |
| 12 | البحث العالمي | `/search` |
| 13 | Decision Log | `/projects/:id/decisions` |
| 14 | Activity Log | `/activity` |
| 15 | إدارة الفريق | `/team` |
| 16 | الإعدادات | `/settings` |
