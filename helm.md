1. 添加Repo
``` shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
3. 查看Repo
``` helm repo list ```
3. 安装Chart
``` helm install xxx repo/name```
例子:
``` helm install monitoring  prometheus-community/kube-prometheus-stack ```
4. 