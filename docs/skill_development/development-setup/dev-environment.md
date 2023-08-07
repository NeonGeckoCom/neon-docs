# Setting Up Your Neon Development Environment

Before you can complete your skill you'll need a Neon environment in which to test it. It's not time-efficient to do your initial development inside of existing Neon hardware like a Mycroft Mark II, but fully testing all portions of a skill requires that hardware.

The Mycroft Mark II [can be purchased from Neon AI](https://neonai.square.site/product/neon-ovos-mycroft-ai-mark-ii/1?cs=true) or [from Mycroft AI](https://mycroft.ai/product/mark-ii/), and is also offered by Neon AI as a bounty for skills development.
If you're interested in receiving a Mark II for skills development, email [Clary@Neon.AI](mailto:Clary@Neon.AI) describing your skill development plans, or to ask about skills that are priorities for us right now

This section details some of the ways you can set up a Neon development environment and makes some recommendations on the best way to use each one.

## Recommended options

### Containers

[Neon Core](https://github.com/NeonGeckoCom/NeonCore) comes with a `docker-compose` file that has all of the necessary components for a minimal Neon setup. At the time of this writing, it does not fully support the GUI, due to issues with serving QML files over a network connection. A minimal GUI does exist.

The NeonCore repository has directions for running each Neon service in a container. Note that the Neon containers will take ~8GB of disk space and that must all be downloaded, so the first time you run `docker-compose up -d` it will take time.

The containerized option is best for development and testing, since the feedback loop is usually shorter than installing a skill and running it on Neon smartspeaker hardware like a Mycroft Mark 2. Due to the limited GUI support, however, it is not a great option for heavy GUI development and testing.

For now, Neon only distributes AMD64 Docker images. Each service can be `git clone`d locally and the Docker image can be built on other architectures such as ARM64 (Mac OS running Apple Silicon). This is necessary in order to use a Linux VM running Docker containers on Apple Silicon. Images must also be built locally to run Neon containers on a Raspberry Pi. Neon does not recommend a Raspberry Pi for Docker containers for Neon due to limited hardware, but it is possible if you build the images for the correct architecture.

### Laptop installation/VM image

The next recommended option for a development environment is running directly on a Linux laptop or VM. This setup is similar to running in Docker containers, but since everything is local to one machine, it is much better for serious GUI development.

For directions on installing a Dev or User environment directly to a Linux laptop or VM, [please see the Neon website](https://neon.ai/NeonSDKInstallationInstructions). Be aware that installing from scratch here is a long process. You can also [purchase a bootable USB drive with Neon pre-installed](https://neonai.square.site/product/NeonAIMycroftMarkIIBootableUSBDrive). Just be aware that this drive is optimized for a Mycroft Mark 2 and will require some configuration adjustments before it will work fully on other hardware.

Finally, there are [Neon images available for Raspberry Pi](https://2222.us/app/files/neon_images/pi/). If you do choose the develop directly on a Raspberry Pi this is the simplest way to get started. Download your preferred image, use imaging software such as Balena Etcher to prepare a USB or MicroSD drive, and insert it into the Pi. Be aware that not all Raspberry Pi models will boot from USB automatically, so you may need to change BIOS/UEFI settings to allow it.

### Mark 2 or Dev Kit

If you have a Mycroft Mark 2 or Mark 2 Dev Kit, [Neon has images available for download](https://2222.us/app/files/neon_images/pi/mycroft_mark_2/) as well as [bootable USB drives for sale](https://neonai.square.site/product/NeonAIMycroftMarkIIBootableUSBDrive). Neon does not recommend trying to install from scratch since the Mark 2 is highly specialized hardware.

## Minimal environment

If you're developing in containers or in an environment with limited resources, note that you do not need to run every single Neon service for it to run properly. The minimum services required are the `neon-messagebus` and `neon-skills` services if you only want to test the text portions of your code. This can make for a quick feedback loop using `neon-cli` or `mana`.

To add TTS and STT, `neon-audio` and `neon-speech` are required, respectively.

Finally, `neon-gui` is required only to test out any GUI functionality your skill may require, and `neon-enclosure` is required only if you are using a PHAL plugin or need to interact with Neon hardware in some way.

### Minimum for standard VUI skills

In order:

- neon-messagebus
- neon-skills

Optional unless testing STT/TTS:

- neon-audio
- neon-speech

Semi-optional:

- neon-enclosure (unless you're working with a PHAL plugin or a VUI associated with a PHAL plugin)
- neon-gui (unless you're doing GUI work)

### Minimum for VUI skills for PHAL plugins

In order:

- neon-messagebus
- neon-enclosure
- neon-skills

## Shortening the feedback loop

Installing a skill and restarting the `neon-skills` service can take a long time. This is especially true if you have a large number of skills to load or end up needing to restart other Neon services. Besides the minimal setup tips above, here are some tips to shorten the development feedback loop while writing a skill:

1. **Keep your logic separate from the skill itself.** In addition to just being good development practice, this makes it very easy to test key logic for your code without having to restart the skills service. You also get the added benefit of your code being easier to unit test.
   - Example: In the Plex skill, the code to interact with the Plex API and construct dictionaries for OVOS Common Play is in a separate file from the skill itself. That code is also broken into multiple small functions to allow for easy unit testing.
1. **Take advantage of Neon's testing automation.** Unit tests for your skill's core logic are highly recommended. However, you don't need to unit test Neon-specific behavior in most cases. We handle that for you with automated test suites. For more information [see the documentation on available GitHub Actions tests](../voice-user-interface-design-guidelines/skill-testing.md).
1. **Develop on your fastest, smallest environment first.** All of the options explored in this page are valid options for creating a development environment. However, you may find it more efficient to do initial development and testing in the smallest and fastest environment possible, e.g. Docker containers on a laptop/desktop. After you've done all the testing you can there, a copy of your skill can be installed in a different environment like a Mycroft Mark 2 for live testing.
1. **Exclude default skills.** The default `docker-compose` setup pulls `neon_skills-default_skills`. If your skill development does not require them, you can change it to `neon_skils` instead. This excludes default skills and only loads the mounted skills directory. Loading skills often takes significant time.
