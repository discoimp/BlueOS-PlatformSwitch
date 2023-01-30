### SD-Card preparing
Insert your SD-card in your laptop/SD-Card reader.
Install Raspberry pi's image burner:
```
sudo snap install rpi-imager
```
run it and choose "Other general purpose  OS"
[Follow this guide incl. advanced](https://ubuntu.com/tutorials/how-to-install-ubuntu-on-your-raspberry-pi#2-prepare-the-sd-card)


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
We need to install some Docker tools (I guess)
[Install Docker:](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
