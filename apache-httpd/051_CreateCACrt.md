
## 1. 認証局(CA)を構築する手順 (Windows)

1. OpenSSL のインストール

    流用apache httpd自带的OpenSSL

    システム環境変数 `PATH` に `%apache_home%\bin` を追加します。

2. CA 用のディレクトリの作成

    ここでは `C:\CA` というフォルダ以下に環境を構築するものとします。 もし同じ名前のフォルダが既にあって使われていたら、適当にパスを読み替えてください。

    `C:\CA` 内に、次の内容を `setup.bat` という名前で作成してください。

    ```cmd
    @mkdir root\certs
    @mkdir root\crl
    @mkdir root\newcerts
    @mkdir root\private
    @mkdir root\csr
    @type nul > root\index.txt
    @type nul > root\crlnumber
    @echo 1000 > root\serial
    @mkdir intermediate\certs
    @mkdir intermediate\crl
    @mkdir intermediate\newcerts
    @mkdir intermediate\private
    @mkdir intermediate\csr
    @type nul > intermediate\index.txt
    @type nul > intermediate\crlnumber
    @echo 1000 > intermediate\serial
    ```

2. 准备配置文件 `ca.conf`

    `C:\CA\root` 内に以下の内容を `openssl.cfg` という名前で作成してください。

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

2. 秘密鍵, 生成ca密钥(ca.key)
    ```
    openssl genrsa -out ca.key 4096
    ```

3. 公開鍵, 生成ca证书签发请求(ca.csr)
    ```
    openssl req -new -sha256 -out ca.csr -key ca.key -config ca.conf
    ```
4. 証明書, 生成ca根证书(ca.crt)
    ```
    openssl x509 -req -days 3650 -in ca.csr -signkey ca.key -out ca.crt
    ```
