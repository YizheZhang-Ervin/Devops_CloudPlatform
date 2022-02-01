# KubeSphere Install
# A. KubeSphere前置环境

## 1. nfs文件系统
### 1.1 安装nfs-server
```
# 在每个机器。
yum install -y nfs-utils

# 在master 执行以下命令 
echo "/nfs/data/ *(insecure,rw,sync,no_root_squash)" > /etc/exports

# 执行以下命令，启动 nfs 服务;创建共享目录
mkdir -p /nfs/data

# 在master执行
systemctl enable rpcbind
systemctl enable nfs-server
systemctl start rpcbind
systemctl start nfs-server

# 使配置生效
exportfs -r

#检查配置是否生效
exportfs
```

### 1.2 配置nfs-client
```
showmount -e 172.31.0.4

mkdir -p /nfs/data

mount -t nfs 172.31.0.4:/nfs/data /nfs/data
```

### 1.3 配置默认存储

## 2. metrics-server

# B. 安装KubeSphere
## 1.下载核心文件
```
wget https://github.com/kubesphere/ks-installer/releases/download/v3.1.1/kubesphere-installer.yaml

wget https://github.com/kubesphere/ks-installer/releases/download/v3.1.1/cluster-configuration.yaml
```

## 2. 修改cluster-configuration

## 3. 执行安装
```
kubectl apply -f kubesphere-installer.yaml

kubectl apply -f cluster-configuration.yaml
```

## 4. 查看安装进度
```
kubectl logs -n kubesphere-system $(kubectl get pod -n kubesphere-system -l app=ks-install -o jsonpath='{.items[0].metadata.name}') -f

kubectl -n kubesphere-monitoring-system create secret generic kube-etcd-client-certs  --from-file=etcd-client-ca.crt=/etc/kubernetes/pki/etcd/ca.crt  --from-file=etcd-client.crt=/etc/kubernetes/pki/apiserver-etcd-client.crt  --from-file=etcd-client.key=/etc/kubernetes/pki/apiserver-etcd-client.key
```