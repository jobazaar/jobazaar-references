# Permission System

---

## الأدوار

| الدور | الوصف |
|-------|-------|
| **admin** | صلاحيات كاملة — Ahmad |
| **manager** | إدارة المشاريع والفريق |
| **member** | تنفيذ المهام ورفع الملفات |
| **viewer** | عرض فقط — بدون تعديل |

### قاعدة الأول
أول مستخدم يسجل في النظام يصبح `admin` تلقائياً.  
بعده لا تسجيل إلا بدعوة من Admin.

---

## جدول الصلاحيات الكامل

| الصلاحية | Admin | Manager | Member | Viewer |
|---------|:-----:|:-------:|:------:|:------:|
| **Projects** |
| إنشاء مشروع | ✅ | ✅ | ❌ | ❌ |
| تعديل مشروع | ✅ | ✅ | ❌ | ❌ |
| أرشفة / حذف مشروع | ✅ | ❌ | ❌ | ❌ |
| **Sections** |
| إضافة قسم | ✅ | ✅ | ❌ | ❌ |
| تعديل قسم | ✅ | ✅ | ❌ | ❌ |
| **Tasks** |
| إضافة مهمة | ✅ | ✅ | ✅ | ❌ |
| تعديل مهمة (معينة له) | ✅ | ✅ | ✅ | ❌ |
| تغيير حالة مهمة | ✅ | ✅ | ✅ | ❌ |
| حذف مهمة | ✅ | ✅ | ❌ | ❌ |
| **Files** |
| رفع ملف | ✅ | ✅ | ✅ | ❌ |
| رفع إصدار جديد | ✅ | ✅ | ✅ | ❌ |
| تغيير حالة ملف | ✅ | ✅ | ✅ | ❌ |
| اعتماد نهائي (final) | ✅ | ❌ | ❌ | ❌ |
| **Reviews** |
| طلب مراجعة | ✅ | ✅ | ✅ | ❌ |
| إجراء مراجعة | ✅ | ✅ | ✅ | ❌ |
| **Project Brain** |
| تعديل Brain | ✅ | ✅ | ❌ | ❌ |
| إضافة ملاحظة | ✅ | ✅ | ✅ | ❌ |
| **AI** |
| تحليل Claude | ✅ | ✅ | ✅ | ❌ |
| بناء Memory | ✅ | ✅ | ❌ | ❌ |
| AI Chat | ✅ | ✅ | ✅ | ✅ |
| **Decisions** |
| إضافة قرار | ✅ | ✅ | ❌ | ❌ |
| **WhatsApp** |
| إرسال مهمة | ✅ | ✅ | ✅ | ✅ |
| **Team** |
| دعوة أعضاء | ✅ | ❌ | ❌ | ❌ |
| تغيير الأدوار | ✅ | ❌ | ❌ | ❌ |
| تعطيل حساب | ✅ | ❌ | ❌ | ❌ |
| **View** |
| عرض كل الصفحات | ✅ | ✅ | ✅ | ✅ |

---

## آلية التحقق

```javascript
// middleware/permissions.js
const permissions = {
  'project.create':  ['admin', 'manager'],
  'project.delete':  ['admin'],
  'task.create':     ['admin', 'manager', 'member'],
  'task.delete':     ['admin', 'manager'],
  'file.final':      ['admin'],
  'memory.build':    ['admin', 'manager'],
  'team.invite':     ['admin'],
};

function can(user, action) {
  return permissions[action]?.includes(user.role) ?? false;
}
```

---

## قواعد خاصة

- **Member** يقدر يعدل فقط المهام المعينة له
- **final** status للملفات: Admin فقط
- **Activity Log**: لا أحد يحذف منه — حتى Admin
- **أول مستخدم**: يصبح Admin تلقائياً بدون دعوة
