# Fresh node install

#### Register data on environemnt
```
export DOMAIN=mortenkristensen.dk \
export EMAIL=mortenkristensen13@gmail.com \
export USE_HOSTNAME=k8s-master-01
```


#### Apply host name
```
echo $USE_HOSTNAME > /etc/hostname
hostname -F /etc/hostname
```

#### Install dependencies
```
sudo apt update && \
sudo apt upgrade -y && \
sudo apt install curl -y && \
sudo apt install ca-certificates -y && \
sudo apt install open-iscsi -y && \
sudo apt install wireguard -y && \
sudo apt install nfs-common -y
```


```
git clone https://github.com/askblaker/k3s.rocks.git \
cd k3s.rocks/manifests
```


### Install k3s
#### Master
```
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.28.3+k3s2 sh -s server \
--cluster-init \
--flannel-backend=wireguard-native \
--write-kubeconfig-mode 644 && \
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml && \
cat traefik-config.yaml | envsubst | kubectl apply -f 
```

Get token
```
cat /var/lib/rancher/k3s/server/node-token
```

#### Agent

```
export K3S_TOKEN=K10dc4aefed9b891c63409cad18efc028c22b81334c07b5bdadf68782b2e491a30e::server:cbefac3be93785d49f9fae1fb4266136
export MASTER_IP=78.47.131.255
```

```
curl -sfL https://get.k3s.io | INSTALL_K3S_VERSION=v1.28.3+k3s2 K3S_TOKEN="${K3S_TOKEN}" sh -s server \
--flannel-backend=wireguard-native \
--server https://${MASTER_IP}:6443
```

#### See all nodes

```
kubectl get nodes
```