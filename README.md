# OdooAI-Windows
Precompiled pgvector for Windows with Odoo install instructions.

---

# Instructions: Odoo AI Features & pgvector for Windows

### ⚠️ Issue Overview

If you are trying to activate AI features in Odoo on Windows, you may encounter errors. This is because these features require the **pgvector** extension, which is not compatible with the PostgreSQL 12 version bundled with the standard Odoo Windows installer.

To resolve this, you must run a newer version of PostgreSQL (18.1) and manually install the pgvector extension using the files provided in this package.

---

## Step 1: Install PostgreSQL 18.1

1. Download the installer for **PostgreSQL 18.1 (Windows x86-64)** from the official EnterpriseDB website:
[https://www.enterprisedb.com/downloads/postgres-postgresql-downloads](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)
2. Run the installer:
* You may keep the default settings.
* **Note the password** you set for the `postgres` user.
* **Note the Port number** assigned (Default is `5432`, but if you have an existing Odoo installation running, it may default to `5433`).



## Step 2: Install pgvector Extension

1. Locate the `pgvector` folders included in this download package:
* `lib`
* `share`
* `include`


2. Copy these three folders.
3. Navigate to your PostgreSQL 18 installation directory.
* *Default location:* `C:\Program Files\PostgreSQL\18`


4. Paste the folders here. Windows will ask to merge them with the existing folders; **allow this action.**

## Step 3: Configure Odoo (`odoo.conf`)

1. Open your Odoo configuration file in a text editor (Notepad, VS Code, etc.).
* *Default location:* `C:\Program Files\Odoo 19.0\server\odoo.conf`


2. Locate and modify the following lines to match your new Postgres 18 setup:
**a) Change the database template (CRITICAL FIX):**
```ini
; Change from template0 to template1
db_template = template1

```


**b) Update connection settings (Match these to your Step 1 choices):**
```ini
db_port = 5432      ; (Or 5433 if 5432 was taken)
db_user = postgres
db_password = [The password you set in Step 1]

```


3. Save and close the file.

---

## Scenario Specific Steps

### Scenario A: Starting from Scratch (New Install)

1. Run the official Odoo installer.
2. When asked what to install (Server, Database, etc.), **UNCHECK "PostgreSQL Database"**.
3. Finish the installation.
4. Perform **Steps 1, 2, and 3** above.
5. Start Odoo and create your new database via the Web Manager as usual.

### Scenario B: Existing Install (Migrating from Bundled Postgres)

> **WARNING:** You **MUST** backup your database before editing the `odoo.conf` file.

1. Open your existing Odoo Web Manager.
2. Use the **Backup** function to download a `.zip` backup of your database.
3. Install Postgres 18.1 (Step 1 above).
* *Note:* Since the bundled Postgres 12 is still running, the new installer will likely select **Port 5433**. Ensure you use `5433` in your `odoo.conf`.


4. Install pgvector files (Step 2 above).
5. Edit `odoo.conf` (Step 3 above).
6. Restart the Odoo service in Windows Services.
7. Open the Odoo Web Manager. It will show no databases (because it is now looking at the new, empty Postgres 18 server).
8. Use the **Restore** function to upload the backup zip you created in step 1.
