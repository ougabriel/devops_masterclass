
---

## Teaching Plan for Beginners: Linux File Permissions, Special Bits & ACLs.

---

## 2.5: Symbolic Permissions (Letter-Based)

### Short Note:

Linux permissions control who can read, write, or execute a file or directory. There are three entities:

* User (u): the file owner
* Group (g): users in the file's group
* Others (o): all other users

Permission types:

* `r` = read
* `w` = write
* `x` = execute

---

### Command 1: Create Nested Directories

```bash
mkdir -p parent/child
```

Explanation:
Creates both the parent and child directories in one step. The `-p` flag makes parent directories if they don’t exist.

---

### Command 2: Secure File (Owner Read-Only)

```bash
chmod 400 password.txt
```

Explanation:
Gives read-only permission to the owner. Group and others have no access. Good for securing sensitive files.

---

### Command 3: Change Group Ownership

```bash
chgrp itdept password.txt
```

Explanation:
Assigns the file to the `itdept` group so users in that group can work with it.

---

## 2.6: Special Permissions

### Short Note:

Linux includes three special permission bits:

| Special Bit | Symbol | Octal | Applies To  | Purpose                                               |
| ----------- | ------ | ----- | ----------- | ----------------------------------------------------- |
| SetUID      | `s`    | `4`   | Files       | Run file with the **owner’s** permissions             |
| SetGID      | `s`    | `2`   | Files/Dirs  | Files: run with group privileges; Dirs: inherit group |
| Sticky Bit  | `t`    | `1`   | Directories | Only file owners can delete their own files           |

---

### Command 4: SetUID – Run as File Owner

Add SetUID:

```bash
chmod 4755 script.sh        # numeric
chmod u+s script.sh         # symbolic
```

Remove SetUID:

```bash
chmod 0755 script.sh        # numeric
chmod u-s script.sh         # symbolic
```

Explanation:
When executed, the file runs with the **owner’s permissions**, regardless of who runs it.

---

### Command 5: SetGID – Group Ownership Inheritance

Add SetGID:

```bash
chmod 2755 script.sh             # numeric
chmod g+s script.sh              # symbolic

chmod 2775 /project/shared       # directory example
chmod g+s /project/shared
```

Remove SetGID:

```bash
chmod 0755 script.sh             # numeric
chmod g-s script.sh              # symbolic
```

Explanation:

* On files: executed with the file's group privileges.
* On directories: new files created inherit the directory’s group.

---

### Command 6: Sticky Bit – Shared Directory Protection

Add Sticky Bit:

```bash
chmod 1777 /shared/dir           # numeric
chmod +t /shared/dir             # symbolic
```

Remove Sticky Bit:

```bash
chmod 0777 /shared/dir           # numeric
chmod -t /shared/dir             # symbolic
```

Explanation:
Prevents users from deleting or renaming each other’s files in shared directories. Common in `/tmp`.

---

### Command 7: Set Default Permissions with umask

```bash
umask 027
```

Explanation:
Controls the default permissions for newly created files and directories:

* Owner: full access
* Group: read/execute
* Others: no access

---

## 2.7: Manage ACLs (Access Control Lists)

### Short Note:

ACLs provide more flexibility than traditional permissions by allowing specific users or groups to have individual access rules.

---

### Command 8: Set ACL

```bash
setfacl -m u:username:r file.txt
```

Explanation:
Grants read access to a specific user even if they aren’t the owner or in the file’s group.

---

### Command 9: View ACL

```bash
getfacl file.txt
```

Explanation:
Displays all ACL entries for the file, showing who has what level of access.

---

## Summary Table

| Concept            | Add (Numeric) | Add (Symbolic)        | Remove (Symbolic) | Description                       |
| ------------------ | ------------- | --------------------- | ----------------- | --------------------------------- |
| SetUID             | `chmod 4755`  | `chmod u+s`           | `chmod u-s`       | File runs as owner                |
| SetGID (file)      | `chmod 2755`  | `chmod g+s`           | `chmod g-s`       | File runs as group                |
| SetGID (directory) | `chmod 2775`  | `chmod g+s`           | `chmod g-s`       | New files inherit group           |
| Sticky Bit         | `chmod 1777`  | `chmod +t`            | `chmod -t`        | Only owner can delete their files |
| Default file perms | `umask 027`   | —                     | —                 | Secure defaults for new files     |
| Set ACL            | —             | `setfacl -m u:user:r` | —                 | Allow specific user access        |
| View ACL           | —             | `getfacl file`        | —                 | Show ACL rules                    |

---

## Permission Numbers Reference

| Number | Meaning |
| ------ | ------- |
| 7      | rwx     |
| 6      | rw-     |
| 5      | r-x     |
| 4      | r--     |
| 0      | ---     |

---

## Understanding umask: File and Directory Defaults

| Item      | Default Perm    | With umask 027 | Final Permission |
| --------- | --------------- | -------------- | ---------------- |
| File      | 666 (rw-rw-rw-) | 027            | 640 (rw-r-----)  |
| Directory | 777 (rwxrwxrwx) | 027            | 750 (rwxr-x---)  |

Example:

```bash
umask 027
touch newfile
mkdir newdir
ls -l newfile newdir
```

---


