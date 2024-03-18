# 在本地激活Metrics Server：
1. `minikube addons list` 查看插件列表，确认metrics-server是否Enabled
2. `minikube addons enable metrics-server` 如果还未安装该插件，进行安装
3. 确认插件已安装
4. `kubectl top pod` / `kubectl top node`测试插件是否正常运行（需等待一分钟左右）

# 在本地激活Metrics Dashboard
1. `minikube addons list` 查看插件列表，确认metrics-server是否Enabled
2. `minikube addons enable dashboard` 如果还未安装该插件，进行安装
3. 确认插件已安装
4. `minikube dashboard` 查看Dashboard