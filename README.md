#  cephfs-csi-deploy
一个cephfs和kubernetes StorageClass集成示例，驱动镜像已经同步至阿里云镜像仓库，要求ceph版本>=14.2.2，建议使用Nautilus版，并且ceph集群已经创建好存储池和文件系统
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
kubectl -n ceph create -f .
```
## 设置为默认StorageClass（可选）
如果需要配置为默认的StorageClass请执行
```bash
kubectl patch storageclass csi-cephfs-sc -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## 测试pvc
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: csi-cephfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
  storageClassName: csi-cephfs-sc
```
## 测试pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: csi-cephfs-demo-pod
spec:
  containers:
    - name: web-server
      image: docker.io/library/nginx:latest
      volumeMounts:
        - name: mypvc
          mountPath: /var/lib/www
  volumes:
    - name: mypvc
      persistentVolumeClaim:
        claimName: csi-cephfs-pvc
        readOnly: false
```
