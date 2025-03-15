#### Step 1 - Generate Client CSR and Client Key:
```sh
cd /root/certificates
```
```sh
openssl genrsa -out saladi.key 2048

openssl req -new -key saladi.key -subj "/CN=saladi" -out saladi.csr
```
#### Step 2 - Sign the Client CSR with Certificate Authority
```sh
openssl x509 -req -in saladi.csr -CA ca.crt -CAkey ca.key -out saladi.crt -days 1000
```
#### Step 3 - Verify Client Certificate
```sh
openssl x509 -in saladi.crt -text -noout

openssl verify -CAfile ca.crt saladi.crt
```

#### Step 4 - Delete the Client Certificate and Key
```sh
rm -f saladi.crt saladi.key saladi.csr
```

---

Next: [4. etcd - Transport Security with HTTPS](etcd-https.md) <br>
Previous: [2. Configure Certificate Authority](configure-ca.md)
