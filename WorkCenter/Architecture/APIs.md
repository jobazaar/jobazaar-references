# APIs Reference

**Base URL:** `https://wkc.jobazaar.net/api`  
**Auth:** Bearer JWT Token في كل طلب (ما عدا Login/Register)  
**Format:** JSON

---

## Auth APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| POST | `/auth/register` | أول مستخدم فقط — يصبح Admin | Public (مرة واحدة) |
| POST | `/auth/invite` | Admin يدعو عضو جديد | Admin |
| POST | `/auth/accept-invite/:token` | قبول الدعوة | Public (برابط) |
| POST | `/auth/login` | تسجيل الدخول | Public |
| POST | `/auth/logout` | تسجيل الخروج | Authenticated |
| GET | `/auth/me` | بيانات المستخدم الحالي | Authenticated |
| PUT | `/auth/password` | تغيير الباسورد | Authenticated |

---

## Projects APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/projects` | قائمة المشاريع | All |
| POST | `/projects` | إنشاء مشروع | Admin \| Manager |
| GET | `/projects/:id` | تفاصيل مشروع | All |
| PUT | `/projects/:id` | تعديل مشروع | Admin \| Manager |
| DELETE | `/projects/:id` | أرشفة مشروع | Admin |
| GET | `/projects/:id/sections` | أقسام المشروع | All |
| POST | `/projects/:id/sections` | إضافة قسم | Admin \| Manager |

---

## Tasks APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/sections/:id/tasks` | مهام القسم | All |
| POST | `/sections/:id/tasks` | إضافة مهمة | Admin \| Manager \| Member |
| GET | `/tasks/:id` | تفاصيل مهمة | All |
| PUT | `/tasks/:id` | تعديل مهمة | Admin \| Manager \| Member (معينة له) |
| PUT | `/tasks/:id/status` | تغيير حالة | Admin \| Manager \| Member (معينة له) |
| DELETE | `/tasks/:id` | حذف مهمة | Admin \| Manager |
| POST | `/tasks/bulk` | إضافة مهام متعددة (من Claude) | Admin \| Manager |
| GET | `/tasks/:id/whatsapp` | توليد رابط واتساب | All |

---

## Files APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| POST | `/sections/:id/files` | رفع ملف | Admin \| Manager \| Member |
| GET | `/files/:id` | تفاصيل ملف | All |
| POST | `/files/:id/version` | رفع إصدار جديد | Admin \| Manager \| Member |
| GET | `/files/:id/versions` | قائمة الإصدارات | All |
| PUT | `/files/:id/status` | تغيير حالة | Admin \| Manager \| Member |
| DELETE | `/files/:id` | حذف ملف | Admin \| Manager |

---

## Project Brain APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/projects/:id/brain` | جلب محتوى الـ Brain | All |
| PUT | `/projects/:id/brain` | تحديث الـ Brain | Admin \| Manager |
| POST | `/projects/:id/brain/note` | إضافة ملاحظة | Admin \| Manager \| Member |

---

## AI Chat APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| POST | `/chat/project/:id` | سؤال بسياق المشروع كامل | All |
| POST | `/chat/section/:id` | سؤال بسياق القسم | All |
| POST | `/chat/task/:id` | سؤال بسياق المهمة | All |
| GET | `/chat/project/:id/history` | تاريخ المحادثة مع المشروع | All |

---

## Claude Engine APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| POST | `/claude/analyze-file` | تحليل ملف | Admin \| Manager \| Member |
| POST | `/claude/analyze-section` | تحليل قسم | Admin \| Manager \| Member |
| POST | `/claude/generate-tasks` | توليد مهام | Admin \| Manager \| Member |
| POST | `/claude/build-memory` | بناء/تحديث Memory للمشروع | Admin \| Manager |
| POST | `/claude/review` | مراجعة أي محتوى | Admin \| Manager \| Member |

---

## Search APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/search?q=:query` | بحث عالمي | All |
| GET | `/search?q=:query&project=:id` | بحث داخل مشروع | All |
| GET | `/search?q=:query&type=task` | بحث بنوع محدد | All |

**أنواع الـ type:** `task` \| `file` \| `comment` \| `decision` \| `brain` \| `review`

---

## Timeline APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/projects/:id/timeline` | تاريخ المشروع كامل | All |
| GET | `/sections/:id/timeline` | تاريخ القسم | All |

---

## Dashboard APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/dashboard` | إحصائيات شاملة للمستخدم | Authenticated |
| GET | `/dashboard/project/:id` | إحصائيات مشروع | All |

**بيانات الـ Dashboard:**
- عدد المشاريع النشطة
- المهام المتأخرة
- بانتظار مراجعة
- آخر نشاط
- آخر تحليل Claude
- المهام المعينة للمستخدم الحالي

---

## Team APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/team` | قائمة الفريق | Admin |
| PUT | `/team/:userId/role` | تغيير الدور | Admin |
| PUT | `/team/:userId/status` | تفعيل/تعطيل | Admin |
| DELETE | `/team/:userId` | حذف عضو | Admin |

---

## Decision Log APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/projects/:id/decisions` | قرارات المشروع | All |
| POST | `/projects/:id/decisions` | إضافة قرار | Admin \| Manager |
| GET | `/decisions/:id` | تفاصيل قرار | All |

---

## Activity Log APIs

| Method | Endpoint | الوصف | الصلاحية |
|--------|----------|-------|----------|
| GET | `/activity` | كل الأنشطة | Admin |
| GET | `/activity/project/:id` | أنشطة مشروع | Admin \| Manager |
| GET | `/activity/user/:id` | أنشطة مستخدم | Admin |
