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

# Additional

At this state, we have done configure our vps to be safer from any malicious threat out there. Now we are gonna do an optional steps to monitor our vps that support any device.

- install glances
```bash
sudo apt update && sudo apt install -y glances
```

- install btop
```bash
sudo apt update && sudo apt install -y btop
```

- install nethogs
```bash
sudo apt update && sudo apt install -y nethogs
```

- install essential package bundle
```bash
sudo apt update && sudo apt install build-essential
```

- edit makefile
```bash
nano ~/Makefile
```

- add this scripts into makefile
```bash
.PHONY: default cpu mem net run-all
# Native Top Snapshot
default:
	@echo "\033[1;36m=== Native Top Snapshot (Summary) ===\033[0m"
	@top -b -n 1 | head -n 5
	@echo ""
# Top 5 CPU Processes
cpu:
	@echo "\033[1;32m=== Top 5 CPU Processes ===\033[0m"
	@printf "\033[1;33m%-10s %-10s %-10s %s\033[0m\n" "PID" "%CPU" "%MEM" "COMMAND"
	@ps aux --sort=-%cpu | head -n 6 | tail -n +2 | awk '{printf "%-10s %-10s %-10s ", $$2, $$3, $$4; $$1=$$2=$$3=$$4=$$5=$$6=$$7=$$8=$$9=$$10=""; print $$0}'
	@echo ""
# Top 5 Memory Processes
mem:
	@echo "\033[1;32m=== Top 5 Memory Processes ===\033[0m"
	@printf "\033[1;33m%-10s %-10s %-10s %s\033[0m\n" "PID" "%CPU" "%MEM" "COMMAND"
	@ps aux --sort=-%mem | head -n 6 | tail -n +2 | awk '{printf "%-10s %-10s %-10s ", $$2, $$3, $$4; $$1=$$2=$$3=$$4=$$5=$$6=$$7=$$8=$$9=$$10=""; print $$0}'
	@echo ""
# Top 5 Network Processes
net:
	@echo "\033[1;32m=== Top 5 Network Processes (Bandwidth) ===\033[0m"
	@if command -v nethogs >/dev/null 2>&1; then \
		sudo nethogs -c 5 -t | head -n 12; \
	else \
		echo "\033[0;31m[Notice] 'nethogs' is required to fetch network bandwidth.\033[0m"; \
		echo "Install it using: \033[1;37msudo apt install -y nethogs\033[0m"; \
	fi
	@echo ""
# Runs all of them in order
run-all: default cpu mem net
```

- now run this
```bash
make
# or
make cpu
```