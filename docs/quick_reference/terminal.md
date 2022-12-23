# Terminal Quick Reference
Terminal code snippets in the documentation assume a [BASH](https://en.wikipedia.org/wiki/Bash_(Unix_shell))
shell, but many commands are supported in a [Unix shell](https://en.wikipedia.org/wiki/Unix_shell)
and even Windows [PowerShell](https://en.wikipedia.org/wiki/PowerShell).

## Common Symbols
- `~` refers to the user's home directory. This is usually equivalent to `/home/$USER` and most shells will automatically fill in the real path.
- `$` at the beginning of a line indicates the command is expected to be run as a regular non-`root` user
- `#` at the beginning of a line indicates the command is expected to be run as the `root` user

## Common Shell Short-keys
- `ctrl`+`c` will kill the current process in a terminal. This can break some scripts, so be careful when interrupting processes
- `ctrl`+`d` will exit a shell; if you have an open `ssh` connection, it will close the connection or if you're just in a
  regular shell, it will exit.

## FAQ
### Typed input isn't showing up in the terminal
- First, make sure the window you're trying to type into has focus (click or tap on it).
   Usually, you'll see a blinking line or square where you're typing
- If you're entering a password, input is often not printed to the terminal for security.
   Type your password and press `Enter`; `Backspace` should work normally too.

### There's an authenticity warning or error when I try to SSH to my device
- When you try to connect to a new device via [SSH](https://en.wikipedia.org/wiki/Secure_Shell),
   your computer will ask you if this is a trusted connection; type `yes` to connect
   after making sure you have the correct IP address entered.
- If a different device is found at an address you've used before, you'll have a
  different warning like the one below that the host device is changed. You can
  follow the directions to remove the old device key with: 
  `ssh-keygen -f "/home/d_mcknight/.ssh/known_hosts" -R "<device_ip>"`
    ```
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    @    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
    @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
    IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
    Someone could be eavesdropping on you right now (man-in-the-middle attack)!
    It is also possible that a host key has just been changed.
    The fingerprint for the ED25519 key sent by the remote host is
    SHA256:A6cPMYCjIHujtDAp1uVrcMId+5416d8VOqdHIPu1Sl0.
    Please contact your system administrator.
    Add correct host key in /home/d_mcknight/.ssh/known_hosts to get rid of this message.
    Offending ECDSA key in /home/d_mcknight/.ssh/known_hosts:12
      remove with:
      ssh-keygen -f "/home/d_mcknight/.ssh/known_hosts" -R "192.168.1.142"
    Host key for 192.168.1.142 has changed and you have requested strict checking.
    Host key verification failed.
    ```
