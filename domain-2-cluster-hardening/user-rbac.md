### Websites Referred in Video:

https://fusionauth.io/dev-tools/jwt-decoder

https://www.unixtimestamp.com/

### Create a ServiceAccount
```sh
kubectl create sa test-sa
```

### Generate Token
```sh
kubectl create token test-sa --duration=100h
```

---

Next: [2. Practical - Role and RoleBinding](role-rolebinding.md)
