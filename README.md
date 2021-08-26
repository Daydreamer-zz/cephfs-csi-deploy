# 一个cephfs和kubernetes StorageClass集成示例
要求ceph版本>=14.2.2，建议使用Nautilus版
并且ceph集群已经创建好存储池和文件系统
## 创建命名空间
```bash
kubectl create namespace ceph
```
## 修改csi-config-map.yaml
将clusterID和monitors修改为你的ceph集群信息
## 修改secret.yaml
将userID和userKey修改为您的ceph集群认证信息
## 修改storageclass
将clusterID改为您的ceph集群id，fsName修改为您的ceph集群的cephfs文件系统名称
## 创建csi
```bash
kubectl create -f .
```
