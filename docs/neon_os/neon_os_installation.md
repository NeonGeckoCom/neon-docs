# Neon OS Installation
Neon OS is a minimal operating system currently targeting the Mycroft Mark2.

## Getting Neon OS
Neon OS is made available as downloadable images and also through distribution of
pre-installed media.

### Downloadable Image
Neon OS images are available for download from [neon.ai](https://neon.ai/NeonAIforMycroftMarkII).
The website is updated with new images as they are published and images may be written to
an 8GB or larger USB flash drive, USB SSD, or Micro SD card.

### Prepared Boot Media
If you prefer a simpler solution, NeonAI® sells prepared boot devices on 
our [square site](https://neonai.square.site/s/shop). USB and SSD drives with the latest
Neon OS are available for purchase and can be shipped internationally.

## Installing a Downloaded Image to a USB Drive
Neon OS can be installed to a USB drive or SSD to use with a Mark 2 or Raspberry
Pi device.

### Requirements
- A USB 3.0 flash drive or SSD that is at least 32GB capacity
- Raspberry Pi Imager which may be downloaded [here](https://www.raspberrypi.com/software/)
- An internet connection to download the Neon OS image

### Instructions
1. Download the latest Neon OS image from 
   [this link](https://2222.us/app/files/neon_images/pi/mycroft_mark_2/recommended_mark_2.img.xz).
2. Plug in your USB drive.
   > Everything on this drive will be erased in the next steps
3. Open [Raspberry Pi Imager](https://www.raspberrypi.com/software/). This is 
   usually just called "Imager".
4. Click "Choose OS", scroll down and select "Use custom".
5. Select the image you downloaded; this is usually `Downloads/recommended_mark_2.img.xz`.
6. Click "Choose Storage" and select the USB drive; if there are multiple drives listed,
   pick the one with a capacity matching the drive you wish to write or unplug any 
   extra drives and select the one that remains.
7. Click "Write". You may be prompted to approve the action or enter your password.
8. Wait for the Write and Verify processes to complete. Imager will display a message
   that the drive has been written successfully.
9. Remove the USB drive from your computer.
10. Make sure the Mark 2 is unplugged from power and there are no USB devices
    attached.
11. Plug the USB drive into the Mark 2.
12. Plug power into the Mark 2.
13. The Mark 2 will now boot using the drive you just created.

## Using Neon OS
To use Neon OS:
1. Turn off and unplug your Mark2.
2. Remove any USB drives and plug your Neon OS drive into the upper left (blue) USB 3 port at the back of your Mark2.
3. Plug your Mark2 back in and [get started](./using_neon_os.md)