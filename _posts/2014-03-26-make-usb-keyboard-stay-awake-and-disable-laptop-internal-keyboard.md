---
layout: post
title: Make USB keyboard stay awake and disable laptop internal keyboard
date: 26/03/2014
categories: arch linux udev keyboard
---

I'm on Arch linux, on a laptop, and I just got a nice [Poker II keyboard](http://geekhack.org/index.php?topic=44118.0), which is a monumental joy to type on.  When I first plugged in the keyboard, everthing worked great, but then I quickly ran into an issue with it falling asleep after two seconds.  This would be fine, except it took a whole keypress to wake it up, and that keypress didn't register with the OS.  I'd type a word, and it would inevitably end up missing its first letter.  Annoying.  I quickly tracked it down to the keyboard falling asleep due to laptop USB power saving.  Now to google.  Here's what I found to fix the issue:

# Find the vendor and product ids for your device

The keyboard luckily output some messages I could easily see with `dmesg`:

    [ 2327.000711] input: Heng Yu Technology Poker II as /devices/pci0000:00/0000:00:14.0/usb1/1-2/1-2:1.1/input/input29
    [ 2327.000899] hid-generic 0003:0F39:0611.000D: input,hidraw2: USB HID v1.10 Device [Heng Yu Technology Poker II] on usb-0000:00:14.0-2/input1

In the second line, the id is `0F39:0611`.  That is the `idVendor` and `idProduct` separated by a colon, respectively.

# Change udev rules to keep the power on

I added the following line to `/etc/udev/rules.d/91-local.rules` (create the file if it doesn't exist):

    ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="0f39", ATTR{idProduct}=="0611", TEST=="power/control", ATTR{power/control}="on"

# Change laptop-mode config

If you have `laptop-mode-tools` installed, it'll interfere with the above udev rule.  Modify `/etc/laptop-mode/conf.d/usb-autosuspend.conf` to blacklist your device, change the `AUTOSUSPEND_USBID_BLACKLIST` value as you can see in the file below (about halfway through the file).

    # The list of USB IDs that should not use autosuspend. Use lsusb to find out the
    # IDs of your USB devices.
    # Example: AUTOSUSPEND_USBID_BLACKLIST="046d:c025 0123:abcd"
    AUTOSUSPEND_USBID_BLACKLIST="0f39:0611"

# Disable the internal, default keyboard

I wanted to put the poker on top of my laptop and use it in the same position as the laptop's internal keyboard, but it kinda crushes the keys (the Poker II is not super lightweight).  I had to disable the internal keyboard.  To do that, first I needed to find the keyboard's id using `xinput`:

    ƛ xinput
    ⎡ Virtual core pointer                          id=2  [master pointer  (3)]
    ⎜   ↳ Virtual core XTEST pointer                id=4  [slave  pointer  (2)]
    ⎜   ↳ SynPS/2 Synaptics TouchPad                id=11 [slave  pointer  (2)]
    ⎜   ↳ ELAN Touchscreen                          id=13 [slave  pointer  (2)]
    ⎣ Virtual core keyboard                         id=3  [master keyboard (2)]
        ↳ Virtual core XTEST keyboard               id=5  [slave  keyboard (3)]
        ↳ Power Button                              id=6  [slave  keyboard (3)]
        ↳ Video Bus                                 id=7  [slave  keyboard (3)]
        ↳ Power Button                              id=8  [slave  keyboard (3)]
        ↳ Sleep Button                              id=9  [slave  keyboard (3)]
        ↳ AT Translated Set 2 keyboard              id=10 [slave  keyboard (3)]
        ↳ Acer WMI hotkeys                          id=12 [slave  keyboard (3)]
        ↳ HD WebCam                                 id=15 [slave  keyboard (3)]
        ↳ Heng Yu Technology Poker II               id=14 [slave  keyboard (3)]
        ↳ Heng Yu Technology Poker II               id=16 [slave  keyboard (3)]

There you can see my Poker II shows up twice for some reason, and my laptop's normal keyboard is labeled "AT Translated Set 2 keyboard".  I discovered the hard way that those ids can change, but luckily the `xinput` command takes names as well, so we'll use that string to make a bash script that toggles the keyboard on/off:

    #!/bin/bash
    #/home/me/bin/toggle-keyboard

    kb_name='AT Translated Set 2 keyboard'

    if xinput list "$kb_name" | grep -i --quiet disable; then
      xinput enable "$kb_name"
    else
      xinput disable "$kb_name"
    fi

Now everything's peachy!.. Almost.  One further level of convenience would be to enable/disable the internal keyboard whenever I un/plug my external one.  `udev` to the rescue again!  `udev` can run a script when those "add" and "remove" events occur.  I modified the `udev` rule to the following:

    ACTION=="add",    ENV{DISPLAY}=":0.0", ENV{XAUTHORITY}=="/home/me/.Xauthority", SUBSYSTEM=="usb", ENV{ID_VENDOR_ID}=="0f39", ENV{ID_MODEL_ID}=="0611", RUN+="/home/me/bin/disable-keyboard", TEST=="power/control", ATTR{power/control}="on"
    ACTION=="remove", ENV{DISPLAY}=":0.0", ENV{XAUTHORITY}=="/home/me/.Xauthority", SUBSYSTEM=="usb", ENV{ID_VENDOR_ID}=="0f39", ENV{ID_MODEL_ID}=="0611", RUN+="/home/me/bin/enable-keyboard"

Notice a couple of things: `RUN` and `ENV{WHATEVER}`.  I switched the `ATTR` parts to `ENV` because apparently `ATTR` isn't always available, but `ENV` is.  If you want to see what `ENV` variables are available, run `udevadm monitor --property` and then (un)plug your device.  The `RUN` part is pretty self-explanatory, and here are the scripts:

    #!/bin/bash
    xinput enable 'AT Translated Set 2 keyboard'

And to disable:

    #!/bin/bash
    xinput disable 'AT Translated Set 2 keyboard'

The trickiest bit was figuring out why this didn't work at first, but it turns out that even though `udev` will run a script just fine, in order to get `xinput` to work, I had to add the `ENV{DISPLAY}=":0.0"` and `ENV{XAUTHORITY}=="/home/me/.Xauthority"` parts to the udev rule.  Took me a while to figure that one out.

Now everything is fully automatic, I don't really need my toggle script.  I could combine both of those xinput scripts into a single toggle script, or a script that takes an argument, but I liked the uber-explicitness of having two separate scripts.  I may change my mind about that later.

I hope this is helpful to others!  Happy keyboarding!

EDIT:

I made it better:

    ACTION=="add",    ENV{XAUTHORITY}="/home/me/.Xauthority", ENV{NAME}=="*?", ENV{DISPLAY}=":0.0", ENV{ID_INPUT_KEYBOARD}=="1", RUN+="/home/me/bin/disable-keyboard", TEST=="power/control", ATTR{power/control}="on"
    ACTION=="remove", ENV{XAUTHORITY}="/home/me/.Xauthority", ENV{NAME}=="*?", ENV{DISPLAY}=":0.0", ENV{ID_INPUT_KEYBOARD}=="1", RUN+="/home/me/bin/enable-keyboard"

This should detect any keyboard added to the system.  I arrived at this by examining the output from the `udevadm monitor --property` command more closely, and choosing an appropriate `UDEV` event for this rule to apply to.  I found out that keyboards get both a `ID_INPUT_KEYBOARD` and a `NAME` property, and it seems to work pretty well, while being totally generic to all keyboards!  Just don't forget to add your keyboard to the `AUTOSUSPEND_USBID_BLACKLIST`.  ; )
