# Claude Integration Architecture

> الـ Claude Module مستقل تماماً — يمكن تعطيله أو استبداله بدون تأثير على بقية النظام.

---

## الطبقات

```
┌─────────────────────────────────┐
│         Claude Module           │
├─────────────────────────────────┤
│  ClaudeService.js               │  ← الواجهة الوحيدة
│  MemoryService.js               │  ← بناء السياق
│  ClaudeQueue.js                 │  ← إدارة الطلبات
│  ClaudeCache.js                 │  ← تجنب التكرار
│  ClaudeResponseParser.js        │  ← تحويل الردود
│  prompts/                       │  ← القوالب
└─────────────────────────────────┘
```

---

## ClaudeService.js

```javascript
class ClaudeService {
  async analyzeFile(fileContent, projectContext) {}
  async analyzeSection(sectionData, projectContext) {}
  async generateTasks(analysisResult) {}
  async buildMemory(projectFiles) {}
  async chatWithContext(question, fullContext) {}
  async reviewContent(content, reviewType) {}
}
```

**قاعدة:** لا أحد يستدعي Anthropic API مباشرة — كلهم يمروا من `ClaudeService`.

---

## MemoryService.js — بناء السياق

```javascript
async function buildFullContext(projectId) {
  const memory    = await getLatestMemory(projectId);
  const brain     = await getProjectBrain(projectId);
  const decisions = await getRecentDecisions(projectId, 10);
  const openTasks = await getOpenTasks(projectId);

  return {
    memory,
    brain,
    decisions,
    openTasks,
    builtAt: new Date()
  };
}
```

---

## Prompts

| الملف | الاستخدام |
|-------|----------|
| `analyze_file.txt` | تحليل ملف HTML/PHP/JS |
| `analyze_section.txt` | تحليل قسم كامل |
| `generate_tasks.txt` | توليد مهام من تحليل |
| `build_memory.txt` | بناء ذاكرة المشروع |
| `answer_with_context.txt` | الإجابة بسياق كامل |
| `review_ux.txt` | مراجعة UX |
| `review_mobile.txt` | فحص الموبايل |

---

## AI Chat Flow

```
المستخدم يكتب سؤالاً
        ↓
MemoryService.buildFullContext(projectId)
        ↓
ClaudeQueue.enqueue(question + context)
        ↓
Anthropic API (claude-sonnet-4-6)
        ↓
ClaudeResponseParser.parse(response)
        ↓
حفظ في chat_history
        ↓
إرجاع الجواب للمستخدم
```

---

## Queue & Rate Limiting

```javascript
// ClaudeQueue.js
const queue = new PQueue({ concurrency: 2 }); // طلبان متزامنان فقط
const RATE_LIMIT = 50; // طلب / دقيقة

// تجنب تجاوز الـ API limits
```

---

## Cache

```javascript
// ClaudeCache.js
// تحليل نفس الملف مرتين → يرجع النتيجة المحفوظة
// Cache يستند على: hash(fileContent + promptType)
// TTL: 24 ساعة
```

---

## Model

```
Model:      claude-sonnet-4-6
Max Tokens: 4000
Temperature: 0 (للتحليل) | 0.3 (للـ Chat)
```

---

## متغيرات البيئة

```env
ANTHROPIC_API_KEY=sk-ant-...
CLAUDE_MODEL=claude-sonnet-4-6
CLAUDE_MAX_TOKENS=4000
CLAUDE_CACHE_TTL=86400
```
