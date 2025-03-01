# V2Ray Fake SNI Document
Fake SNI in V2Ray

## Installation:

### 1. Install V2Ray:
Use the following command to install V2Ray:

```bash
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

### 2. Generate SSL Certificate for Fake SNI:
Create an SSL certificate with the following command:

```bash
sudo openssl req -x509 -newkey rsa:4096 -nodes -keyout /usr/local/etc/v2ray/v2ray-cert.pem -out /usr/local/etc/v2ray/v2ray-cert.pem -days 365 -subj "/CN=example-fake-sni.com"
```

### 3. Set File Permissions:
Adjust the permissions of the certificate and key files:

```bash
chmod 644 /usr/local/etc/v2ray/v2ray-cert.pem;
chmod 644 /usr/local/etc/v2ray/v2ray-key.pem;
```

### 4. Configure V2Ray:
Create a configuration file at `/usr/local/etc/v2ray/config.json` with the following content:

```json
{
  "inbounds": [
    {
      "port": YOUR_PORT,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "YOUR_UUID_HERE",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "tls",
        "tlsSettings": {
          "serverName": "example-fake-sni.com",
          "certificates": [
            {
              "certificateFile": "/usr/local/etc/v2ray/v2ray-cert.pem",
              "keyFile": "/usr/local/etc/v2ray/v2ray-key.pem"
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
