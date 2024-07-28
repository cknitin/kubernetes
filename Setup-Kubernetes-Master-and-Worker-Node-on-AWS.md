## Setup Kubernetes Master and Worker Node on AWS

```
sudo su
apt-get update
apt-get install apt-transport-https
```

```
apt install docker.io -y
docker --version
systemctl start docker
systemctl enable docker
```

```
sudo curl -s https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXk5QkFRclFqenNVSHVWVGhqQms1SWFsaFRKQXxBQ3Jtc0trSlBEN0cxRV90WXRXZ2lvX21IS2pFT1VSWVYyQW9INzZ5a3VhTC1XZEZ2LS1RUkE0RS1VR2tFMUFEek9vS2FlVHFGRGJFV2pzcGZCOTFBNEZod0hYX0RGc1J0dE0zZEQwUHVEZGFWV2hrWlBrbUdCOA&q=https%3A%2F%2Fpackages.cloud.google.com%2Fapt%2Fdoc%2Fapt-key.gpg&v=ftrAFHL6w2c | sudo apt-key add 
```

```
nano /etc/apt/sources.list.d/kubernetes.list
```

```
deb https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa1VfR1VKRm1JMW5DWFpleDNQdWhjd0F0UWNMd3xBQ3Jtc0tuS1V0TTdWUkxBMWJaQ05HTHdpV1I1ekRmdkd3M2h3TllNSzFiZm9JbmtqZkx6OWo2bHNmNTlyS1IwRTJLX0E3bk56QXA5TXNsTnpHalFqZkFkS0gwbjNWWU5KNTI5bUE1TkM3RWkxamp3OC1kTnJkMA&q=http%3A%2F%2Fapt.kubernetes.io%2F&v=ftrAFHL6w2c/ kubernetes-xenial main
```

```
apt-get update
```

```
apt-get install -y kubelet kubeadm kubectl kubernetes-cni
```

## BOOTSTRAPPING THE MASTER NODE (IN MASTER)

```
kubeadm init
```

## COPY THE COMMAND TO RUN IN NODES & SAVE IN NOTEPAD

```
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
```

```
chown $(id -u):$(id -g) $HOME/.kube/config
```

```
kubectl apply -f https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqazRxVnhXc2xDVk1EbzFPazlvNHN6czVlSHhjQXxBQ3Jtc0ttVkVESXdUUmpFSmV6SVRfeW00a0toMWJRejJzSF9WQ1F6a0dwSVVCQWh1dG95dHRJOHJIMEJKc1p3cFM1Q29DazNkMkgzQzgzYzhiUFRXdk9rcGMxTjhBTHViRTVjdERpdzdoYmlBNkRhQlhUV1VySQ&q=https%3A%2F%2Fraw.githubusercontent.com%2Fcoreos%2Fflannel%2Fmaster%2FDocumentation%2Fkube-flannel.yml&v=ftrAFHL6w2c
```
```
kubectl apply -f https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbnp6eFJpWkkwR0h0a1VEQmM5Wkc2ZDhoT0JNZ3xBQ3Jtc0trOXB5dXhmOXZKUGhubVctZmhwVkpmbG1IVmFjYWQxd2J4WHNzdk84RHVJYWx6MVJKVEhXRk44XzJlYm5DSFBPTDFucU9JM0xmdHdUazVRbDRJSi1qVTNfai04b0swQ0hhSEVaUFl0TjhfaW9Hcm5qdw&q=https%3A%2F%2Fraw.githubusercontent.com%2Fcoreos%2Fflannel%2Fmaster%2FDocumentation%2Fk8s-manifests%2Fkube-flannel-rbac.yml&v=ftrAFHL6w2c
```

## CONFIGURE WORKER NODES (IN NODES)

### COPY LONG CODE PROVIDED MY MASTER IN NODE NOW LIKE CODE GIVEN BELOW

```
e.g- kubeadm join 172.31.6.165:6443 --token kl9fhu.co2n90v3rxtqllrs --discovery-token-ca-cert-hash sha256:b0f8003d23dbf445e0132a53d7aa1922bdef8d553d9eca06e65c928322b3e7c0
```

## GO TO MASTER AND RUN THIS COMMAND

```
kubectl get nodes
```

