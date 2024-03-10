# ElasticStack部署流程：
1. 将elastic-stack的第250行替换为自己的PersistentVolume的StorageClass
2. 替换镜像源：k8s.gcr.io:elasticsearch:v6.2.5 为 josephdzhoume/elasticsearch:v6.2.5
3. 替换镜像源: k8s.gcr.io:fluentd-elasticsearch:v2.2.0 为 josephdzhoume/fluentd-elasticsearch:v2.2.0
4. apply fluentd-config.yaml
5. apply elasticstack.yaml
