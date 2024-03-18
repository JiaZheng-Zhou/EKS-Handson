# 创建集群与安装插件
``` shell
eksctl create cluster --name xxx --nodes-min=3
``` 

 ``` shell
eksctl utils associate-iam-oidc-provider --region=YOUR-REGION --cluster=YourClusterNameHere --approve
```

``` shell 
eksctl create iamserviceaccount --name ebs-csi-controller-sa --namespace kube-system --cluster YourClusterNameHere --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy --approve  --role-only  --role-name AmazonEKS_EBS_CSI_DriverRole
```

``` shell
eksctl create addon --name aws-ebs-csi-driver --cluster YourClusterNameHere --service-account-role-arn arn:aws-cn:iam::$(aws sts get-caller-identity --query Account --output text):role/AmazonEKS_EBS_CSI_DriverRole --force
```

# 开始部署
``` shell
kubectl apply -f storage.yaml -f mongo-stack.yaml
```

**检查PV以及PVC是否正常**

``` shell
kubectl apply -f workloads.yaml -f service.yaml
```

**检查应用是否正常运行**
ELB-DNS:30080

``` shell
kubectl apply -f fluentd-config.yaml -f elastic-stack.yaml
```

**检查Kibana是否正常运行**
kubectl get svc -n kube-system
KIBANA-DNS:5601


``` shell
kubectl apply -f crds.yaml 
```

``` shell
kubectl apply -f eks-monitoring.yaml
```
**检查Promethues和Grafana是否正确运行**
kubectl get svc -n monitoring   
grafana: admin prom-operator