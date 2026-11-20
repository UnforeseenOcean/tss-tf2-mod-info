# tss-tf2-mod-info
Compiled info for modifying t 0 ss f R 0n t 2 prior to 9.13 (Dead-end patch)

## Opening the device and entering programming mode
1. Take the bottom hatch off
2. Remove the rubber feet
3. Remove 5 visible screws
4. Insert a flat-head screwdriver or spudger into each rectangular hole where the tongues of the hatch cover go, and pull it outward while pulling the back cover up
5. Remove the 4 visible screws where the display panel is situated
6. Remove the grey cosmetic cover from the back of the device
7. Undo 2 screws
8. Insert guitar picks around the seam near the speaker hole and release the back cover carefully
9. Look at the connector with yellow tape and rainbow-colored wires on it
10. Look below the third and fourth pins of the connector
11. You will see `1V8` and `BOOT`; note the position
12. Insert a Micro USB cable (recommended: get a new one with data lines; do not use USB hubs or front USB ports) into the matching connector on the board
13. While shorting it with a pair of tweezers, **BEING SURE NOT TO TOUCH 12V PAD or nearby connector pins**, plug the USB-C cable in (If you short 1V8 or BOOT to DP or GND or 12V, you're screwed. Recommended action: Solder two thin wires to those pads and wire it up to a button newly installed in the empty spot below the hatch)
14. If successful, you will hear a sound on your computer that tells you new hardware is connected
15. If not, check the USB cable and tweezer position (again, if you do it wrong enough or if your hand slips, you might inject lethal voltage into the CPU power rail and kill it)

Note: On older devices 7.1 or earlier, press and hold the power button while plugging the USB-C cable in to enter programming mode (the device may appear to be not booting, but if it shows up as `Qualcomm HS-USB QDLoader 9008` or `QUSB_BULK_CID...` then you're good to go)

If you see an exclamation mark next to the device name, it means you need to use Zadig to install the WinUSB driver; go and do it

## Using programming mode
You will use tools made by B.Kerler available on GitHub

Note: Compatibility differs, and the required elf fire hose file differs from version to version
- If your version number ends with 9.13, you're screwed
- If your version number ends with 9.5 or 9.1, you may be in luck, but you will need to look around a lot more
- If your version number is less than 7.1, then you're good to go; one of Xiaomi's tablets with the same CPU will have this elf file
- You must source the loader yourself; it is not here and will not be linked here
1. `python edl.py printgpt --loader=firehose.elf` to test if you are in luck or not (If you see the partition table being printed, you are in luck and can continue)
2. `python edl.py r swraw swraw.img --loader=firehose.elf` to dump SWRAW data
3. Using a hex editor, add `SYSDEBUGMODE=true` at offset 0x300 (null-terminated; do not hit enter)
4. Now add `APPDEBUGMODE=true` at offset 0x400 (null-terminated)
5. Check again to see if you made any typos or wrong byte edits
6. Save as a new file
7. `python edl.py w swraw (new image name).img` to write the config (This is also the last chance to undo anything you messed up; if you made a mistake, redo it immediately)
8. `python edl.py reset` and it will reboot immediately
9. If you have done it correctly, you will see a transparent red bar at the top of the screen with the version number and SW number upon booting into the OS (If not: You're screwed)
10. Now you are ready to use ADB
11. **DO NOT CONNECT TO THE INTERNET**
12. Press the "Connect to network" button on the app
13. **DO NOT CONNECT TO THE INTERNET**
14. Swipe from the top of the screen to open the menu
15. **DO NOT CONNECT TO THE INTERNET**
16. Swipe again for the full menu
17. Tap on the cogwheel
18. Go to About Device
19. Spam the build number until the Developer menu opens
20. Enable USB Debugging
21. Check if you can see ADB devices
22. If not, allow USB debugging from the prompt that appears (use `adblink` for easier management)
23. BACK UP YOUR SYSTEM!!! If you screw it up, you will have a 0% chance of recovery without backups
24. `adb reboot fastboot` if you are lucky
25. `fastboot devices`
26. `fastboot -w`
27. Procure a LineageOS GSI ROM image (But not 20260105; it is known to boot loop)
28. `fastboot flash system system.img`
29. It will now flash the image. DO NOT MOVE OR TURN OFF THE DEVICE!
30. If it says `Not enough space` then `fastboot delete-logical-partition product_a` and `fastboot delete-logical-partition product_b`
31. If it STILL says so, `fastboot delete-logical-partition system_ext_a` and `fastboot delete-logical-partition system_ext_b`
32. `fastboot -w`
33. `fastboot reboot`
34. If all went well, you will boot into LineageOS instead of whatever was on the device

## Not lucky?
Wait for a new version of fire hose

If you typed `adb reboot fastboot` and it rudely said "Fastboot is disabled", well, you're out of luck and have to wait for the new version of fire hose again

## Screwed up and connected to the Internet?
First of all, how dare you, I told you not to do it, you are an idiot and I hate you

Second of all, if you see a red bar at the top of the screen anyway, you're in luck. Just fetch the full package list and uninstall anything that has the word tee-oh-ess-ess in it with `adb shell pm uninstall --user 0 (package name)` then install F-Droid to use apps

Note that you cannot use GApps on this version of Android; this means no Google Play Store and Play Services, and no apps that rely on this feature will work at all (i.e., Spotify, YT Music, YouTube, etc.)
