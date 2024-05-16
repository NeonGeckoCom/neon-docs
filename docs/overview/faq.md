# Frequently Asked Questions
In no particular order, here are the answers to some common questions and some
common troubleshooting tasks:

### My Mark 2 device isn't responding to "Hey Mycroft"
The default wake word for Neon releases is "Hey Neon".

### There is a problem with Neon after changing configuration
If Neon is still listening, you can ask "Hey Neon, update configuration" to get
default core and skill configuration as you would during an updated.

### How can I change WiFi Networks on my Mark 2
You can swipe down from the top of the screen to open a menu. The `Wireless` option
will let you select how to configure/manage your networks.

### Network/Internet checks aren't working
Some routers/firewalls block or forward traffic that can cause Neon's network
checks to fail. DNS addresses can be specified in configuration to resolve this.
Add the following to `~/.config/neon/neon.yaml` to specify network test endpoints:

```yaml
network_tests:
  dns_primary: 1.1.1.1
  # Change this to the DNS server used by your router
  web_url: "http://nmcheck.gnome.org/check_network_status.txt"
  # Change this to a URL that can be accessed from your network
```

### Neon won't load after installing an alpha update
Sometimes alpha releases include changes to the update process that require
a second update to fully apply. If Neon doesn't appear to be fully loaded or
something isn't working as expected, try asking Neon to "update configuration".
If that doesn't work, ask Neon to "check for updates" and run the update again.

### My Mark 2 screen is blank after restarting or installing an update
There is an apparent bug in the production Mark2 display's driver that can cause
the display to not initialize. 

If this happens, you can try pressing the action button and asking Neon to shut down,
or open an SSH connection to the device and `sudo shutdown now` to power off the
Mark2. Starting in 2024, Neon OS releases include a check for this and the LED 
ring will light up yellow before the device shuts down when this happens.

After the device is shut down, unplug it and plug it back in to turn it back on.

### How can I restore Neon to a default state
Neon can be restored by going to `Settings` -> `Factory Settings` and selecting
`Factory Reset`. This will remove any configuration changes and restore Neon to
the version that was originally shipped.

If you are using a Mark2, you can also
reset by holding the `vol -` and `Action` buttons when the LED ring lights up
green on boot or red on shutdown. The LED ring will light up white to confirm 
the reset and indicate you can release the buttons.

### What is a Skill ID vs a Package Name vs a Class Name
Skills can be identified differently in different contexts. See 
[definitions](./definitions.md#skill-id)
