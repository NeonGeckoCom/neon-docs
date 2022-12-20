# neon-image-recipe
Make a Pi image from scratch.
- [Core Configuration](#core_configuration) - Base configuration of ubuntu server
- [Network Manager](#network_manager) - Configures networking to allow for wifi configuration via
  [wifi-connect](https://github.com/balena-os/wifi-connect).
- [SJ201 Support](#sj201) - Installs and configures drivers and scripts to run the SJ-201 HAT.
- [OVOS Shell](#embedded_shell) - Installs and configures the GUI shell
- [Neon Core](#neon_core) - Installs Neon Core with dependencies.
- [Dashboard](#dashboard) - Installs a web dashboard for device diagnostics
- [Camera](#camera) - Installs system dependencies for CSI Camera Support
- [Splash Screen](#splash_screen) - Configures device splash screen on boot

## Docker Automated Image Building
The included Dockerfile can be used to build a default image in a Docker environment.

The following dependencies must be installed on the build system before running the
container:

* [chroot](https://wiki.debian.org/chroot)
* [qemu-user-static](https://wiki.debian.org/QemuUserEmulation)

First, create the Docker container:
```shell
docker build . -t neon-image-builder
```

Then, run the container to create a Neon Image. Set `CORE_REF` to the branch of
`neon-core` that you want to build and `RECIPE_REF` to the branch of `neon-image-recipe`
you want to use. Set `MAKE_THREADS` to the number of threads to use for `make` processes.
```shell
sudo mknod /dev/loop99 b 7 99
sudo losetup -P /dev/loop99 {image}
# TODO: Above is just creating p1/p2 files; can this be done via mknod or something?

docker run \
-v /home/${USER}/output:/output:rw \
-v /run/systemd/resolve:/run/systemd/resolve \
-v /dev/loop99:/dev/loop99 \
-v /dev/loop99p1:/dev/loop99p1 \
-v /dev/loop99p2:/dev/loop99p2 \
-e CORE_REF=${CORE_REF:-dev} \
-e RECIPE_REF=${RECIPE_REF:-master} \
-e BASE_IMAGE=${BASE_IMAGE:-debian-base-image-rpi4} \
-e MAKE_THREADS=${MAKE_THREADS:-4} \
--privileged \
--network=host \
--name=neon-image-builder \
neon-image-builder
```

### Base Image Selection
The `BASE_IMAGE` environment variable selects which image is mounted and built on.
Valid base images are uploaded to [2222.us](https://2222.us/app/files/neon_images/pi/) and
specified by their file basename (i.e. `debian-base-image-rpi4_2022-11-08_18_50`).
The latest `debian-base-image-rpi4` is generally a good choice and is the latest
output from [neon_debos](https://github.com/neongeckocom/neon_debos).


The entire build process will generally take several hours; it takes 1-2 hours
on a build server with 2x Xeon Gold 5118 CPUs (48T Total).

### Build-Time Overlay Files
Files to be overlaid on the image file system can be placed in `/output/overlay`.
These files will be moved to the build directory and applied as described in [below](#image-overlay-files).

## Interactive Image Building
The scripts in the `automation` directory are available to help automate building a default image.
For building an image interactively, specify paths for `IMAGE_FILE` and `BUILD_DIR`
before running the below commands:

```shell
bash automation/prepare.sh "${IMAGE_FILE}" "${BUILD_DIR}"
bash /tmp/run_scripts.sh
```

### Image Overlay Files
Extra files to be included in the image can be placed in `${BUILD_DIR}/overlay`.
This directory will be recursively copied to the image at the beginning of the
build process. This may be useful for including key files or other files that
do not belong in the image recipe repository.

The below documentation describes how to manually build an image using the individual scripts in this repository.


## Getting Started
The scripts and overlay files in this repository are designed to be applied to Ubuntu Server 22.04
as the `root` user. It is recommended to apply these scripts to a clean image 
[available here](https://cdimage.ubuntu.com/releases/22.04/release/). Instructions are available
[at opensource.com](https://opensource.com/article/20/5/disk-image-raspberry-pi).

> **Note**: The GUI shell is not installable under Ubuntu 20.04 and earlier

For each step except [boot_overlay](#boot_overlay), the directory corresponding
to the step should be copied to the mounted image and the script run from a terminal
`chroot`-ed to the image. If running scripts from a booted image, they should be
run as `root`.

## Preparation
From the host system where this repository is cloned, running `prepare.sh <base_image>`
will copy boot overlay files, mount the image, mount DNS resolver config from the host system,
copy all other image overlay files to `/tmp`, and `chroot` into the image. From here, you can
run any/all of the following scripts to prepare the image before [cleaning up](#Clean-Up)

## core_configuration
Configures user accounts and base functionality for RPi. `neon` user is created with proper permissions here.

At this stage, a booted image should resize its file system to fill the drive it is flashed to. Local login and 
ssh connections should use `neon`/`neon` to authenticate and be prompted to change password on login.

## network_manager
Adds Balena wifi-connect to enable a portal for connecting the Pi device to a wifi network.

A booted image will now be ready to connect to a network via SSID `Neon`.

## sj201
For SJ201 board support, the included script will build/install drivers, add required overlays, install required system 
packages, and add a systemd service to flash the SJ201 chip on boot. This will modify pulseaudio and potentially overwrite 
any previous settings.

>*Note:* Running this scripts grants GPIO permissions to the `gpio` group. Any user that interfaces with the SJ201 board
> should be a member of the `gpio` group. Group permissions are not modified by this script

Audio devices should now show up with `pactl list`.
Audio devices can be tested in the image by recording a short audio clip and playing it back.

```shell
parecord test.wav
paplay test.wav
```

## embedded_shell
Installs ovos-shell and mycroft-gui-app. Adds and enables neon-gui.service to start the shell
on system boot.

The image should now boot to the GUI shell.

## neon_core
Installs `neon-core` and dependencies. Configures services for core modules.

At this stage, the image is complete and when booted should start Neon.

## dashboard
Installs the [OVOS Dashboard](https://github.com/openvoiceos/ovos-dashboard) and service
to start the dashboard from the GUI.

From the GUI `Settings` -> `Developer Settings` menu, `Enable Dashboard` will now start
the dashboard for remote access to device diagnostics.

## camera
Installs `libcamera` and other dependencies for using a CSI camera.

The default camera skill can be used to take a photo; `libcamera-apps` are also
installed for testing via CLI.

## splash_screen
Enables a custom splash screen and disables on-device TTY at boot.

On boot, a static image should be shown until the GUI Shell starts.

## updater
Enables an updater service to update python packages.

`systemctl start neon-updater` will stop core services, create a code backup, 
install updated Python packages, validate the core modules load properly and
optionally roll back changes, and then restart core services.

## Clean Up
`cleanup.sh` removes any temporary files from the mounted image before unmounting it.
After running `cleanup.sh`, the image is ready to burn to a drive and boot.

# Patches
The `patches` directory contains scripts used to patch existing installations to
add new features or make them compatible with updates Python modules. These files
are intended to be referenced by updater scripts and should not be used during 
image creation.

# Deprecated Scripts

## base_ubuntu_server
For Ubuntu Server base images, the included scripts install the openbox DE, add a `neon` user with default `neon` password, 
and configure the system to auto-login and disable sleep. `cleanup.sh` removes the `ubuntu` user, expires the `neon` user 
password, and schedules a device restart.