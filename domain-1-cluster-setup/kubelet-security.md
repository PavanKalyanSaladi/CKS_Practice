#### Step 1: Access Worker Node Kubelet from Control Plane Node
```sh
cd /etc/kubernetes/pki
```
Change the IP address in below command to that of Worker Node IP
```sh
curl -k --cert apiserver-kubelet-client.crt --key apiserver-kubelet-client.key https://143.244.140.236:10250/pods
```
#### Step 2 Make a request to Kubelet API (Worker Node)
```sh
apt install net-tools
netstat -ntlp
curl -k -X GET https://localhost:10250/pods
```

#### Step 2 Modify the Kubelet Configuration file: (Worker Node)
```sh
cd /var/lib/kubelet
cp config.yaml config.yaml.bak
```

```sh
Set anonymous auth to false
Authorization Mode to AlwaysAllow
```

```sh
systemctl restart kubelet
systemctl status kubelet
```

#### Step 3 Make Request to Kubelet API: (Worker Node)
```sh
curl -k -X GET https://localhost:10250/pods
```
#### Step 4 Download kubeletctl: (Worker Node)

##### GitHub Repo

https://github.com/cyberark/kubeletctl

```sh
wget https://github.com/cyberark/kubeletctl/releases/download/v1.13/kubeletctl_linux_amd64 && chmod a+x ./kubeletctl_linux_amd64 && mv ./kubeletctl_linux_amd64 /usr/local/bin/kubeletctl
```

```sh
kubeletctl pods -i
kubeletctl run "whoami" --all-pods -i
```

#### Step 5: Verify Kubelet Certificate

```sh
openssl s_client -showcerts -connect 127.0.0.1:10250 2>/dev/null | openssl x509 -inform pem -noout -text
```

#### Step6: Chabge the config to authorized access

```sh
mv config.yaml.bak config.yaml

systemctl restart kubelet
```

```sh
curl -k -X GET https://localhost:10250/pods
```

---

Next: [18. Verifying Platform Binaries](verify-binaries.md) <br>
Previous: [16. Revising Taints and Tolerations](taint-toleration.md)
