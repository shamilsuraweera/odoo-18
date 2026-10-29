# Odoo 18 Development Environment Setup

Quick setup guide for running **Odoo 18 (Community + Enterprise)** on your local machine.

---

## 1. Folder Structure & Git Setup

To match the existing `.gitignore` configuration without any modifications, clone or extract the repositories directly into the designated folder names:

| Folder | Description |
| :--- | :--- |
| `odoo-c/` | Odoo 18 Community Edition source code |
| `odoo-e/` | Odoo 18 Enterprise Edition source code |
| `source_code/` | Archived source packages / tarballs (`odoo-18.0.zip`, `odoo_18.0+e.*.tar.gz`) |
| `backups/` | Pre-built database backup archive |
| `.venv/` | Python virtual environment |

### Cloning the Repositories
```bash
# Clone Odoo Community into 'odoo-c'
git clone https://github.com/odoo/odoo.git --depth 1 --branch 18.0 odoo-c

# Clone Odoo Enterprise into 'odoo-e' (requires access permissions)
git clone https://github.com/odoo/enterprise.git --depth 1 --branch 18.0 odoo-e
```

> **Note:** If you downloaded ZIP/tarball sources, extract and rename their root folders to `odoo-c` and `odoo-e` respectively.

---

## 2. Python Environment & Dependencies

1. **Create and activate a virtual environment:**
   - **Windows (PowerShell):**
     ```powershell
     python -m venv .venv
     .\.venv\Scripts\Activate.ps1
     ```
   - **Linux / macOS:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```

2. **Install required dependencies:**
   ```bash
   pip install --upgrade pip
   pip install -r odoo-c/requirements.txt
   pip install -r odoo-e/requirements.txt
   ```

---

## 3. PostgreSQL Database Setup

1. Start your local PostgreSQL service.
2. Open `psql` or **pgAdmin Query Tool** and execute the queries from [`sql_queries.sql`](sql_queries.sql):

```sql
-- Create odoo user with password
CREATE USER odoo WITH PASSWORD 'odoo';

-- Grant permission to create databases
ALTER USER odoo WITH CREATEDB;
```

---

## 4. Running Odoo

Run the server using the command in [`commands.txt`](commands.txt):

```powershell
python odoo-c/odoo-bin -c odoo.conf
```

Once started, access Odoo at: [http://localhost:8069](http://localhost:8069)

---

## 5. Restoring Included Database Backups

Navigate to the Database Manager in your browser:  
👉 [http://localhost:8069/web/database/manager](http://localhost:8069/web/database/manager)

Click **Restore Database**, enter the **Master Password** (`admin_passwd` in [`odoo.conf`](odoo.conf)), and upload the pre-packaged backup from the [`backups/`](backups) folder:

- **Fresh install** (clean Odoo 18 database)  
  📁 `backups/odoo18_2026-07-24_16-49-05-fresh.zip`

---

## 6. Helper Files Reference

- **[`sql_queries.sql`](sql_queries.sql)**: Pre-written SQL statements to create/drop the `odoo` user and the `odoo-18` database.
- **[`commands.txt`](commands.txt)**: CLI cheat sheet (venv, running the server, module install/update, scaffolding, shell, tests, pg_dump/restore, git pulls).
- **[`odoo.conf`](odoo.conf)**: Central server configuration file (addons paths, DB credentials, master password, dev mode, port settings).
