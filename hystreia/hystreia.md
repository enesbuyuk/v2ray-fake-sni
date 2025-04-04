# hystreia

server installation:
bash <(curl -fsSL https://get.hy2.sh/)


+ Edit server config file at /etc/hysteria/config.yaml
        + Start your hysteria server with systemctl start hysteria-server.service
        + Configure hysteria start on system boot with systemctl enable hysteria-server.service
/etc/hysteria/config.yaml


openssl.cnf
```
[req]
distinguished_name = req_distinguished_name
x509_extensions = v3_req
prompt = no

[req_distinguished_name]
CN = domain.net

[v3_req]
keyUsage = keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[alt_names]
DNS.1 = domain.net

```


openssl req -x509 -newkey rsa:2048 -nodes \
  -keyout key.pem \
  -out cert.pem \
  -days 365 \
  -config openssl.cnf



```
listen: :443

tls:
  cert: /etc/hysteria/selfsigned.crt
  key: /etc/hysteria/selfsigned.key
  sni: domain.net

auth:
  type: password
  password: your-pass

masquerade:
  type: proxy
  proxy:
    url: https://domain/
    rewriteHost: true

```
