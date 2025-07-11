* * * * *

✅ **END GOALS**:

----------------

> Be able to confidently use:

-   `rsync` for efficient file transfer

-   Secure root logins (SSH hardening)

-   Manage user/group permissions and advanced access (like ACLs, special bits)

* * * * *

### 🟦 **1\. RSYNC --- Efficient File Copying**

`rsync` is used to copy and sync files **locally** or **across servers**.

#### 🔸 Basic Usage (same server):

```

rsync -av /source/folder/ /destination/folder/

```

#### 🔸 Remote Transfer via SSH:

```

rsync -avz -e ssh /local/folder/ user@remote:/remote/folder/

```

| Option | Meaning |

| --- | --- |

| `-a` | Archive mode (preserves perms, timestamps, etc.) |

| `-v` | Verbose |

| `-z` | Compress data during transfer |

| `-e` | Use SSH for transport |

#### 🔸 Exclude Files:

```

rsync -av --exclude '*.log' /src/ /dest/

```

* * * * *

### 🟦 **2\. Secure Root Logins (SSH Hardening)**

**Goal**: Prevent direct root SSH access and use key-based login.

#### 🔸 Edit SSH Config

```

sudo nano /etc/ssh/sshd_config

```

Find or add the following:

```

PermitRootLogin prohibit-password

PasswordAuthentication no

PubkeyAuthentication yes

```

> ✅ `PermitRootLogin prohibit-password` allows root login **only via SSH key**, not password.

> 🔒 Use `no` to fully disable root login.

#### 🔸 Restart SSH Service:

```

sudo systemctl restart ssh

```

#### 🔸 Add your public key to `~/.ssh/authorized_keys`:

```

mkdir -p ~/.ssh

chmod 700 ~/.ssh

nano ~/.ssh/authorized_keys

# Paste your public key here

chmod 600 ~/.ssh/authorized_keys

```

* * * * *

### 🟦 **3\. Mastering User/Group Permissions**

#### ✅ **Check Current Permissions:**

```

ls -l file.txt

```

#### ✅ **Change Ownership:**

```

chown user:group file.txt

```

#### ✅ **Modify Permissions (Numeric & Symbolic):**

```

chmod 755 script.sh      # Numeric

chmod u+x file.sh        # Symbolic

```

#### ✅ **Set Special Permissions:**

| Permission | Command | Effect |

| --- | --- | --- |

| **SetUID** | `chmod 4755 script.sh` | Run as **owner** |

| **SetGID** | `chmod 2755 dir/` | Run as **group** |

| **Sticky Bit** | `chmod +t /dir/` | Only owner can delete |

* * * * *

### 🟦 **Advanced Control: ACLs**

When standard permissions aren't enough:

#### ✅ Set ACL:

```

setfacl -m u:username:rw file.txt

```

#### ✅ View ACL:

```

getfacl file.txt

```

* * * * *

✅ SUMMARY CHECKLIST

-------------------

| ✅ Task | Command |

| --- | --- |

| Create/manage users/groups | `adduser`, `groupadd`, `usermod`, `userdel` |

| Enforce password policies | `chage`, `passwd`, `/etc/login.defs` |

| Secure root access | Edit `/etc/ssh/sshd_config`, use SSH keys |

| Sync files | `rsync -avz ...` |

| Set permissions | `chmod`, `chown`, `umask`, `setfacl`, `getfacl` |

| Test restrictions | Try file operations as other users |

* * * * *
