# 一个cephfs和kubernetes StorageClass集成示例
## 创建命名空间
```bash
kubectl create namespace ceph
```
## 修改csi-config-map.yaml
将clusterID和monitors修改为你的ceph集群信息
