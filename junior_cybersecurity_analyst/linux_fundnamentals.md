# Linux Fundamentals

### File System Management

```bash
ls -il
sudo fdisk -l
cat /etc/fstab

mount
sudo mount /dev/sdb1 /mnt/usb
cd /mnt/usb && ls -l
sudo umount /mnt/usb
lsof | grep cry0l1t3
```

### Containerization

```bash
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Add user htb-student to the Docker group
sudo usermod -aG docker htb-student
echo '[!] You need to log out and log back in for the group changes to take effect.'

# Test Docker installation
docker run hello-world
```


```bash
FROM ubuntu:22.04

# Update the package repository and install the required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user called "docker-user"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Give the docker-user user full access to the Apache and SSH services
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose the required ports
EXPOSE 22 80

# Start the SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

```bash
docker build -t FS_docker .
docker run -p <host port>:<docker port> -d <docker container name>
docker run -p 8022:22 -p 8080:80 -d FS_docker

sudo apt install lxc -y
sudo lxc-create -n linuxcontainer -t ubuntu
sudo vim /usr/share/lxc/config/linuxcontainer.conf
sudo systemctl restart lxc.service

```

### Network Configuration

```bash
ifconfig
ip addr
sudo ifconfig eth0 up
sudo ip link set eth0 up
sudo ifconfig eth0 192.168.1.2
sudo ifconfig eth0 netmask 255.255.255.0
sudo route add default gw 192.168.1.1 eth0
sudo vim /etc/resolv.conf
sudo vim /etc/network/interfaces
sudo systemctl restart networking
ping 8.8.8.8
traceroute www.inlanefreight.com
netstat -a

```


### Remote Desktop Protocols in Linux

```bash
cat /etc/ssh/sshd_config | grep X11Forwarding
ssh -X htb-student@10.129.23.11 /usr/bin/firefox

sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
vncpasswd

touch ~/.vnc/xstartup ~/.vnc/config
cat <<EOT >> ~/.vnc/xstartup
#!/bin/bash
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/startxfce4
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &
EOT

cat <<EOT >> ~/.vnc/config
geometry=1920x1080
dpi=96
EOT

chmod +x ~/.vnc/xstartup
vncserver

vncserver -list
ssh -L 5901:127.0.0.1:5901 -N -f -l htb-student 10.129.14.130
xtightvncviewer localhost:5901

```

### Linux Security

```bash
apt update && apt dist-upgrade
cat /etc/hosts.allow
cat /etc/hosts.deny

sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```

### Solaris

```bash
uname -a
showrev -a
sudo apt-get install apache2
pkgadd -d SUNWapchr
chmod 700 filename
find / -perm 4000
share -F nfs -o rw /export/home
mount -F nfs 10.129.15.122:/nfs_share /mnt/local
cat /etc/dfs/dfstab
sudo lsof -c apache2
pfiles `pgrep httpd`
sudo strace -p `pgrep apache2`
truss ls
```

