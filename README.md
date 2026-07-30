# 📚 NoteVault — PDF Notes Website

A complete PHP + MySQL website where students can browse subject-wise notes, view PDFs in-browser, and download them. Admins can upload PDFs or external links and **lock/unlock** notes per file.

---

## 📁 File Structure

```
notes-website/
│
├── index.php           Home page — subject folder grid
├── folder.php          Notes inside a subject (with lock status)
├── config.php          ← EDIT THIS: database settings
├── database.sql        Run once to set up the database
├── .htaccess
│
├── assets/
│   ├── css/style.css   Neumorphism design + day/night mode
│   └── js/main.js      Theme toggle, search, PDF modal
│
├── uploads/            Uploaded PDFs are stored here
│   └── .htaccess       Blocks PHP execution (security)
│
└── admin/
    ├── login.php       Admin login page
    ├── logout.php      Session logout
    ├── index.php       Dashboard (stats + recent files)
    ├── folders.php     Subject folder CRUD
    ├── files.php       Note upload + lock/unlock management
    └── sidebar.php     Shared sidebar include
```

---

## ⚙️ Setup — 3 Steps

### Step 1 — Import the Database

Open **phpMyAdmin** → Import tab → select `database.sql` → click Go.

Or via MySQL CLI:

```bash
mysql -u root -p < database.sql
```

---

### Step 2 — Edit config.php

```php
define('DB_HOST', 'localhost');
define('DB_USER', 'root');           // your MySQL username
define('DB_PASS', '');               // your MySQL password
define('DB_NAME', 'notes_website');
define('SITE_URL', 'http://localhost/notes-website'); // your URL
```

---

### Step 3 — Set Folder Permissions

```bash
chmod 755 uploads/
```

On **Windows XAMPP**: right-click `uploads/` → Properties → Security → give write permission.

---

## 🔑 Admin Login

| URL | Page |
|-----|------|
| `http://localhost/notes-website/` | Public website |
| `http://localhost/notes-website/admin/login.php` | Admin panel |

**Default credentials:** `admin` / `admin123`

> ⚠️ **Change the password before going live!**

To generate a new password hash:

```php
echo password_hash('your_new_password', PASSWORD_DEFAULT);
```

Then update the `admins` table in phpMyAdmin.

---

## ✅ Features

### Client (Student) Side

- Subject folder grid on home page
- Live search for subjects and notes
- Lock status badge on every note card (🔒 Locked / 🔓 Available)
- Locked notes show a message — no view or download buttons
- In-browser PDF viewer (modal) for accessible notes
- Download button
- Day / Night theme toggle (saved in localStorage)
- Neumorphism design
- Fully responsive — works on mobile, tablet, desktop

### Admin Side

- Secure login / logout
- Dashboard with stats (total notes, locked, accessible, downloads)
- Subject Folder CRUD (create, edit, delete with icon + color picker)
- Note / File Upload:
  - PDF file upload (max 50 MB)
  - External link (Google Drive, Dropbox, etc.)
  - Both PDF and link together
  - Set lock status at upload time
- **Lock / Unlock toggle** — one click per note in the file table
- Filter notes by subject and by lock status
- Auto-delete PDF file from disk when note is deleted

---

## 🔒 Lock / Unlock Feature (How It Works)

**Locked note (student view):**

- A red `🔒 Locked` badge appears on the card
- No View or Download buttons shown
- Message: *"This note is currently locked. Contact admin to get access."*

**Unlocked note (student view):**

- A green `🔓 Available` badge appears
- View and Download buttons are shown normally

**Admin control:**

- Each note in the admin file table has a `🔒 Lock` or `🔓 Unlock` button
- One click toggles the status — saved permanently in the database
- You can also set the lock status when first uploading a note
- Filter the table by "Locked" or "Accessible" to manage quickly

---

## ⚠️ Common Errors & Fixes

| # | Error | Cause | Fix |
|---|-------|-------|-----|
| 1 | Database connection failed | Wrong credentials in config.php | Double-check DB_USER, DB_PASS, DB_NAME |
| 2 | Table not found (SQLSTATE) | database.sql not imported | Run database.sql in phpMyAdmin |
| 3 | Upload failed / move_uploaded_file error | uploads/ folder has no write permission | `chmod 755 uploads/` on Linux; full control on Windows |
| 4 | Admin login not working | Password hash mismatch | Re-run database.sql or regenerate hash |
| 5 | PDF not showing in viewer | SITE_URL is wrong | Set SITE_URL to your actual domain/path in config.php |
| 6 | Google Docs viewer blank | External link is not public | On Google Drive: Share → "Anyone with the link" |
| 7 | File too large error | PHP upload limits too low | Set in php.ini: `upload_max_filesize = 50M`, `post_max_size = 55M` |
| 8 | Lock/Unlock button not working | POST not reaching PHP | Check file permissions and that the form action is correct |
| 9 | Session keeps expiring | Low PHP session lifetime | In php.ini: `session.gc_maxlifetime = 7200` |
| 10 | Site works locally but not on server | SITE_URL still set to localhost | Update SITE_URL in config.php to your live domain |

---

## 🔒 Security Notes

1. Change the default admin password immediately
2. Enable HTTPS (uncomment in `.htaccess`)
3. `uploads/.htaccess` blocks PHP execution inside uploads
4. `config.php` is blocked from direct browser access
5. All queries use PDO prepared statements — safe from SQL injection
6. File MIME type is validated server-side before saving

---

## 📋 PHP Requirements

- PHP 7.4 or higher
- MySQL 5.7+ / MariaDB 10+
- Apache with `mod_rewrite` enabled
- PHP extensions: `pdo_mysql`, `fileinfo`, `mbstring`

---

*Built with PHP, MySQL, Neumorphism CSS — designed for students.*