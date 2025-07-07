
---

# 🧑‍🏫 **Teaching Plan for Beginners: Linux File Permissions, Special Bits & ACLs**

---

## 🔹 **2.5: Symbolic Permissions (Letter-Based)**

### 📝 **Short Note:**

Linux uses **permissions** to control who can **read**, **write**, or **execute** a file or directory. Symbolic permissions use letters:

* `r` = read
* `w` = write
* `x` = execute

Each file has permissions for:

* **User (u)** – the file owner
* **Group (g)** – the assigned user group
* **Others (o)** – everyone else

---

### ✅ **Command 1: Create Nested Directories**

```bash
mkdir -p parent/child
```

**Explanation:**

* `mkdir` creates directories.
* `-p` makes all required parent directories, if they don’t exist.

---

### ✅ **Command 2: Secure File (Owner Read-Only)**

```bash
chmod 400 password.txt
```

**Explanation:**

* `chmod` changes file permissions.
* `400` = owner can read (`r`), group/others have no access.
* Useful for securing sensitive files.

---

### ✅ **Command 3: Change Group Ownership**

```bash
chgrp itdept password.txt
```

**Explanation:**

* `chgrp` changes the group owner of a file.
* Useful when files need to be accessed by team members in a shared group.

---

## 🔹 **2.6: Special Permissions**

### 📝 **Short Note:**

In addition to the standard `rwx` permissions, Linux has **three special permission bits** that give extra behavior:

1. **SetUID (Set User ID):** Run executable files as the **file owner**.
2. **SetGID (Set Group ID):**

   * On files: Run as the **file group**.
   * On directories: New files inherit the **parent’s group**.
3. **Sticky Bit:** Only the **file owner** can delete their files in shared folders.

---

### ✅ **Command 4: SetUID – Run as File Owner**

```bash
chmod 4755 script.sh
```

**Explanation:**

* `4` enables the SetUID bit.
* When another user runs this file, it runs with the **file owner's permissions**.
* You’ll see `s` instead of `x` for the owner in `ls -l`.

---

### ✅ **Command 5: SetGID – Run or Inherit Group Ownership**

```bash
chmod 2755 sharedscript.sh         # SetGID on file
chmod 2775 /project/sharedfolder   # SetGID on directory
```

**Explanation:**

* `2` enables the SetGID bit.
* On **files**: Executed as the **file group owner**, not the user’s group.
* On **directories**: New files created inside **inherit the group** of the folder (not the creator’s default group).
* This promotes consistent **group collaboration**.

---

### ✅ **Command 6: Sticky Bit – For Shared Directories**

```bash
chmod +t /shared/dir
```

**Explanation:**

* The `+t` flag sets the Sticky Bit.
* Only **the file’s owner** can delete or rename their files in the directory.
* Commonly used in public folders like `/tmp`.

---

### ✅ **Command 7: Set Default Permissions with umask**

```bash
umask 027
```

**Explanation:**

* `umask` controls **default permissions** for new files and directories.
* `027` means:

  * Owner gets full access (`rwx`)
  * Group gets read/execute
  * Others get no access

---

## 🔹 **2.7: Manage ACLs (Access Control Lists)**

### 📝 **Short Note:**

Linux ACLs allow you to assign **fine-grained permissions** beyond the basic owner/group/others model. You can give **specific users or groups** custom read, write, or execute access.

---

### ✅ **Command 8: Set ACL**

```bash
setfacl -m u:username:r file.txt
```

**Explanation:**

* `setfacl` sets ACL permissions.
* `-m` modifies the ACL.
* This command gives `username` **read-only** access to `file.txt`.

---

### ✅ **Command 9: View ACL**

```bash
getfacl file.txt
```

**Explanation:**

* `getfacl` shows the full ACL permission list.
* Useful to verify what users or groups have access to a file.

---

## 📌 **Quick Summary Table**

| Concept                 | Command                        | Short Note                             |
| ----------------------- | ------------------------------ | -------------------------------------- |
| Create nested folders   | `mkdir -p parent/child`        | Make parent & child folders in one go  |
| Secure file (read-only) | `chmod 400 file.txt`           | Only owner can read; others blocked    |
| Change group            | `chgrp itdept file.txt`        | Assign file to `itdept` group          |
| **SetUID**              | `chmod 4755 script.sh`         | File runs with **owner's privileges**  |
| **SetGID (file)**       | `chmod 2755 sharedscript.sh`   | File runs with **group privileges**    |
| **SetGID (directory)**  | `chmod 2775 /shared/dir`       | New files inherit **group**            |
| **Sticky Bit**          | `chmod +t /shared/dir`         | Only file owner can delete their files |
| Set default permissions | `umask 027`                    | Controls new file/dir access           |
| Set ACL                 | `setfacl -m u:user:r file.txt` | Grant specific user access             |
| View ACL                | `getfacl file.txt`             | Show extended permissions              |

---

## 📘 **Bonus: Permission Numbers Explained**

Each permission is represented by a number:

* `r` = 4
* `w` = 2
* `x` = 1

So:

* `7` = `rwx`
* `6` = `rw-`
* `5` = `r-x`
* `4` = `r--`
* `0` = `---`

---

## 🧮 **Understanding umask (How Defaults Work)**

| Type        | Base Permission   | Affected by umask |
| ----------- | ----------------- | ----------------- |
| Files       | `666` (rw-rw-rw-) | Yes               |
| Directories | `777` (rwxrwxrwx) | Yes               |

**Example:**

```bash
umask 027
touch file.txt
mkdir dir
ls -l file.txt dir
```

### umask 027 Calculation:

* File: `666 - 027 = 640` → `rw-r-----`
* Dir:  `777 - 027 = 750` → `rwxr-x---`

---


