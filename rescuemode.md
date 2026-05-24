# Ubuntu 22.04 Password Reset in Rescue Mode

## Boot into GRUB Recovery

1. Reboot the server.
2. Press and hold `Shift` or `Esc` to open the GRUB menu.
3. Select the Ubuntu boot entry.
4. Press `e` to edit boot parameters.
5. Find the line starting with:

```bash
linux
```

6. Replace:

```bash
ro quiet splash
```

with:

```bash
rw init=/bin/bash
```

7. Press:

```bash
Ctrl + X
```

or

```bash
F10
```

to boot.

---

## Reset Password

Once the shell opens, run:

```bash
passwd root
```

Enter the new password twice.

If resetting another user:

```bash
passwd username
```

---

## Save Changes

```bash
sync
```

---

## Remount Filesystem

```bash
mount -o remount,ro /
```

---

## Reboot Server

```bash
/sbin/reboot -f
```

If reboot does not work:

```bash
/sbin/init 6
```

---

## Login After Reboot

Login using:

```bash
root
```

or your sudo user credentials.

---

## Fix SSH Host Key Warning (If Appears)

If SSH shows:

```bash
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

Remove the old SSH key:

```bash
ssh-keygen -f "/root/.ssh/known_hosts" -R "SERVER_IP"
```

Reconnect:

```bash
ssh root@SERVER_IP
```

Type:

```bash
yes
```

when prompted.

---

## Verify Access

Check current user:

```bash
whoami
```

Check SSH service:

```bash
systemctl status ssh
```

Check IP address:

```bash
ip a
```

---

## Optional Security Recommendation

Disable direct root SSH login after recovery.

Edit SSH config:

```bash
nano /etc/ssh/sshd_config
```

Set:

```bash
PermitRootLogin no
```

Restart SSH:

```bash
systemctl restart ssh
```