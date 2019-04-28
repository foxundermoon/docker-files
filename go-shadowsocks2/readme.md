


# source code
[https://github.com/shadowsocks/go-shadowsocks2](https://github.com/shadowsocks/go-shadowsocks2)

# usage

## help
```bash
docker run -it  --rm  foxundermoon/go-shadowsocks2
Usage of /shadowsocks2:
  -c string
        client connect address or url
  -cipher string
        available ciphers: AEAD_AES_128_GCM AEAD_AES_192_GCM AEAD_AES_256_GCM AEAD_CHACHA20_POLY1305 AES-128-CFB AES-128-CTR AES-192-CFB AES-192-CTR AES-256-CFB AES-256-CTR CHACHA20-IETF XCHACHA20 (default "AEAD_CHACHA20_POLY1305")
  -key string
        base64url-encoded key (derive from password if empty)
  -keygen int
        generate a base64url-encoded random key of given length in byte
  -password string
        password
  -redir string
        (client-only) redirect TCP from this address
  -redir6 string
        (client-only) redirect TCP IPv6 from this address
  -s string
        server listen address or url
  -socks string
        (client-only) SOCKS listen address
  -tcptun string
        (client-only) TCP tunnel (laddr1=raddr1,laddr2=raddr2,...)
  -u    (client-only) Enable UDP support for SOCKS
  -udptimeout duration
        UDP tunnel timeout (default 5m0s)
  -udptun string
        (client-only) UDP tunnel (laddr1=raddr1,laddr2=raddr2,...)
  -verbose
        verbose mode
```

---

# Basic Usage

## Server
Start a server listening on port 8488 using 

```bash
 docker run -d --name go-shadowsocks2  foxundermoon/go-shadowsocks2 -s  -p 8488:8488 'ss://AEAD_CHACHA20_POLY1305:your-password@:8488' -verbose
```

AEAD_CHACHA20_POLY1305 AEAD cipher with password your-password.


## Client
Start a client connecting to the above server. The client listens on port 1080 for incoming SOCKS5 connections, and tunnels both UDP and TCP on port 8053 and port 8054 to 8.8.8.8:53 and 8.8.4.4:53 respectively.

```bash
docker run -d --name go-shadowsocks2-server  foxundermoon/go-shadowsocks2 -s  -p 1080:1080  foxundermoon/go-shadowsocks2 -c 'ss://AEAD_CHACHA20_POLY1305:your-password@[server_address]:8488' -verbose -socks :1080 -u -udptun :8053=8.8.8.8:53,:8054=8.8.4.4:53 -tcptun :8053=8.8.8.8:53,:8054=8.8.4.4:53
```

Replace [server_address] with the server's public address.

Advanced Usage
Netfilter TCP redirect (Linux only)
The client offers -redir and -redir6 (for IPv6) options to handle TCP connections redirected by Netfilter on Linux. The feature works similar to ss-redir from shadowsocks-libev.

Start a client listening on port 1082 for redirected TCP connections and port 1083 for redirected TCP IPv6 connections.
```
docker run -d --name go-shadowsocks2-client -p 1082:1082 -p 1083:1083 foxundermoon/go-shadowsocks2 -c 'ss://AEAD_CHACHA20_POLY1305:your-password@[server_address]:8488' -redir :1082 -redir6 :1083
```