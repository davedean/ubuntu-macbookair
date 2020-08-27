# ubuntu-macbookair
Notes about running Ubuntu on a Macbook Air 6,2 (2013)

This time around I'm running Kubuntu 20.04 but this is probably transferrable to regular Ubuntu (Focal Fossa).

## Not working out of the box:

### Wireless

Get online somehow (I used a usb network adapter) and run an `apt update && apt-upgrade`

You should now have wireless.

### suspend wakes up immediately

Whilst trying to fix this I installed:

tlp
powertune
powertop

[https://askubuntu.com/questions/1073162/macbook-running-ubuntu-18-04-wont-go-to-sleep]


## Other stuff I installed

Microsoft fonts: 
`sudo apt install ttf-mscorefonts-installer`

