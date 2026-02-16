# Home Server

## IP Configuration
| IP Address | Description | Services running |
| --- | --- | --- |
| 192.168.1.254 | ASUS Router | Gateway to the internet |
| 192.168.1.253 | ProxMox | ProxMox |
| 192.168.1.252 | PiHole | DNS, DHCP |
| 192.168.1.251 | Docker | Frigate, nginx-proxy-manager |

## Web UI's
| Service | Link | Username | Password |
| --- | --- | --- | ---- |
| ProxMox | [https://proxmox.server.local](https://proxmox.server.local) | root | qwerty1234 |
| Pi-Hole | [https://pihole.server.local/admin](http://pihole.server.local/admin) | N/A | qwerty |
| nginx-proxy-manager | [https://npm.server.local](http://npm.server.local:81) | lanfrey.jansen@pm.me | qwerty1234 |

## Server logins
First run `cd C:/Users/LJ/.ssh/`.

| Server | IP | Command |
| --- | --- | --- |
| PiHole | 192.168.1.252 | `ssh -i ./pihole lanfrey@192.168.1.252` |
| Portainer | 192.168.1.251 | `ssh -i ./portainer portainer@192.168.1.251` |

## DNS wildcard configuration
To configure the DNS wildcard configuration, you have to do the following steps.

1. Run `sudo pihole-FTL --config misc.etc_dnsmasq_d true`
2. Create a new file called `/etc/dnsmasq.d/01-server-local.conf` and add the following contents
```
address=/server.local/192.168.1.251
```

This will make sure that `*.server.local` always gets resolved to `192.168.1.251`.

## DHCP Configuration
DHCP is done through Pi-Hole with the following configuration:

| Item | Configuration |
| --- | --- |
| Start | `192.168.1.50` |
| End | `192.168.1.200` |
| Gateway |  `192.168.1.254` |
| Netmask | `255.255.255.0` |

## Generating keys for a new server
### On Windows
1. Run `ssh-keygen -t rsa -b 4096 -C "server_name"`
2. Run `cat C:/Users/LJ/.ssh/server_name.pub`
3. Copy the key (with pre and postfix)

### On the server
1. Run `echo "<public key>" >> ~/.ssh/authorized_keys`
2. Run `chmod 600 ~/.ssh/authorized_keys`
2. Run `sudo nano /etc/ssh/sshd_config`
3. Uncomment the following lines:
- `PubkeyAuthentication yes`
- `AuthorizedKeyFile .ssh/authorized_keys`
3. Run `sudo systemctl restart ssh`

## Generating a self-signed certificate for nginx-proxy-manager
```mkdir -p /etc/ssl/selfsigned
cd /etc/ssl/selfsigned

# Generate private key
openssl genrsa -out npm-selfsigned.key 2048

# Generate self-signed certificate (valid for 365 days)
openssl req -x509 -nodes -days 365 -key npm-selfsigned.key -out npm-selfsigned.crt```