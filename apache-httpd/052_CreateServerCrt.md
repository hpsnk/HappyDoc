## 2. 生成服务器证书

1. 准备配置文件 `server.conf`
    ```conf
    [ req ]
    default_bits       = 2048
    distinguished_name = req_distinguished_name
    req_extensions     = req_ext

    [ req_distinguished_name ]
    countryName                 = Country Name (2 letter code)
    countryName_default         = CN
    stateOrProvinceName         = State or Province Name (full name)
    stateOrProvinceName_default = Shanghai
    localityName                = Locality Name (eg, city)
    localityName_default        = Shanghai
    organizationName            = Organization Name (eg, company)
    organizationName_default    = Hpsnk Company
    commonName                  = Common Name (e.g. server FQDN or YOUR name)
    commonName_default          = www.hpsnk.com

    [ req_ext ]
    subjectAltName = @alt_names

    [ alt_names ]
    DNS.1   = www.hpsnk.com
    DNS.2   = hpsnk.com
    DNS.3   = localhost
    IP      = 192.168.1.1
    ```

2. 生成服务端密钥 `server.key`
    ```cmd
    openssl genrsa -out server.key 4096
    ```

3. 生成服务端CSR `server.csr`
    ```
    openssl req -new -sha256 -out server.csr -key server.key -config server.conf
    ```

4. 生成证书 `server.crt`
    ```
    openssl x509 -req -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt -extensions req_ext -extfile server.conf
    ```
