

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
