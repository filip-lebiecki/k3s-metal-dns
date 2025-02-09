[Youtube video](https://youtu.be/jDQSKYKOgLA?si=ZXbjujI-Qm92YmUU)

# K3s install

```
curl -sfL https://get.k3s.io | sh -s - server --cluster-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/16 --disable=servicelb
install -D /etc/rancher/k3s/k3s.yaml .kube/config
```

# MetalLB install

```
kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.14.9/config/manifests/metallb-native.yaml
vi metallb-addr-pool.yaml
kubectl apply -f metallb-addr-pool.yaml
vi metallb-advertise.yaml
kubectl apply -f metallb-advertise.yaml
```

# PiHole install

```
cat pihole-ns.yaml
kubectl apply -f pihole-ns.yaml
cat pihole-pv.yaml
kubectl apply -f pihole-pv.yaml
vi pihole-deployment.yaml
kubectl apply -f pihole-deployment.yaml
vi pihole-svc.yaml
kubectl apply -f pihole-svc.yaml
host google.com 192.168.10.101
```

Go to http://192.168.10.101/admin
Settings -> DNS -> Permit All Origins -> Save

# Mikrotik configuration

```
ssh 192.168.10.200
/certificate
add name=ca-cert common-name=mikrotik days-valid=3650 key-usage=key-cert-sign,crl-sign
sign ca-cert
add name=https-cert common-name=mikrotik days-valid=3650
sign ca=ca-cert https-cert
/ip service enable www-ssl
/ip service set www-ssl certificate=https-cert
/user add name=dns password=dns group=write address=192.168.10.230/32 
/user print
```

# ExternalDNS install

```
cat external-dns-ns.yaml
kubectl apply -f external-dns-ns.yaml
cat external-dns-secret.yaml
kubectl apply -f  external-dns-secret.yaml
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm repo add external-dns https://kubernetes-sigs.github.io/external-dns/
helm repo update
vi external-dns-values.yaml
helm upgrade --install --namespace external-dns external-dns external-dns/external-dns -f external-dns-values.yaml
```

# PiHole add DNS entry

```
vi pihole-svc-full.yaml
kubectl apply -f pihole-svc-full.yaml
```

http://pihole.lab.local/admin

```
ssh 192.168.10.200 ip dns static print
kubectl delete -f pihole-svc-full.yaml
ssh 192.168.10.200 ip dns static print
```

# CodeServer install

```
cat code-ns.yaml
kubectl apply -f code-ns.yaml
cat code-pv.yaml
kubectl apply -f code-pv.yaml
cat code-deployment.yaml
kubectl apply -f code-deployment.yaml
vi code-svc.yaml
kubectl apply -f code-svc.yaml
```

http://code.lab.local/

```
ssh 192.168.10.200 ip dns static print
```

