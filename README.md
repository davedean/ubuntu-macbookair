# ubuntu-macbookair

Notes about installing/running Ubuntu on a Macbook Air 6,2 (2013)

This time around I'm running Kubuntu 20.04 but this is probably transferrable to regular Ubuntu (Focal Fossa).

## Not working out of the box

### Wireless

Get online somehow (I used a usb network adapter) and run an `apt update && apt-upgrade`

You should now have wireless.

### power usage was excessive

Installing this seemed to do the trick:

`sudo apt install tlp`

Even without configuration that seemed to make a big difference.

I added a cust config because a web page suggested it would help.

```bash
$ cat /etc/tlp.d/02-dd.conf
DISK_IDLE_SECS_ON_AC=0
DISK_IDLE_SECS_ON_BAT=1
MAX_LOST_WORK_SECS_ON_BAT=15
DISK_APM_LEVEL_ON_BAT="1 1"
RUNTIME_PM_ALL=1
```

Restart tlp if you want that to take effect without a reboot.

### suspend wakes up immediately

In the end I think the only thing needed is this custom service, which I borrowed from the link below. The only impact of this is that the power button is the only way to wake from suspect.

Occasionally the Macboook Air may wake up immediately after suspend. To fix this:

`cat /proc/acpi/wakeup`

Check to see that XHC1 and LID0 are enabled. If they are, disabling them will fix the problem. After disabling them, the only way to wake up your computer from suspend is by using the power button.

To disable these settings, create the following service that runs on startup:

`sudo vim /etc/systemd/system/suspend-fix.service`

Then add the following text and save:

```
[Unit]
Description=Fix for the suspend issue
[Service]
Type=oneshot 
ExecStart=/bin/sh -c "echo XHC1 > /proc/acpi/wakeup && echo LID0 > /proc/acpi/wakeup"
[Install]
WantedBy=multi-user.target
```

And then run the following:

```bash
systemctl enable suspend-fix.service
systemctl start suspend-fix.service
```

Disabling only XHC1 is not recommended if you have this bug, since it may result in glitchy behavior.

From: https://askubuntu.com/questions/1073162/macbook-running-ubuntu-18-04-wont-go-to-sleep

### Color Profile

You can copy your icc color profile from OSX, in `/Library/ColorSync/Profiles/Displays` to your Ubuntu install. Your .icc file will be named something like `Color LCD-EC8836A8-9252C-B558-A128-05BC224C42B8.icc`

To get KDE to load it, install `colord-kde`:

`sudo apt install colord-kde`

Then you can load the 'Color Corrections' panel in 'System Settings', select your monitor, and use the 'Add Profile' button to load your .icc file.

## Other stuff I installed

Microsoft fonts: 
`sudo apt install ttf-mscorefonts-installer`

Fan Control:
`sudo apt install macfanctld`

## Links which helped

https://help.ubuntu.com/community/MacBookAir6-2/Trusty

https://askubuntu.com/questions/1073162/macbook-running-ubuntu-18-04-wont-go-to-sleep

https://davidxie.net/install-ubuntu-on-macbook-air

https://int3ractive.com/2019/12/things-to-do-after-installing-ubuntu-mate.html
