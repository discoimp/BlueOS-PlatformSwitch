Disclamer: these are my own notes as I embark on installing BlueOS on Ubuntu server 20.04 on a Raspberry Pi 3b in order to effortlessly be able to add Ros services in standalone containers and it may just work on the raspbianOS bundled with the BlueOS image. Just testing

### SD-Card preparing
Insert your SD-card in your laptop/SD-Card reader.
Install Raspberry pi's image burner:
```
sudo snap install rpi-imager
```
run it and choose "Other general purpose  OS" For now it seems we should run a 32-bit version.
[Follow this guide incl. advanced](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#2-prepare-the-sd-card)
[EDIT] I got the BlueOS docker running after several low level adjustments. The wifi management module in BlueOS will not cooperate. Thinking: What else isn't working? - Trying Rasbian Buster, the Debian 10 that works with ROS. Maybe that a better choice.

If the raspberry doesn't connect to your wifi (and you have it hooked up to a monitor+keyboard.) try this after doublechecking wifi credentials:
```
sudo nano /etc/netplan/50-cloud-init.yaml
```
In here the number of spaces are holy, so don't use tab or add random spaces. Make sure the wifi name and password has quotes "" around them.

It should look like this:
```
wifis:
  wlan0:
    dhcp4: true
    optional: true
    access-points:
      "NAME OF YOUR NETWORK":
        password: "YOUR PASSWORD FOR EVERYONE TO SEE"
```

press
ctrl s
ctrl x
then run: 
```
sudo netplan apply && sudo reboot
```
else: ask ChatGPT to help you. 

### Docker Setup
We need to install some of the tools here:
[Install Docker:](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
Make sure you completed these [Linux post installation steps ](https://docs.docker.com/engine/install/linux-postinstall/)

### BlueOS install
Before we can run the installation file from Blue Robotics we need to fix some issues.
First run this line to create a symlink to where BlueOS expects these files to be:
```
sudo ln -s /boot/firmware/cmdline.txt /boot/ && sudo ln -s /boot/firmware/config.txt /boot/
```
Then we need to add some missing apps:
```
sudo apt install rfkill dhcpcd5 -y
```
Finally run the install script for [BlueOS](https://github.com/bluerobotics/BlueOS-docker/tree/master/install)
```
sudo su -c 'curl -fsSL https://raw.githubusercontent.com/bluerobotics/blueos-docker/master/install/install.sh | bash'
```
Well, that didn't work
I have to manually create the service that starts the docker This: [rc.local](https://marsown.com/wordpress/how-to-enable-etc-rc-local-with-systemd-on-ubuntu-20-04/)
