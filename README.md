# shadowsocks-v2ray-docker

## docker install

If you need to install docker by yourself, follow the [official installation guide][1].

## Pull the image

```bash
$ docker pull potzedd/shadowsocks-v2ray
```

It can be found at [Docker Hub][2].

## configuration file with docker volumes

You **must create a configuration file**  `/opt/shadowsocks-libev/config.json` in host at first:

And **create a cert file** with [acme.sh][3] in /opt/ssl (same file used in nginx）


A sample in JSON like below:

```
{
    "server":"0.0.0.0",
    "server_port":8984,
    "local_port": 1080,
    "password":"password",
    "timeout":600,
    "method":"chacha20-ietf-poly1305",
    "no_delay": true,
    "fast_open":true,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
}
```

If you want to enable **v2ray-plugin with http**, a sample in JSON like below:

```
{
    "server":"0.0.0.0",
    "server_port":8984,
    "local_port": 1080,
    "password":"password",
    "timeout":600,
    "method":"chacha20-ietf-poly1305",
    "no_delay": true,
    "fast_open":true,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
    "plugin":"v2ray-plugin",
    "plugin_opts":"server"
}
```

If you want to enable **v2ray-plugin with tls**, a sample in JSON like below:

```
{
    "server":"0.0.0.0",
    "server_port":8984,
    "local_port": 1080,
    "password":"password",
    "timeout":600,
    "method":"chacha20-ietf-poly1305",
    "no_delay": true,
    "fast_open":true,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
    "plugin":"v2ray-plugin",
    "plugin_opts":"server;tls;cert=/etc/ssl/fullchain.cer;key=/etc/ssl/private.key;path=/path;fast-open;loglevel=none"
}
```
**#"path" in "plugin_opts" is your path in nginx conf file "location" also same as path in yuor sahdowsocks cli**

If you want to enable **xray-plugin with http**, a sample in JSON like below:

```
{
    "server":"0.0.0.0",
    "server_port":8984,
    "local_port": 1080,
    "password":"password",
    "timeout":600,
    "method":"chacha20-ietf-poly1305",
    "no_delay": true,
    "fast_open":true,
    "nameserver":"8.8.8.8",
    "mode":"tcp_and_udp"
    "plugin":"xray-plugin",
    "plugin_opts":"server"
}
```

For more `v2ray-plugin` configrations please visit v2ray-plugin [usage][4].

For more `xray-plugin` configrations please visit xray-plugin [usage][5].

This container with sample configuration `/etc/shadowsocks-libev/config.json`

## Start a container
There is an example to start a container that listens on default `8984` (both TCP and UDP):

```bash
$ docker run -d -p 8984:8984 -p 8984:8984/udp --name ss-libev --restart=always -v /opt/shadowsocks-libev:/etc/shadowsocks-libev -v /opt/ssl:/etc/ssl potzedd/shadowsocks-v2ray
```

**Warning**: The port number must be same as configuration and opened in firewall.


[1]: https://docs.docker.com/install/
[2]: https://hub.docker.com/r/potzedd/shadowsocks-v2ray/
[3]: https://github.com/acmesh-official/acme.sh/wiki
[4]: https://github.com/shadowsocks/v2ray-plugin#usage
[5]: https://github.com/teddysun/xray-plugin#usage
