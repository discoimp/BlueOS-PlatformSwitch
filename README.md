Disclamer: these are my own notes as I embark on installing BlueOS on Debian 10 Buster on a Raspberry Pi 3b in order to effortlessly be able to add Ros services in standalone containers.

### SD-Card preparing
Insert your SD-card in your laptop/SD-Card reader.
Install Raspberry pi's image burner:
```
sudo snap install rpi-imager
```
run it and choose Raspberry Pi OS (other) and select Raspberry Pi OS Lite (Legacy) (Buster)

### headless connect
If you are installing it headless, and without access to the dhcp service, boot the rasbian and on a computer on the same network run:
```
sudo apt install nmap
hostname -I
```
Now copy the first three numbers of the ip address and replace the # in this:
```
nmap #.#.#.1/24
```
This will knock on all the doors in your subnet. Your raspberry should be the one with 22/tcp open ssh and 999 closed ports.
```
ssh THE-USERNAME-YOU-CHOSE-WHEN-YOU-MADE-THE-IMAGE@THE.IPADDRESS.YOU.JUST.FOUND
```
type: yes and you should be good.

btw: If you get warnings like...
```
apt-listchanges: Can't set locale; make sure $LC_* and $LANG are correct!
```
...while using SSH, just disregard. If I understand correctly you are passing your own locale settings when using SSH, but I don't think this will be a problem for what we are doing.


Now update, reboot and reconnect your pi with the above ssh command
```
sudo apt update && sudo apt upgrade -y
sudo reboot
```

### Docker Setup
Follow instructions at [docs.docker.com](https://docs.docker.com/engine/install/debian/#install-using-the-convenience-script), or just run:
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Give the Docker user all skills (not secure), and apply the new policy
```
sudo usermod -aG docker $USER
newgrp docker
```

To test if Docker is set up correctly run this and if you see Hello from Docker!, all is good:
```
docker run hello-world
```
### BlueOS install

Now we can run the install script for [BlueOS](https://github.com/bluerobotics/BlueOS-docker/tree/master/install)
```
sudo su -c 'curl -fsSL https://raw.githubusercontent.com/bluerobotics/blueos-docker/master/install/install.sh | bash'
```
The Pi should automatically restart and you can access it as BlueOS at 192.168.2.2.

### Backup your SD-card
This would be a good time to backup your working SD-Card.
Insert your card in your machine and locate the name of it:
```
dd if=/sudo fdisk -l
```
Replace DISKNAME with the relevant name and provide a valid path to where to save the file.
```
dd if=/dev/DISKNAME | gzip > /home/USERNAME/backup.img.gz
```
(If we only run the dd command your image will be the size of your card, unconvenient. The gzip will compress it)
To send it back to a memory card (not tested yet, but internet says it will work):
```
cat /home/USERNAME/backup.img.gz | gunzip | dd of=/dev/DISKNAME status=progress
```
