# Neon OS on the Mark II
This section is for content specific to the Neon AI private personal assistant on the Mycroft Mark II Device.

## Making an Additional Boot Drive
Summary: 
To create an additional bootable drive using your Mark II, plug a second USB or 
an SSD into the right hand blue port, and say "Hey Neon, make bootable media" 
then follow the prompts. When finished, shut down your device, move the new drive 
into the first port and plug it back in. 

### Detailed Instructions 
**Supplies needed:**

1. An SSD (or USB) drive with a minimum of 8GB and USB 3.0 connection. *Any content on the drive will be deleted during this process.*

**Preparation:**

1. Make sure your Neon OS is up to date. We reccomend updating to the current release, rather than a beta version. *Say, "Enable stable updates" and "Check for updates"*
2. Have your Mark II running.
3. Plan for this to take about an hour. It may be much faster.
4. When booting using your new drive, you will need to reestablish your internet connection. Other personal information you have taught Neon such as your name will be retained on your original boot drive, but will not be on your new one.

**Process:**

1. Plug the SSD or USB drive that you want to make into your new boot drive into the upper (blue) USB port to the right of your existing USB drive.
2. Tell Neon to "Make Bootable Media". *It will ask you to confirm. Then it will download the new OS image file. How long this takes depends on your internet speed and the speed of your current boot drive. It may take as little as 10 minutes, or up to an hour.*
3. When the download is finished, an alert icon will appear on the upper left of the screen and Neon will ask you to confirm that you want it to write to your new SSD drive.  *At this time Neon is not providing this verbal alert, we'll have that fixed soon.* 
Select the alert by touching the alert icon, and then the words of the alert itself. Neon should give you a verbal prompt to complete the process, warning you it will erase anything on that drive. Confirm you want it to do that. *The time it takes to write the image to the new drive varies based on the drive speeds of both drives. 10 minutes is a reasonable estimate.*
5. When it's finished writing the image, it will tell you, and an alert will appear again at the top left of the screen. When you confirm verbally or select the alert, Neon will shut down automatically. *At this time Neon is not providing this verbal alert, we'll have that fixed soon.* 
6. Shut down your device as normal if for any reason it did not shut down on its own.
7. Unplug the power from your Mark II.
8. Unplug your original drive, and move the new drive over into the upper left USB port.
9. Plug power back in and Neon will boot up from the new drive. 

> If you do not have a functioning Mark 2 device, see the 
  [Installation Instructions](./neon_os_installation.md#installing-a-downloaded-image-to-a-usb-drive).



### Troubleshooting and Tech Support

Here is our troubleshooting walk-through, and I am also available for a personal tech support video call if you prefer more in-person assistance. 

**The first things to check are the power and your USB boot drive:**

**Start by unplugging your Mark II.**

1. The boot drive (either a USB memory key or SSD) should be securely plugged into the top left USB port, which is port 0. 
2. There should be no other USB drives plugged in for the first boot.
3. You must leave the boot drive plugged in while the Mark II is powered up, including during the entire first boot process, which should take about 5 minutes. 
4. Securely plug in the Mark II power adapter to your power source, and within 10 seconds you should see a the cursor prompt. That will be followed by text, then a splash screen with the Neon name and logo in orange, and then a loading screen with circling lines & the Neon logo overlayed on OVOS' logo, and lastly a Neon AI screen in white & orange. *These splash screens may change in future versions.* After the splash screens, you should see a set of three green on green buttons with choices for setup. 



**Before you start general troubleshooting, here are a few common issues that we've seen, and the solutions to them:**

- If you unplug your Neon OS USB boot drive (your USB key or USB SSD) during the first boot, the drive can become corrupted, and you'll need to re-image the USB boot drive - see below. The potential for corrupting USB drives by unplugging or powering down while they are being accessed isn't something we can fix, since it's inherent in the hardware, but we're in the process of putting together a backup boot system on the boot drive in case this occurs.

- If you see hesitations in the execution of commands on your first try, Neon is building its “cache” for the skills you use, so the second iteration will be smooth. 

  

**If you haven't identified the issue yet, then it's time to establish answers to two key questions:**

1. Is this a hardware issue or a software issue?
2. Is the issue with the Mark II device or the boot drive?

**The following steps can help answer those questions:**

1. If your Mark II screen is displaying (typically the time) choose Restart from the swipe down menu.

2. Alternatively, choose Shutdown from the swipe down menu. Then unplug power, wait 20 seconds & plug back in. Your device will restart automatically. 

3. Plug in a different USB boot drive (a Mycroft Dinkum OS, OVOS, or another Neon OS drive.)

4. If you have a second Mark II, plug your boot drive into it & see if it will boot. 

5. Plug your boot drive into a computer. If it is corrupted, you may receive a message saying that it has errors on it. The solution in that case is to re-image the drive. If that imaging fails, or it becomes corrupted a second time that would indicate a USB drive hardware issue, and the drive needs to be replaced. 

6. Re-image your boot drive - see below.

   

**Documenting and sharing issues with Tech Support:**

- The Support Helper skill provides a direct route to support troubleshooting of more complex issues. If Neon is basically listening and functioning please say "create a support ticket" or "create a troubleshooting package". If you tell Neon your email address first, it can be sent to you. Otherwise it can go directly to us. If you want to submit the report from your email, please forward it to clary@neon.ai and please include your own notes on the technical support issue. 

- Some users find taking a short video is an easier way to share that information with us, instead of typing it up. 

  

#### How to image or re-image a drive
See the [Installation Instructions](./neon_os_installation.md#installing-a-downloaded-image-to-a-usb-drive)
for instructions on how to create a Neon OS drive.

> After starting up, you will be guided through connecting to WiFi.



It's important to us that you have a functioning Neon OS to use. **If this troubleshooting hasn't resolved your issue, please email me at clary@neon.ai of contact us on any of the forums for a next step.** 

Thank you for working with Neon OS, and improving the experience for all users. 
