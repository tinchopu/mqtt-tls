# MQTT TLS Configuration

Following instructions [here](https://mosquitto.org/man/mosquitto-tls-7.html)

## Generate CA
```sh
openssl req -new -x509 -days 4000 -extensions v3_ca -keyout ca.key -out ca.crt
```

## Generate Server certs
```sh
openssl genrsa -des3 -out server.key 2048
openssl genrsa -out server.key 2048
openssl req -out server.csr -key server.key -new
#Send the CSR to the CA, or sign it with your CA key:
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 4000
```

## Generate client certs (Common Name *CN* is username)

Don't add password as to be able to spare it when sending message
```sh
openssl genrsa -out client.key 2048 
openssl req -out client.csr -key client.key -new
#Send the CSR to the CA, or sign it with your CA key:
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 4000
```

## Add CA / Server CRT/Key to Kubectl

```sh
kubectl create secret generic mqtt-ca-secret --from-file=<path>

kubectl create secret tls mqtt-tls-secret --key <private key filename> --cert <certificate filename>
```
