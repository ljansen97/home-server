# Home Server

## IP Configuration
| IP Address | Description | Services running |
| --- | --- | --- |
| 192.168.1.254 | ASUS Router | |
| 192.168.1.253 | ProxMox | |
| 192.168.1.252 | PiHole | DNS, DHCP |
| 192.168.1.251 | Portainer | |

## Default username and passwords
Username: `lanfrey`
Password: `qwerty`

## DNS wildcard configuration
To configure the DNS wildcard configuration, you have to do the following steps.

1. Run `sudo pihole-FTL --config misc.etc_dnsmasq_d true`
2. Create a new file called `/etc/dnsmasq.d/01-server-local.conf` and add the following contents
```
address=/server.local/192.168.1.251
```

This will make sure that `*.server.local` always gets resolved to `192.168.1.251`.