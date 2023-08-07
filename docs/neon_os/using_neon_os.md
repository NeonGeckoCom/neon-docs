# Using Neon OS
Some tips for using Neon OS.

## First Run
The first time you boot Neon OS, there are some one-time processes that need to
run. Please be patient as the first boot will need to resize partitions and 
perform some other initialization. At times, it may appear the device isn't doing
anything, but please don't disconnect power as this can break the installation
on the USB drive. This process may take up to 10 minutes, though it is generally
faster.

When presented an option to continue setup, follow the prompts to connect to a
network.

After network setup is complete, there will be some skills loading in the background;
when those are loaded, the spinner in the top right corner of the screen will 
animate and the homescreen will be displayed. At this point, Neon is set up and
ready to go!

## Resetting
To reset Neon to original settings, there is a menu option under `Settings` ->
`Factory Settings` -> `Factory Reset`. This removes any user settings, restores a
previous version of Neon (and any other Python dependencies), and then restarts.
This can be useful to resolve potential configuration errors or Python errors if
installing an update or new package is causing problems.

For releases after July 2023, Neon OS provides a method for resetting an installation
to a clean state, even if Neon will not start properly. This means all configuration,
data, network information, and user-installed software will be removed. To perform a
reset, either plug in a powered off device or restart. When the LED ring lights up
(green if powering on, red if powering off), press and hold the `Volume -` and `Action`
buttons on top of the device until the LEDs light up white. After the LEDs illuminate,
release the buttons and Neon will restart, reset, and then boot as if it is the
first time it was powered on.