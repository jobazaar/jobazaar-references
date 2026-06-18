# Database Structure

**Engine:** SQLite (Development) → PostgreSQL (Production)  
**Strategy:** UUID لكل كيان — لا Auto-increment

---

## جدول Users

```sql
CREATE TABLE users (
  id              TEXT PRIMARY KEY,        -- UUID
  name            VARCHAR(100) NOT NULL,
  email           VARCHAR(255) UNIQUE NOT NULL,
  password_hash   VARCHAR(255) NOT NULL,   -- bcrypt rounds=12
  role            TEXT CHECK(role IN ('admin','manager','member','viewer')),
  phone           VARCHAR(20),             -- رقم واتساب
  avatar_url      VARCHAR(500),
  is_active       BOOLEAN DEFAULT true,
  is_first_user   BOOLEAN DEFAULT false,   -- أول مستخدم = Admin تلقائي
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  last_login      TIMESTAMP
);
```

---

## جدول Invite Tokens

```sql
CREATE TABLE invite_tokens (
  id          TEXT PRIMARY KEY,
  email       VARCHAR(255) NOT NULL,
  role        TEXT NOT NULL,
  token       VARCHAR(255) UNIQUE NOT NULL,
  created_by  TEXT REFERENCES users(id),
  expires_at  TIMESTAMP NOT NULL,          -- 48 ساعة
  used_at     TIMESTAMP                    -- null = لم يُستخدم
);
```

---

## جدول Projects

```sql
CREATE TABLE projects (
  id          TEXT PRIMARY KEY,
  name        VARCHAR(200) NOT NULL,
  description TEXT,
  status      TEXT DEFAULT 'active' CHECK(status IN ('active','paused','completed','archived')),
  owner_id    TEXT REFERENCES users(id),
  color       VARCHAR(7) DEFAULT '#2E6DA4',
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Project Brain

```sql
CREATE TABLE project_brain (
  id          TEXT PRIMARY KEY,
  project_id  TEXT UNIQUE REFERENCES projects(id),  -- واحد لكل مشروع
  vision      TEXT,
  notes       TEXT,
  important_links  TEXT,                 -- JSON array
  important_codes  TEXT,                 -- JSON array
  open_questions   TEXT,                 -- JSON array
  best_practices   TEXT,
  updated_by  TEXT REFERENCES users(id),
  updated_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Project Memory

```sql
CREATE TABLE project_memory (
  id              TEXT PRIMARY KEY,
  project_id      TEXT REFERENCES projects(id),
  version         INTEGER DEFAULT 1,
  summary         TEXT,                  -- ملخص Claude للمشروع
  completed_features  TEXT,             -- JSON array
  pending_features    TEXT,             -- JSON array
  known_bugs          TEXT,             -- JSON array
  technical_debt      TEXT,             -- JSON array
  architecture_notes  TEXT,
  last_analyzed_at    TIMESTAMP,
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Sections

```sql
CREATE TABLE sections (
  id          TEXT PRIMARY KEY,
  project_id  TEXT REFERENCES projects(id),
  name        VARCHAR(200) NOT NULL,
  description TEXT,
  order_index INTEGER DEFAULT 0,
  status      TEXT DEFAULT 'active' CHECK(status IN ('active','completed','archived')),
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Tasks

```sql
CREATE TABLE tasks (
  id                TEXT PRIMARY KEY,
  section_id        TEXT REFERENCES sections(id),
  title             VARCHAR(500) NOT NULL,
  description       TEXT,
  status            TEXT DEFAULT 'todo' CHECK(status IN ('todo','in_progress','review','done','cancelled')),
  priority          TEXT DEFAULT 'medium' CHECK(priority IN ('low','medium','high','urgent')),
  assigned_to       TEXT REFERENCES users(id),
  created_by        TEXT REFERENCES users(id),
  due_date          TIMESTAMP,
  source            TEXT DEFAULT 'manual' CHECK(source IN ('manual','claude_generated')),
  parent_task_id    TEXT REFERENCES tasks(id),  -- للمهام الفرعية
  whatsapp_sent_at  TIMESTAMP,
  created_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Files

```sql
CREATE TABLE files (
  id              TEXT PRIMARY KEY,
  section_id      TEXT REFERENCES sections(id),
  name            VARCHAR(300) NOT NULL,
  file_type       TEXT CHECK(file_type IN ('html','image','pdf','json','zip','other')),
  storage_path    VARCHAR(1000) NOT NULL,
  file_size       INTEGER,
  version         INTEGER DEFAULT 1,
  parent_file_id  TEXT REFERENCES files(id),  -- null = نسخة أصلية
  status          TEXT DEFAULT 'draft' CHECK(status IN ('draft','review','needs_changes','approved','final')),
  uploaded_by     TEXT REFERENCES users(id),
  created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Reviews

```sql
CREATE TABLE reviews (
  id            TEXT PRIMARY KEY,
  file_id       TEXT REFERENCES files(id),
  requested_by  TEXT REFERENCES users(id),
  assigned_to   TEXT REFERENCES users(id),
  status        TEXT DEFAULT 'pending' CHECK(status IN ('pending','in_review','approved','needs_changes')),
  due_date      TIMESTAMP,
  notes         TEXT,
  created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  completed_at  TIMESTAMP
);
```

---

## جدول Comments

```sql
CREATE TABLE comments (
  id                TEXT PRIMARY KEY,
  entity_type       TEXT CHECK(entity_type IN ('task','file','review','section','brain')),
  entity_id         TEXT NOT NULL,
  user_id           TEXT REFERENCES users(id),
  content           TEXT NOT NULL,
  parent_comment_id TEXT REFERENCES comments(id),
  created_at        TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Decision Log

```sql
CREATE TABLE decision_log (
  id          TEXT PRIMARY KEY,
  project_id  TEXT REFERENCES projects(id),
  section_id  TEXT REFERENCES sections(id),  -- nullable
  title       VARCHAR(500) NOT NULL,
  description TEXT,
  reason      TEXT,
  decided_by  TEXT REFERENCES users(id),
  created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## جدول Activity Log

```sql
CREATE TABLE activity_log (
  id           TEXT PRIMARY KEY,
  user_id      TEXT REFERENCES users(id),
  action       VARCHAR(100) NOT NULL,  -- مثال: task.created
  entity_type  TEXT,
  entity_id    TEXT,
  metadata     TEXT,                   -- JSON
  ip_address   VARCHAR(45),
  created_at   TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  -- لا يمكن الحذف من هذا الجدول
);
```

---

## جدول Search Index

```sql
CREATE VIRTUAL TABLE search_index USING fts5(
  entity_type,
  entity_id,
  project_id,
  title,
  content,
  tokenize='unicode61'
);
-- يُحدَّث تلقائياً عبر Triggers
```

---

## العلاقات

```
users
  ├── projects (owner_id)
  ├── tasks (assigned_to, created_by)
  ├── files (uploaded_by)
  ├── reviews (requested_by, assigned_to)
  ├── comments (user_id)
  ├── decision_log (decided_by)
  └── activity_log (user_id)

projects
  ├── sections (project_id)
  ├── project_brain (project_id) [1:1]
  ├── project_memory (project_id)
  └── decision_log (project_id)

sections
  ├── tasks (section_id)
  └── files (section_id)

files
  ├── files (parent_file_id) [versioning]
  └── reviews (file_id)

tasks
  └── tasks (parent_task_id) [subtasks]

comments [polymorphic]
  → task | file | review | section | brain
```
