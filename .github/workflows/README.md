---

Used this project to SSH communication:
https://github.com/appleboy/ssh-action

---

If this error occur **no tty present and no askpass program specified**
Need add the SSH user in sudoers
sudo vi /etc/sudoers
<USERNAME> ALL=(ALL) NOPASSWD: ALL

---
