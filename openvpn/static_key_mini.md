# Static key mini

This setup is the minimal setup for OpenVPN

## Basic structure

Create VPN tunnel at server endpoint `10.8.0.1`, client endpoint `10.8.0.2`. Encrypted communication occurs over UDP port `1194`.

## 1. Keygen

Generate static key
```bash
openvpn --genkey --secret static.key
```

## 2. Copy key to client and server (via secure channel)

## 3. Config server and client

Server:

```
dev tun
ifconfig 10.8.0.1 10.8.0.2
secret static.key
```

Client:
```
remote <remote domain>
dev tun
ifconfig 10.8.0.2 10.8.0.1
secret static.key
```

## 4. Firewall rule

- UDP port `1194` is open on server