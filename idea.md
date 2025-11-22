Perfect — here is the **FULL, production-grade project architecture** for your:

✅ Golang Interactive CLI
✅ Laravel Backend API
✅ MySQL Database
✅ Email Provider (Resend)
✅ Local Cache
✅ Ready for future Web UI

Everything is structured cleanly so you can build + deploy without confusion.

---

# 📦 **1. GO CLI FOLDER STRUCTURE**

```
bulkmailer-cli/
│
├── cmd/
│   ├── root.go
│   ├── login.go
│   ├── upload.go
│   ├── send.go
│   ├── status.go
│   └── config.go
│
├── internal/
│   ├── api/
│   │   ├── client.go
│   │   ├── auth.go
│   │   ├── upload.go
│   │   ├── send.go
│   │   └── status.go
│   │
│   ├── cache/
│   │   ├── cache.go
│   │   └── sqlite.go
│   │
│   ├── csv/
│   │   ├── parser.go
│   │   └── validator.go
│   │
│   ├── config/
│   │   └── config.go
│   │
│   └── ui/
│       └── interactive.go   # for interactive menus (e.g., choose CSV, confirm send, etc.)
│
├── pkg/
│   └── util.go
│
├── go.mod
├── go.sum
└── README.md
```

### 🧠 Why this structure?

* `cmd/` = CLI commands
* `internal/` = real logic
* Separation = clean, testable code
* Easy to add more commands later

---

# 🌐 **2. LARAVEL API FOLDER STRUCTURE**

(Inside your hosting)

```
bulkmailer-api/
│
├── app/
│   ├── Http/
│   │   ├── Controllers/
│   │   │   ├── AuthController.php
│   │   │   ├── UploadController.php
│   │   │   ├── EmailController.php
│   │   │   └── StatusController.php
│   │   ├── Middleware/
│   │   └── Requests/
│   │
│   ├── Models/
│   │   ├── EmailList.php
│   │   ├── EmailRecord.php
│   │   └── Batch.php
│   │
│   ├── Services/
│   │   ├── Email/
│   │   │   ├── ResendAPI.php
│   │   │   └── MailerSendAPI.php
│   │   └── CSV/
│   │       └── CSVImporter.php
│   │
│   ├── Jobs/
│   │   ├── SendEmailJob.php
│   │   └── ProcessCSVJob.php
│   │
│   └── Traits/
│       └── ApiResponse.php
│
├── routes/
│   └── api.php
│
├── database/
│   ├── migrations/
│   ├── seeders/
│   └── factories/
│
├── config/
│   ├── mail.php
│   └── services.php
│
└── composer.json
```

---

# 🗄 **3. DATABASE SCHEMA (MySQL)**

### `email_lists`

Stores the parsed CSV rows.

| id | batch_id | name | email | meta_json | created_at |
| -- | -------- | ---- | ----- | --------- | ---------- |

### `batches`

Each upload creates one batch.

| id | name | total | sent | failed | created_at |

### `email_records`

Each sent email is logged here.

| id | batch_id | email | status | provider_message | created_at |

---

# 🔌 **4. API ROUTES**

### **Authentication**

```
POST /api/login
```

### **CSV Upload**

```
POST /api/upload
```

### **Email Sending**

```
POST /api/send
```

### **Sending Status**

```
GET /api/status/{batchId}
```

### **List Emails**

```
GET /api/emails/{batchId}
```

---

# 🚀 **5. COMPLETE WORKFLOW WITH THESE FOLDERS**

## **Step 1: User runs CLI**

```
bulkmailer login
```

Stores API key locally.

---

## **Step 2: Upload CSV**

```
bulkmailer upload members.csv
```

Golang CLI:

* parse CSV
* send to Laravel API
* store local cache

Laravel:

* store CSV rows in `email_lists`
* create batch entry

---

## **Step 3: Send emails**

```
bulkmailer send --batch 12
```

Laravel:

* queue job (SendEmailJob)
* send via Resend API
* update `email_records`

---

## **Step 4: Check status**

```
bulkmailer status --batch 12
```

Laravel returns JSON:

```
{
  "sent": 120,
  "failed": 5,
  "total": 150
}
```

CLI displays progress bar.

---

# 💾 **6. LOCAL CACHE STRUCTURE (SQLite)**

In user home directory:

```
~/.bulkmailer/cache.sqlite
```

Tables:

```
config
email_lists_cache
batches_cache
logs_cache
```

---

# 📎 **7. Optional Web App (Later)**

Because backend is API-based, you can add:

* Next.js frontend
* Laravel Blade
* Flutter
* React Native

All can consume the *same API*.

---

# 🎁 **If you want, I can generate next:**

1. **Go CLI command code (root.go + login.go etc.)**
2. **Laravel controllers boilerplate**
3. **Database migrations**
4. **API Request + Response format**
5. **Resend integration code**

Just say:
👉 *"Generate the Go CLI"*,
👉 *"Generate the Laravel API"*, or
👉 *"Generate everything."*
