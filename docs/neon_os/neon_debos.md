# Embedded device images
This repo contains [debos](https://github.com/go-debos/debos) recipes for building base images for embedded voice assistant
devices. Images generated here are intended to include all dependencies, drivers, and OS-level configuration that an assistant
device might need, such as Camera Drivers, wifi-connect, QT packages, Vocal Fusion Drivers, etc.

## Available recipes
- debian-base-image-rpi4.yml: Debian Bookworm image with updated RPi kernel, RPi userland binaries, libcamera, 
  wifi-connect, SJ-201/Vocalfusion drivers, and a Splash Screen.

## Repository Structure
Each directory contains numbered files or directories; earlier numbers correspond to earlier build stages, and 
associate files/directories between the top-level `overlays`, `recipes`, and `scripts` directories.

Top-level `yml` files specify recipes for building images for a particular combination of base OS and platform.

- The `overlays` directory contains image root FS overlays.
- The `recipes` directory contains go-debos recipes.
- The `scripts` directory contains shell scripts that run in the image.

## Build instructions
To build a default image, use the included `run_docker_debos.sh` shell script.

Instructions for running debos natively can be found in the [official readme](https://github.com/go-debos/debos#sypnosis).