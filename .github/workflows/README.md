# SSH project to communication

https://github.com/appleboy/ssh-action

# If this error occur **no tty present and no askpass program specified**
Need add the SSH user in sudoers
sudo vi /etc/sudoers
<USERNAME> ALL=(ALL) NOPASSWD: ALL

# Startup configuration on Ubunutu [REFS](https://askubuntu.com/questions/814/how-to-run-scripts-on-start-up)

```shell
sudo vi /etc/systemd/system/platiagro.service
[Unit]
Description=PlatIAgro Install

[Service]
ExecStart=/home/cpqd/install.sh
Type=oneshot
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reload
sudo systemctl enable platiagro.service
```

Check output log with:
journalctl -e -u platiagro.service