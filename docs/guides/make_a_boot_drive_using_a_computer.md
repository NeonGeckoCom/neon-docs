# Make a Boot Drive Using Your Computer
How to use your Windows computer to make a USB or SSD boot drive for your Mark II.

## Requires
A portable drive, either USB or SSD with a USB version 3.0 or greater connection.

## Steps
1. Download the Neon OS to your computer from the link on this page: https://neon.ai/NeonAIforMycroftMarkII
2. Download a (free) imaging program, if you don't have one already. We suggest Raspberry Pi Imager or Balena Etcher.
3. Plug the drive you want to update into your computer.
4. Open your imaging program.
- If using Raspberry Pi Imager:
   - Under the 'Operating System' menu, select 'Use Custom'.
   - Locate and select the Neon OS image you downloaded.
   - Under 'Storage', select the drive you intend to write to.
   - Click 'Write' and wait for the image to be written and verified. This takes several minutes, depending on your system.
- If using Balena Etcher
   - Under the '+', choose 'Flash from file'.
   - Locate and select the Neon OS image you downloaded.
   - Under 'Select target', select the drive you intend to write to.
   - Click 'Flash' and wait for the image to be written and verified. This takes several minutes, depending on your system.
6. Remove the USB or SSD drive from your computer.
7. Disconnect power from your Mark II.
8. Plug the newly imaged USB drive into the top left USB port on your Mark II (Port 0).
9. Reconnect power to your Mark II.
10. Set up as normal, and you're on the new version!

## Troubleshooting
- Imaging using a computer is not quite 100% reliable. If your newly imaged drive does not perform well, try imaging it again. 

## Tags
- imaging
- make a boot drive
- computer
