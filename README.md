# Basic Example of Echo service with envoy

# Setup local kind cluster
kind create cluster

# Deploy

`kubectl apply -f goechoapp-envoy.yaml`

## Get Service
`kubectl get svc`

## Port forward
`kubectl port-forward svc/envoy-sidecar-service 8443:8443`

Request the go echo app via envoy proxy

curl -k https://127.0.0.1:8443/echo\?text\=hello

# Generate local certificate

https://letsencrypt.org/sv/docs/certificates-for-localhost/

Run command to generate local cert and key
```
openssl req -x509 -out localhost.crt -keyout localhost.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

## Update tls.key and tls.crt in the secrets
`cat localhost.crt| base64`
`cat localhost.key| base64`