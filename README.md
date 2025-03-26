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
sudo openssl req -x509 -newkey rsa:4096 -nodes \
-keyout /usr/local/etc/v2ray/v2ray-key.pem \
-out /usr/local/etc/v2ray/v2ray-cert.pem \
-days 365 \
-subj "/CN=example.com"
```

### 3. Set File Permissions:
Adjust the permissions of the certificate and key files:

```bash
sudo chown nobody:nogroup /usr/local/etc/v2ray/v2ray-cert.pem
sudo chown nobody:nogroup /usr/local/etc/v2ray/v2ray-key.pem
sudo chmod 600 /usr/local/etc/v2ray/v2ray-key.pem
sudo chmod 644 /usr/local/etc/v2ray/v2ray-cert.pem
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
            "id": "YOUR_UUID_HEREf",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "tcp",
        "security": "tls",
        "tlsSettings": {
          "serverName": "example.com",
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

### 5. Enable and Start V2Ray:
Enable and start V2Ray using with the following commands:

```bash
systemctl enable v2ray;
systemctl start v2ray;
```
