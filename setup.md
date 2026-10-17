# Getting Started

To start this guide, you must have a vps with minimum spesification.

# Prepape vps

After your vps's operating system installed, we need to connect and update the system. Since in this setup we are using linux (ubuntu), use apt update to refresh the system and do upgrade with this command.

```
apt update -y && apt upgrade
```

# Configuration

Next, we have to configure this vps to make it more secure from any possible malicious threat. We will do this into a few steps below,

- Edit ssh config
```
nano /etc/ssh/sshd_config
```

