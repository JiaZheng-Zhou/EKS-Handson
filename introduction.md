# 本地 Yaml与EKS Yaml的区别：
1. 如果需要通过EKS来自动部署EBS Volume，需要执行如下命令： 

    **Step1：**
    ``` shell
    eksctl utils associate-iam-oidc-provider --region=YOUR-REGION --cluster=YourClusterNameHere --approve
    ```
    **Step2：**
    ``` shell 
    eksctl create iamserviceaccount --name ebs-csi-controller-sa --namespace kube-system --cluster YourClusterNameHere --attach-policy-arn arn:aws:iam::aws:policy/service-role/AmazonEBSCSIDriverPolicy --approve  --role-only  --role-name AmazonEKS_EBS_CSI_DriverRole
    ```
    **Step3：**
    ``` shell
    eksctl create addon --name aws-ebs-csi-driver --cluster YourClusterNameHere --service-account-role-arn arn:aws:iam::$(aws sts get-caller-identity --query Account --output text):role/AmazonEKS_EBS_CSI_DriverRole --force
    ```
2. 存储PersistentVolume应该使用**EBS**而不应该使用**HostPath**的本地存储方式
3. Service层面Type不应该使用**NodePort**，如果是**对外开放**的服务，改变为**LoadBalancer**，如果是**集群内部**服务，更改为**ClusterIP**




