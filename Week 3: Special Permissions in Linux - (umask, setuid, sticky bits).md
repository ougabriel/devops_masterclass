

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
* **Owner (u)**
* **Group (g)**
* **Others (o)**

---

### ✅ **Command 1: Create Nested Directories**

```bash
mkdir -p parent/child
```

**Explanation:**

* `mkdir` is used to create directories.
* `-p` allows you to create **parent and child directories** at once, even if the parent doesn’t exist yet.

---

### ✅ **Command 2: Secure File (Owner Read-Only)**

```bash
chmod 400 password.txt
```

**Explanation:**

* `chmod` stands for “change mode.”
* `400` means:

  * Owner: **read only**
  * Group: **no access**
  * Others: **no access**
* Used to **secure sensitive files** like passwords.

---

### ✅ **Command 3: Change Group Ownership**

```bash
chgrp itdept password.txt
```

**Explanation:**

* `chgrp` changes the **group ownership** of a file.
* Useful when multiple users in a group (e.g., `itdept`) need access to a file.

---

## 🔹 **2.6: Special Permissions**

### 📝 **Short Note:**

Linux has three special permission bits:

1. **SetUID (Set User ID):** Run file as **owner**.
2. **Sticky Bit:** Restricts deletion in **shared folders**.
3. **umask:** Sets **default permissions** for new files/directories.

---

### ✅ **Command 4: SetUID – Run as File Owner**

```bash
chmod 4755 script.sh
```

**Explanation:**

* The `4` at the beginning activates **SetUID**.
* It allows any user to execute `script.sh` **with the file owner's permissions**.
* Useful for shared tools needing elevated rights.

---

### ✅ **Command 5: Sticky Bit – For Shared Directories**

```bash
chmod +t /shared/dir
```

**Explanation:**

* `+t` adds the **Sticky Bit**.
* In shared folders, this ensures **only the file owner can delete their files**, not others.
* Commonly used in `/tmp`.

---

### ✅ **Command 6: Set Default Permissions with umask**

```bash
umask 027
```

**Explanation:**

* `umask` sets the **default file and directory permissions** at creation.
* `027` removes:

  * **write/execute for others**
  * **write for group**
* Ensures new files are secure by default.

---

## 🔹 **2.7: Manage ACLs (Access Control Lists)**

### 📝 **Short Note:**

Standard permissions are limited (Owner, Group, Others). **ACLs** allow **fine-grained control**, letting you assign permissions to **specific users or groups**.

---

### ✅ **Command 7: Set ACL**

```bash
setfacl -m u:username:r file.txt
```

**Explanation:**

* `setfacl` modifies ACL.
* `-m` means "modify".
* `u:username:r` gives `username` **read-only access** to `file.txt`.

---

### ✅ **Command 8: View ACL**

```bash
getfacl file.txt
```

**Explanation:**

* `getfacl` displays all **ACL entries** set on a file.
* Useful for verifying who has access beyond traditional permissions.

---

## 📌 **Quick Summary Table**

| Concept                 | Command                        | Short Note                              |
| ----------------------- | ------------------------------ | --------------------------------------- |
| Create nested folders   | `mkdir -p parent/child`        | Create parent + child folders at once   |
| Secure file (read-only) | `chmod 400 file.txt`           | Only owner can read; others denied      |
| Change group            | `chgrp itdept file.txt`        | Give group ownership to `itdept`        |
| SetUID                  | `chmod 4755 script.sh`         | Run file with **owner's** privileges    |
| Sticky Bit              | `chmod +t /shared/dir`         | Only file owners can delete their files |
| Default perms           | `umask 027`                    | Controls permissions for new files      |
| Set ACL                 | `setfacl -m u:user:r file.txt` | Allow specific user to read a file      |
| View ACL                | `getfacl file.txt`             | View all users who have special access  |

---

## 📘 **Bonus Explanation: Permission Numbers**

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


