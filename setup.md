# Getting Started

To start this guide, you must have a vps with minimum spesification.

# Prepape vps

After your vps's operating system installed, we need to connect and update the system. Since in this setup we are using linux (ubuntu), use apt update to refresh the system and do upgrade with this command.

```
apt update -y && apt upgrade
```

# Configuration

Next, we have to configure this vps to make it more secure from any possible  threat. We will do this into a few steps below,

- Edit ssh config and adjust default port to access this server
```bash
# open editor
nano /etc/ssh/sshd_config

# add this line (adjust port number as you want)
Port 22220

# restart sshd
systemctl restart sshd

# try access server with this
ssh root@ip_address -p 22220
```

- Create user login instead of root
```bash
# you need to fill password and some organization thing
# you could fill blank to any field of organization that you are not knowing
adduser frayoneon

# now log in again as root to set your user with root permission
usermod -aG sudo frayoneon

# now exit. and try to log in as user
ssh frayoneon@ip_address -p 22220
sudo apt update # this command should work
```

- Prevent default root login
```bash
nano /etc/ssh/sshd_config

# make sure this line adjusted like this
PermitRootLogin no

systemctl restart sshd
```

# Firewall

Now we have done setting up our basic configuration for the server. Next we are gonna double up the protection by enabling firewall. Here is what we do below,

```bash
ssh frayoneon@ip_address -p 22220
sudo su && cd # we need to be root to do this

# enable firewall
ufw status # at this state it should be inactive
# we need to allow default access to vps's port
ufw allow 22220/tcp
ufw enable
ufw status # now the state would be active
```

> these are useful command according to ufw
```bash
# check firewall status
ufw status

# reload firewall
ufw reload # now the state would be active
```