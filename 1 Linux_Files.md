

````markdown
# 🧾 DevOps Task Scenario — MasterTech Incorporated

## 🏢 Company: MasterTech Incorporated  
## 🛠 Role: Junior DevOps Engineer  
## 📌 Task Title: Initial File System Setup and Configuration Audit

---

## 🧩 Background

As part of the onboarding process for a new internal monitoring tool,
MasterTech’s infrastructure team has asked you to organize and prepare
a working directory on a Linux server. This will involve understanding
file types, creating and editing configuration files,
organizing scripts and logs, and applying the correct permissions and ownership.

---

## 🎯 Objective

You are tasked with setting up a configuration directory on a Linux server that will:

- Contain various file types (scripts, logs, config)  
- Be properly owned and permissioned  
- Support easy navigation and future automation

---

## 📋 Task Checklist

### ✅ 1. Understand File Types

Use the `file` command to identify:

- Binary files  
- Text files  
- Shell scripts  

```bash
file /bin/ls
file setup.sh
````

---

### ✅ 2. Create File Templates Using `touch`

Create the following files inside `/opt/mastertech/setup/`:

```bash
mkdir -p /opt/mastertech/setup/
cd /opt/mastertech/setup/
touch deploy.sh config.cfg error.log notes.txt
```

---

### ✅ 3. Edit Files Using CLI Tools

* Use `nano` or `vim` to insert content into `config.cfg`
* Use `echo` to populate `notes.txt`
* Use `cat` to view contents of any file

```bash
nano config.cfg
echo "Initial setup notes for monitoring tool." > notes.txt
cat notes.txt
```

---

### ✅ 4. Navigate Through the File System

```bash
cd /opt/mastertech/setup/
pwd
ls -l
```

---

### ✅ 5. Copy, Move, Rename, and Delete Files

```bash
cp notes.txt /tmp/notes_backup.txt
mv error.log logs.log
rm /tmp/notes_backup.txt
```

---

### ✅ 6. Change File Permissions

Make `deploy.sh` executable:

```bash
chmod +x deploy.sh
```

Make `config.cfg` readable only by the owner:

```bash
chmod 600 config.cfg
```

---

### ✅ 7. Change Ownership of Files

Assume the deployment files should belong to the `devops` user:

```bash
sudo chown devops:devops deploy.sh config.cfg
```

---

### ✅ 8. View Hidden Files and System Files

Check your home directory:

```bash
ls -a ~
```

Edit `.bashrc` or view `.profile`:

```bash
nano ~/.bashrc
```

---

### ✅ 9. Check File Types Again

```bash
file deploy.sh
file config.cfg
```

---

## 📁 Deliverables

* Directory: `/opt/mastertech/setup/`
* Files: `deploy.sh`, `config.cfg`, `logs.log`, `notes.txt`
* Permissions: `deploy.sh` should be executable, `config.cfg` should be owner-readable only
* Ownership: All files owned by `devops:devops`

---

## ✅ Success Criteria

* All files created and organized correctly
* Permissions and ownership follow MasterTech’s security policy
* Able to navigate, edit, and manage files via CLI without GUI

---

