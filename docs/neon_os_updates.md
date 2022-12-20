# NeonOS Updates
You can always download the latest NeonOS image from [neon.ai](https://neon.ai/NeonAIforMycroftMarkII).
In most cases though, you can perform in-place updates from your device.

## Updates via Skill Intent (since Neon Core 22.10.2a15)
For images as of December 20, 2022, you may check for updates by asking your
device `"are there any updates?"`. Neon will let your current Neon Core version
and if there is a newer one available. If a newer version available, you will be
given the option to install it.

## Manual Update for Installations prior to Neon Core 22.10.2a15
If you have an older version of Neon that does not include the update skill, you
may still update your device manually without having to install a new image.

### Update via SSH
If you have another computer available on the same network as your Neon device,
you can connect via SSH to start the update.
1. Ask your Neon device `"what is your IP address"`.
2. From a terminal on your other computer, `ssh neon@<device ip address`.
3. The default password for neon images is `neon`.
   You will be prompted to change this after your first login.
4. [Run the one-time update script](#run-the-one-time-script)

### Update via on-device terminal
If you have a USB keyboard available, you can open a terminal on your Neon device
to start the update.
1. Connect a USB keyboard to your Neon device.
2. Press `ctrl`+`shift`+`F1` to open a terminal.
3. Tap or click on the terminal.
4. [Run the one-time update script](#run-the-one-time-script)


### Run the one-time script
Run the following commands to download and run the one-time update script:
> ```shell
> wget https://raw.githubusercontent.com/NeonGeckoCom/neon-image-recipe/FEAT_ImagePatches/patches/add_updater_service.sh
> bash add_updater_service.sh
> ```