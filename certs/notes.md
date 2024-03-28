# Generate ca

root.conf

```bash

[ req ]
default_bits             = 4096
prompt                   = no
default_md               = sha256
req_extensions           = req_ext
distinguished_name       = dn

[ dn ]
C                        = US
ST                       = State
L                        = City
O                        = Organization Name
OU                       = Organizational Unit Name
CN                       = Root CA

[ req_ext ]
subjectAltName           = @alt_names
basicConstraints         = CA:TRUE

[alt_names]
DNS.1                    = root.example.com


```

### Generate the key

openssl genrsa -out root.key 4096

### Generate the cert


openssl req -x509 -new -nodes -key root.key -days 1024 -out root.crt -config root.conf -extensions req_ext

inspect the certificate

openssl x509 -in root.crt -text -noout | less

