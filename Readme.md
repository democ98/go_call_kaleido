# Untrusted-Enclave Remote Attestation code sample

## Custom CA/client setup

To establish a TLS channel, we need a CA and generates a client cert for mutual authentication. We store them at `cert`.

1. Generate CA private key
openssl ecparam -genkey -name prime256v1 -out ca.key

2. Generate CA cert
openssl req -x509 -new -SHA256 -nodes -key ca.key -days 3650 -out ca.crt

3. Generate Client private key
openssl ecparam -genkey -name prime256v1 -out client.key

4. Export the keys to pkcs8 unencrypted format
openssl pkcs8 -topk8 -nocrypt -in client.key -out client.pkcs8

5. Generate Client CSR
openssl req -new -SHA256 -key client.key -nodes -out client.csr

6. Generate Client Cert
openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost,DNS:www.example.com") -days 3650 -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt

7. Intel CA report signing pem. Download and uncompress:
https://software.intel.com/sites/default/files/managed/7b/de/RK_PUB.zip

## Run

Start project (golang should be installed)
```
cd ue-ra-client-go
make
cd bin
./app
```
