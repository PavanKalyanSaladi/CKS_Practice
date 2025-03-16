### Pre-Requisite:

Complete the setup mentioned in `Practical - Nginx with TLS` lecture

### Study the default behaviour
```sh
kubectl get service -n ingress-nginx

curl -kv https://example.internal:<https-node-port>

curl -I http://example.internal:<http-node-port>
```
### Modify the Ingress
```sh
kubectl edit ingress demo-ingress
```
```sh
metadata:
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
```

### Test the Setup
```sh
curl -I http://example.internal:<http-node-port>
```

---

Next: [22. Structure of Network Policy](netpol-structure.md) <br>
Previous: [21. Practical - Ingress with TLS](ingress-security.md)
