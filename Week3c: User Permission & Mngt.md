

* * * * *

**User Permissions and Management in Linux**
--------------------------------------------

### **Overview**

Linux is a multi-user operating system that manages access and control through a well-defined **user and group permission system**. Understanding how users are created, grouped, and granted specific access to files and directories is essential for maintaining system security and resource organization.

* * * * *

### **User Management**

-   **Users** represent individual accounts on the system. Each user has a **unique username** and **user ID (UID)**.

-   **User accounts** can be created using `useradd` or `adduser`, modified with `usermod`, and removed using `userdel`.

-   Users can be assigned to one or more **groups**, which help define shared permissions across multiple accounts.

----
![USERS, GROUPS, AND PERMISSIONS IN LINUX | by perfectogo | Medium](https://miro.medium.com/v2/resize:fit:1200/1*pI94lVONL4p54Lh3icTz6g.png)


#### Common User Management Commands:

```
useradd john              # Create a new user
passwd john               # Set a password for the user
usermod -aG groupname john  # Add user to an existing group
deluser john              # Delete a user account

```

* * * * *

### **Group Management**

-   A **group** is a collection of users that share the same access rights.

-   Groups help manage permissions for files, directories, and system resources more efficiently.

-   You can create a group with `groupadd`, and users can be added using `usermod` or directly via `/etc/group`.

* * * * *

### **File Permissions**

Linux files and directories are assigned three types of permissions for three categories of users:

| Category | Description |
| --- | --- |
| Owner | The user who owns the file |
| Group | The group assigned to the file |
| Others | All other users |

Each can have the following permissions:

| Permission | Symbol | Description |
| --- | --- | --- |
| Read | `r` | View file contents or list directory |
| Write | `w` | Modify file contents or directory |
| Execute | `x` | Run the file or enter a directory |

Permissions can be viewed using `ls -l` and modified using `chmod`, `chown`, or `chgrp`.

* * * * *

### **Example:**

```
-rw-r--r-- 1 john developers 1234 Jan 1 12:00 file.txt

```

-   `john` is the owner.

-   `developers` is the group.

-   Permissions: owner can read/write, group can read, others can read.

* * * * *

### **Conclusion**

Proper user and permission management ensures that system resources are secure and only accessible to authorized users. It is a foundational skill for system administrators and anyone managing a Linux environment such as Devops Engineers.

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


🎯 **Scenario: Setting up a New Developer Team on a Shared Linux System**
-------------------------------------------------------------------------

You are a DevOps Admin. Your task is to onboard a new developer (`alice`), 
assign her to the right group (`devteam`), set up permissions and security policies, 
and ensure everything is correctly managed.

* * * * *

🧩 **Step-by-Step Tasks (Following Class Outline)**
---------------------------------------------------

* * * * *

### 🔹 **2.1: Create Local Users & Groups**

#### ✅ **Task**: Create a new user called `alice` with a custom group.

1.  **Create the user `alice`**

```
sudo adduser alice

```

1.  **Check Alice's user and group info**

```
id alice

```

1.  **Create a custom group `devteam` with specific GID**

```
sudo groupadd -g 5000 devteam

```

1.  **Assign Alice to the `devteam` group**

```
sudo usermod -aG devteam alice

```

1.  **Set password for Alice**

```
sudo passwd alice

```

1.  **Change default password policy (system-wide)**

```
sudo nano /etc/login.defs

```

-   Example changes:

    ```
    PASS_MAX_DAYS   90
    PASS_MIN_DAYS   1
    PASS_WARN_AGE   7

    ```

1.  **Remove a user (for example, old dev named `bob`)**

```
sudo userdel -r bob

```

1.  **Delete an unused group**

```
sudo groupdel oldgroup

```

* * * * *

### 🔐 **MAGIC POWER: Privileges & sudo**

#### ✅ **Task**: Give Alice sudo power securely.

1.  **Add Alice to the `sudo` group**

```
sudo usermod -aG sudo alice

```

1.  **Edit sudo rules securely**

```
sudo visudo

```

-   Ensure there's a line:

    ```
    alice ALL=(ALL) ALL

    ```

1.  **To restrict a user (e.g., remove sudo)**

```
sudo deluser alice sudo

```

* * * * *

### 🔄 **2.2: Modify Users, Groups & Password Aging**

#### ✅ **Task**: Set password expiration and audit settings.

1.  **Switch to Alice's account**

```
su - alice

```

1.  **Check Alice's password aging policy**

```
sudo chage -l alice

```

1.  **Force her to change password at next login**

```
sudo passwd -e alice

```

1.  **Set password to expire every 90 days**

```
sudo chage -M 90 alice

```

1.  **Change Alice's primary group to `devteam`**

```
sudo usermod -g devteam alice

```

* * * * *

### 🌐 **2.3: Use External Authentication Service (LDAP)**

#### ✅ **Task**: Integrate system with LDAP (for enterprise login)

1.  **Launch GUI auth config (if available)**

```
sudo authconfig-gtk

```

1.  **Configure LDAP**

-   Set LDAP server, base DN, enable LDAP authentication.

1.  **Verify with LDAP user (e.g., `ldapuser`)**

```
su - ldapuser

```

* * * * *

### 🧾 **2.4: Numeric Permissions**

#### ✅ **Task**: Create a script and secure it.

1.  **Create a file**

```
touch script.sh

```

1.  **Make it executable for all (rwx for owner, rx for others)**

```
chmod 755 script.sh

```

1.  **View permissions**

```
ls -l script.sh

```

1.  **Try running it as different users (unauthorized) to test restriction**

* * * * *

### 🔤 **2.5: Symbolic Permissions**

#### ✅ **Task**: Lock a file to owner only.

1.  **Create nested directories**

```
mkdir -p projects/projectA

```

1.  **Create sensitive file**

```
touch password.txt

```

1.  **Make it read-only for owner**

```
chmod 400 password.txt

```

1.  **Change its group to `devteam`**

```
sudo chgrp devteam password.txt

```

* * * * *

### ⚙️ **2.6: Special Permissions**

#### ✅ **Task**: Set ownership behavior and shared directories.

1.  **SetUID: Allow script to always run as owner**

```
chmod 4755 script.sh

```

1.  **Create shared dir for team collaboration**

```
mkdir /shared
sudo chmod 1777 /shared

```

1.  **Set umask for secure default perms**

```
umask 027

```

* * * * *

### 🧩 **2.7: ACL Management**

#### ✅ **Task**: Give `bob` read-only access to Alice's file.

1.  **Set ACL on file**

```
setfacl -m u:bob:r password.txt

```

1.  **View ACL**

```
getfacl password.txt

```

* * * * *

✅ **End-of-Class Test Case (Optional Practice)**
------------------------------------------------

-   Create `testuser`, `qa`, `hr` groups.

-   Add `testuser` to both groups.

-   Create file `confidential.txt`, make it readable only by owner.

-   Give read access to `qa` group using ACL.

-   Force password change on next login for `testuser`.

-   Make a shared directory writable by all but deletable only by owner.

* * * * *
