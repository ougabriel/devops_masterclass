

* * * * *

**Linux File Permissions -- Beginner-Friendly Guide**
----------------------------------------------------

Understanding file permissions is essential for secure and effective Linux system administration. This guide explains ownership, permission modes, and how to modify them step by step.

![image](https://github.com/user-attachments/assets/aab2c0ef-592f-4d1e-9a96-1bf30e4fe302)


* * * * *

### **1\. Viewing File Permissions**

```
ls -l

```


Example output:

```
-rw-r--r-- 1 user group  0 Jun 28 12:00 myfile.txt

```

#### Breakdown of `-rw-r--r--`:

| Position | Meaning | Affects |
| --- | --- | --- |
| `-` | File type (`-` = file, `d` = directory) | --- |
| `rw-` | Owner can read and write | Owner |
| `r--` | Group can only read | Group |
| `r--` | Others can only read | Others (everyone else) |

* * * * *

![image](https://github.com/user-attachments/assets/d01b9000-311d-4e99-a703-265c083f6839)


### **2\. Understanding Permission Values**

![image](https://github.com/user-attachments/assets/7f0069b9-4959-4279-81d8-c5d43b0e460e)


| Symbol | Meaning | Value |
| --- | --- | --- |
| `r` | Read | 4 |
| `w` | Write | 2 |
| `x` | Execute | 1 |
| `-` | No permission | 0 |

Examples:

-   `rw-` = 4 (read) + 2 (write) = 6

-   `r-x` = 4 (read) + 1 (execute) = 5

-   `rwx` = 4 + 2 + 1 = 7

* * * * *

### **3\. Changing File Permissions with `chmod`**

Using numeric (octal) mode:

```
chmod 755 script.sh

```

Sets:

-   Owner: read, write, execute (7)

-   Group: read, execute (5)

-   Others: read, execute (5)

```
chmod 600 secret.txt

```

Sets:

-   Owner: read, write

-   Group and Others: no permissions

* * * * *

### **4\. Symbolic Mode with `chmod`**

![image](https://github.com/user-attachments/assets/b10043d0-2228-4d63-9692-8af193afbbea)


```
chmod u+x script.sh       # Add execute permission to the owner
chmod g-w myfile.txt      # Remove write permission from the group
chmod o=r report.txt      # Set read-only permission for others

```

* * * * *

### **5\. Changing File Ownership with `chown`**

```
sudo chown alice myfile.txt

```

Changes the file owner to `alice`.

```
sudo chown bob:admin myfile.txt

```

Changes the file owner to `bob` and the group to `admin`.

* * * * *

### **6\. Understanding Directory Permissions**

To access a directory:

| Permission | Purpose |
| --- | --- |
| `x` | Enter or access the directory |
| `r` | List contents of the directory |
| `w` | Create or delete files in the directory |

All three permissions are often required to work inside a directory.

* * * * *

### **7\. Practice Lab Task**

```
mkdir permission_lab
cd permission_lab
touch file1 file2 script.sh

chmod 600 file1
chmod 644 file2
chmod 755 script.sh

ls -l

```

Discussion:

-   Who can read, write, or execute each file?

-   What does each numeric permission mean?

-   Try changing permissions using both symbolic and numeric modes.

* * * * *

### **8\. Quick Reference Summary**

| Command | Description |
| --- | --- |
| `ls -l` | Display file permissions |
| `chmod 755 file` | Set permissions using numeric mode |
| `chmod u+x file` | Add execute permission to owner |
| `chmod -R 755 folder/` | Recursively change folder permissions |
| `chown user file` | Change file owner |
| `chown user:group file` | Change file owner and group |

* * * * *

#
#
#
#
#
#
#
#
#
#
#


###PROJECT LAB

* * * * *

 **Scenario-Based Task: Mastering Linux File Permissions**
------------------------------------------------------------

### **Scenario:**

You are a junior system administrator at a company called **Gabs-Mastertech**. Your team is working on a shared project stored in `/project-data`. You are tasked with creating files, setting permissions, and ensuring secure access for different team members.

* * * * *

###  **Team Setup**

-   You (admin): user `admin`

-   Developer: user `dev`

-   Auditor: user `audit`

-   Group for the project: `projectteam`

* * * * *

###  **Objectives**

You will:

1.  Create users and group

2.  Create a project directory and files

3.  Set correct permissions based on roles

4.  Practice symbolic and numeric permission changes

5.  Verify access using `su`

* * * * *

📝 **Step-by-Step Tasks**
-------------------------

* * * * *

### ✅ **Step 1: Create Users and Group**

```
sudo groupadd projectteam
sudo useradd -m -G projectteam dev
sudo useradd -m -G projectteam audit
sudo useradd -m admin
sudo passwd dev
sudo passwd audit
sudo passwd admin

```

-   This sets up your team. `dev` and `audit` are in the same group.

* * * * *

### ✅ **Step 2: Create the Project Directory and Files**

```
sudo mkdir /project-data
sudo chown admin:projectteam /project-data
sudo chmod 770 /project-data

sudo su - admin
cd /project-data
touch report.txt script.sh secret.txt

```

* * * * *

### ✅ **Step 3: Set Specific Permissions**

#### 1\. `report.txt`

-   Should be **readable by all project members**

-   Only `admin` should be able to edit it

```
chmod 640 report.txt

```

#### 2\. `script.sh`

-   Should be **executable by everyone in the group**

-   Only `admin` can edit

```
chmod 750 script.sh

```

#### 3\. `secret.txt`

-   Should be **readable and editable only by admin**

```
chmod 600 secret.txt

```

* * * * *

### ✅ **Step 4: Test Access**

Use `su` to simulate other users:

```
su - dev
cd /project-data
cat report.txt          # should work
nano report.txt         # should fail (readonly)

bash script.sh          # should work
nano script.sh          # should fail

cat secret.txt          # should fail
exit

```

Do the same for `audit`.

* * * * *

### ✅ **Step 5: Practice Symbolic and Numeric Modes**

1.  Change permissions of `script.sh` so only the owner can read/write/execute:

```
chmod u=rwx,go= script.sh

```

1.  Reset to previous state using numeric mode:

```
chmod 750 script.sh

```

1.  View all permissions:

```
ls -l

```

* * * * *

### 🧠 **Reflection Questions**

-   What's the difference between `640` and `750`?

-   Why do we use group permissions for collaboration?

-   What would happen if `chmod 777` was used?

* * * * *

✅ **Bonus Challenge**
---------------------

Create a subfolder `/project-data/logs`:

-   Only the `dev` user should be able to write files there.

-   Everyone in `projectteam` can read files inside it.

Hints:

-   Use `chmod` and `chown`

-   Think about setting group ownership with `:projectteam`

* * * * *
