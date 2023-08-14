# NeonOS Updates
You can always download the latest NeonOS image from [neon.ai](https://neon.ai/NeonAIforMycroftMarkII).
In most cases though, you can perform in-place updates from your device. Updates
will generally take about 30 minutes to complete and once started, you will not
be able to use your device until updates finish.

## Updates via Skill Intent (since Neon Core 22.10.2a15)
For images as of December 20, 2022, you may check for updates by asking your
device `"are there any updates?"`. Neon will tell you the installed Neon Core version
and if there is an update available. If a newer version is available, you will be
given the option to install it.

## Manual Update for Installations (prior to Neon Core 22.10.2a15)
If you have an older version of Neon that does not include the update skill, you
may still update your device manually without having to install a new image.

### Update via SSH
If you have another computer available on the same network as your Neon device,
you can connect via SSH to start the update.
1. Ask your Neon device `"what is your IP address"` and type it somewhere you can copy & paste from.
2. From a terminal on your other computer, enter `ssh neon@<device ip address`. 
   
   *(Windows 10/11 has an app called 'Terminal' that you can find using the 'search' feature in your Start menu. It will open a terminal window for you.)*
   
   Your terminal address will differ, but here is an example:
   
   ![image](https://user-images.githubusercontent.com/100237954/209027681-6d966348-2733-451d-95a7-32a5869e950e.png)

   You might see this warning: 
   
   ![image](https://user-images.githubusercontent.com/100237954/209027884-96830a15-64b1-4871-b7e2-1e6bfb10b589.png)

   That's normal. Type in 'yes' and press enter. After the change is made, you'll be returned to where you started.
   ![image](https://user-images.githubusercontent.com/100237954/209027927-d7035228-0cc2-4325-85e6-4cade6189da0.png)

3. Type in `ssh neon@<device ip address` again to start the password update process. You'll be prompted to enter the password. The default password for Neon AI OS images is 'neon'. 
   It will not show in the terminal window when you type passwords. Type in neon, and press enter. Follow the prompts to set a new password.
![image](https://user-images.githubusercontent.com/100237954/209028532-1bb47ea8-1537-4270-a072-fa98642c09af.png)
   
4. One more time, type in `ssh neon@<device ip address` and enter your password.                                          
   Your terminal prompt should change to show that you are accessing Neon on your Mark II device. For example: 
   ![image](https://user-images.githubusercontent.com/100237954/209031943-4c8c4633-7128-4446-b514-b4709e7cf097.png)

   **Now [Run the one-time update script](#run-the-one-time-script)**

### Update via on-device terminal
If you have a USB keyboard available, you can open a terminal on your Neon device
to start the update.
1. Connect a USB keyboard to your Neon device.
2. Press `ctrl`+`shift`+`F1` to open a terminal.
3. Tap or click on the terminal.
4. [Run the one-time update script](#run-the-one-time-script)


### Run the one-time script
Run the following commands to download and run the one-time update script:
> ```
> wget https://neon.ai/one-time-update 
> bash one-time-update
> ```
   
   Example of running wget:
   
   *(The https://neon.ai/one-time-update page is a friendly url which redirects to the longer one used in the example.)*
   
   ![image](https://user-images.githubusercontent.com/100237954/209029608-cc138b16-8579-445a-aa5a-4ab033c24e9f.png)   

   **Your Mark II display should change to show the Neon logo, and words showing its status. This may take a couple minutes, or longer depending on your connection speed. When you see the home screen return, you're good to go!**

## Update Process
The update process may vary slightly as systems mature, but a description of the
components is included here.

### Neon Updates Skill
The [Neon Updates Skill](https://github.com/NeonGeckoCom/skill-update/tree/dev)
handles user requests to check for updates. If an update is requested, the skill
will emit a message to start an update which the relevant plugin will handle. 
This skill is also where settings determine if pre-release versions
are included in updates.

### Device Updater Plugin (since Neon Core 23.7.31a4)
NeonOS Releases since July 20, 2023 now use SquashFS to perform a different kind
of update. Operating System updates are managed by the 
[Device Updater Plugin](https://github.com/NeonGeckoCom/neon-phal-plugin-device-updater)
which checks configured remote paths for new SquashFS or InitramFS images to be
applied. When an update is available, the plugin downloads and applies updates;
InitramFS is applied without needing a restart, SquashFS updates require a system
reboot to be applied.

SquashFS updates will overwrite much of the root file system to make sure everything
is in a working state after updating. Any custom scripts or user files should be
kept in the `/home` directory to avoid being removed as part of the update 
process. Note that system packages may be removed during the course of an update,
so any manually installed packages may need to be re-installed after updating.

#### Advanced Usage
SquashFS updates attempt to migrate relevant information (like `/var`, `/home`, 
SSH keys, NetworkManager config, etc.) between updates, but system packages and 
other manual configuration may be removed as part of the update process. For most
users, this is helpful to clean up incidental changes and restore an installation
to a predictable state after updating. For users who want to apply customizations
that persist through updates, here are some guidelines.

- With the exception of `venv`, the user directory is not modified between updates.
  `venv` is replaced with a clean version as part of an update to ensure package
  compatibility and allow for updating python versions.
- Extra skills should be added to the user configuration file. This allows Neon core
  to manage dependency installation, and makes sure a skill is re-installed after
  system updates.
- Any extra customizations can be added to `/root/post_update`. This script will be
  run as root after an update is applied and is intended to handle any desired
  system package installation, system service configuration, etc. The system
  service does specify an interpreter, so this file should start with a `#!`,
  i.e. `#!/bin/bash` or `#!/usr/bin/python3`
- Any changes not explicitly handled during the update will be saved at
  `/opt/neon/old_overlay`. A `post_update` script may choose to do something to
  restore specific files from here.

### Core Updater Plugin
Python package updates are managed by the [Core Updater Plugin](https://github.com/NeonGeckoCom/neon-phal-plugin-core-updater)
which is configured in Neon OS to run the `neon-updater` SystemD service. The
[`neon-updater`](#neon-updater-service) service is responsible for backing up 
the current system and installing a newer version of `neon-core`.

### neon-updater Service
The `neon-updater` service comes from 
[neon-image-recipe](https://github.com/NeonGeckoCom/neon-image-recipe/blob/master/10_updater/configure_updates.sh)
and is responsible for performing Python package updates. The service will stop
all core processes, backup the current Python environment, and then update 
Python packages. After installation, the service will check that the core services
load before optionally rolling back failed changes and then restarting Neon.

