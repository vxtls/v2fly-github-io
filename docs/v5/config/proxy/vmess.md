# VMess

[VMess](../../../developer/protocols/vmess.md) 是一个加密传输协议，它分为入站和出站两部分，通常作为 V2Ray 客户端和服务器之间的桥梁。

VMess 依赖于系统时间，请确保使用 V2Ray 的系统 UTC 时间误差在 90 秒之内，时区无关。在 Linux 系统中可以安装`ntp`服务来自动同步系统时间。

## VMess 入站

inbound.vmess

> `users` : [string]

一组服务器认可用户的 ID，必须为合法的 UUID。

## VMess 出站

outbound.vmess

> `address`: string

服务器地址，支持 IP 地址或者域名。

> `port`: number

服务器端口号。

> `uuid`: string

服务器认可的 VLESS 用户 ID，必须为合法的 UUID。可以使用Linux命令 `uuidgen`或`./v2ray uuid`,或使用在线UUID生成器：https://www.v2fly.org/awesome/tools.html#%E7%AC%AC%E4%B8%89%E6%96%B9%E5%9B%BE%E5%BD%A2%E5%AE%A2%E6%88%B7%E7%AB%AF

VMess+ws+TLS的最佳实践示例
```json
{
  "log": {
    "loglevel": "info"
  },
  "inbounds": [
    {
      "port": 443,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "6e35d026-dfda-4bcc-a5f5-07cd22fc0c8f",
            "alterId": 0,
            "security": "auto"
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/guojixinwen"
        },
        "security": "tls", //如果不需要tls，请将其改为`none`
        "tlsSettings": {
          "certificates": [
            {
              "certificateFile": "/root/fullchain.pem", //使用自签名证书或使用CA签发的证书
              "keyFile": "/root/privkey.pem"
            }
          ]
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```
Vmess+httpupgrade+TLS相较于ws，提高了一些性能
```json
{
    "log": {
        "error": {
            "level": "info"
        },
        "access": {
            "type": "None"
        }
    },
    "inbounds": [
        {
            "protocol": "vmess",
            "settings": {
                "users": [
                    "35555e67-fb2f-4fba-9b06-a633937a2263"
                ]
            },
            "port": 80,
            "streamSettings": {
                "transport": "httpupgrade",
                "transportSettings": {
                    "path": "/download",
                    "host": "hk1.skymansion.cn"
                },
        "security": "tls", //如果不需要tls，请将其改为`none`
        "tlsSettings": {
          "certificates": [
            {
              "certificateFile": "/root/fullchain.pem", //使用自签名证书或使用CA签发的证书
              "keyFile": "/root/privkey.pem"
            }
          ]
        }
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
```
