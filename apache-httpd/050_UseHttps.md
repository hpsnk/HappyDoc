
## 1. 生产ROOT CA证书

1. 准备配置文件 `ca.conf`
    ```conf
    [ ca ]
    default_ca    = CA_default

    [ CA_default ]
    dir              = C:\\CA\\root
    certs            = C:\\CA\\root\\certs
    crl_dir          = C:\\CA\\root\\crl
    database         = C:\\CA\\root\\index.txt
    new_certs_dir    = C:\\CA\\root\\newcerts
    serial           = C:\\CA\\root\\serial
    crlnumber        = C:\\CA\\root\\crlnumber
    crl              = C:\\CA\\root\\crl.pem
    certificate      = C:\\CA\\root\\certs\\ca.cert.pem
    private_key      = C:\\CA\\root\\private\\ca.key.pem
    name_opt         = ca_default
    cert_opt         = ca_default
    crl_extensions   = crl_ext
    default_days     = 365
    default_crl_days = 30
    default_md       = sha256
    preserve         = no
    policy           = policy_match

    [ policy_match ]
    countryName             = match
    stateOrProvinceName     = optional
    organizationName        = optional
    organizationalUnitName  = optional
    commonName              = supplied
    emailAddress            = optional

    [ policy_anything ]
    countryName             = optional
    stateOrProvinceName     = optional
    localityName            = optional
    organizationName        = optional
    organizationalUnitName  = optional
    commonName              = supplied
    emailAddress            = optional

    [ req ]
    default_bits            = 2048
    distinguished_name      = req_distinguished_name
    x509_extensions         = v3_ca
    string_mask             = utf8only
    default_md              = sha256

    [ req_distinguished_name ]
    countryName                     = Country Name (2 letter code)
    countryName_default             = CN
    countryName_min                 = 2
    countryName_max                 = 2
    stateOrProvinceName             = State or Province Name (full name)
    stateOrProvinceName_default     = Shanghai
    localityName                    = Locality Name (eg, city)
    localityName_default            = Shanghai
    0.organizationName              = Organization Name (eg, company)
    0.organizationName_default      = hpsnk Ins.
    organizationalUnitName          = Organizational Unit Name (eg, section)
    organizationalUnitName_default  = http://hpsnk.inc/
    commonName                      = Common Name (e.g. server FQDN or YOUR name)
    commonName_default              = hpsnk Internal Root CA
    commonName_max                  = 64
    emailAddress                    = admin@hpsnk.com
    emailAddress_default            = admin@hpsnk.com
    emailAddress_max                = 64

    [ server_cert ]
    basicConstraints       = CA:FALSE
    nsCertType             = server
    nsComment              = "OpenSSL Generated Server Certificate"
    subjectKeyIdentifier   = hash
    authorityKeyIdentifier = keyid,issuer:always
    keyUsage               = critical, digitalSignature, keyEncipherment
    extendedKeyUsage       = serverAuth

    [ v3_ca ]
    subjectKeyIdentifier   = hash
    authorityKeyIdentifier = keyid:always,issuer
    basicConstraints       = critical,CA:true
    keyUsage               = critical, digitalSignature, cRLSign, keyCertSign

    [ v3_intermediate_ca ]
    subjectKeyIdentifier   = hash
    authorityKeyIdentifier = keyid:always,issuer
    basicConstraints       = critical,CA:true, pathlen:0
    keyUsage               = critical, digitalSignature, cRLSign, keyCertSign

    [ crl_ext ]
    authorityKeyIdentifier = keyid:always
    ```

2. 生成ca密钥(ca.key)
    ```
    C:\Apache24\bin\openssl.exe genrsa -out ca.key 4096
    ```

3. 生成ca证书签发请求(ca.csr)
    ```
    C:\Apache24\bin\openssl.exe req -new -sha256 -out ca.csr -key ca.key -config ca.conf
    ```
4. 生成ca根证书(ca.crt)
    ```
    C:\Apache24\bin\openssl.exe x509 -req -days 3650 -in ca.csr -signkey ca.key -out ca.crt
    ```

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
    ```

2. 生成服务端密钥 `server.key`
    ```cmd
    C:\Apache24\bin\openssl.exe genrsa -out server.key 4096
    ```

3. 生成服务端CSR `server.csr`
    ```
    C:\Apache24\bin\openssl.exe req -new -sha256 -out server.csr -key server.key -config server.conf
    ```

4. 生成证书 `server.crt`
    ```
    C:\Apache24\bin\openssl.exe x509 -req -days 3650 -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt -extensions req_ext -extfile server.conf
    ```

----

* 参考サイト
    * https://www.cnblogs.com/hld123/p/6343437.html
    * https://blog.csdn.net/qinwenbo114/article/details/47335757
    * https://argrath.ub32.org/annex/2018/01/08-18.html
    * https://jp.globalsign.com/support/ssl/confinfo/apache.html
    * https://qiita.com/fukushun-ka/items/b01b836f86f313a14d08
    * https://www.koikikukan.com/archives/2013/12/03-012345.php
    * [Apacheをプライベート認証局やオレオレ証明書でHTTPS化](https://pvision.jp/tech/2023/05/apache-how-to-enable-https/)
    * https://jprs.jp/pubcert/service/manual/install/install_Apache_v1.1.pdf
    * https://ajya.hatenablog.jp/entry/2012/07/20/223151
    * https://www.alibabacloud.com/help/zh/ssl-certificate/user-guide/install-ssl-certificates-on-apache-servers
    * https://www.ssl.ph/help/manual/instal/apache24.html
    
    * https://blog.csdn.net/MeiWenjilu/article/details/128244363
    * https://www.jianshu.com/p/5e53423b239f

    * プライベート認証局の構築
        * https://pvision.jp/tech/2023/05/web-server-https-support-by-private-ca/
        * https://blog.apar.jp/linux/8587/
        * https://hcm-jinjer.com/blog/e-sign/private-certificate-authority/
        * ★ https://nodejs.keicode.com/nodejs/openssl.php
        * ★ https://blog.csdn.net/qq_19734597/article/details/108142074
