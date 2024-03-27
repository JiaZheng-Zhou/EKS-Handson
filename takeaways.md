### HorizontalPodAutoscaler(HPA)
1. 解决了单个Pod资源达到一定限度后需要增加或者减少Pod的问题

### Probes
1. 解决当HPA自动扩容时，新的Pod还没有准备完毕可以接收请求时，就接收到请求的问题

### Eviction
1. 设置Limit与Request为不同情况时，Pod会有一个自动生成的Label:QoS
    设置为相同值: Granteed
    只有Request: Burstable
    都不设置: BestEffort
    Eviction顺序: BestEffort -> Burstable -> Granteed
2. 还有一个Pod Priority系统，与QoS可以交叉使用

### ConfigMap
1. 解决的是公共配置如何在一处修改影响全局
2. 通过`kubectl describe cm xxx`可以查看值
3. 使用方式
单个引用
```yaml
      env:
        - name: SPRING_PROFILES_ACTIVE
          value: production-microservice
        - name: DATABASE_URL
          valueFrom:
            configMapKeyRef:
              name: global-database-config
              key: database.url
```
整体引用
```yaml
      envFrom:
      - configMapKeyRef:
          name: global-database-config      
```
作为Volume被挂载
```yaml
      volumes:
      - name: database-config-volume
        configMap:
          name: global-database-config     
```
4. 查看方式,进入Pod，执行 `echo $NAME`
5. **更改ConfigMap，不会影响当前运行中Pod的环境变量**，想要修改的时候，一般直接修改configmap的version，并且修改所有其引用
6. Spring Cloud Kubernetes技术支持实时变更配置，教程：https://www.youtube.com/watch?v=DiJ0Na8rWvc&t=563s

### Secret
1. 不能直接使用字符串，必须使用base64进行编码
`echo -n 'value' | base64`
2. 如果使用stringData的配置方式，可以直接提供字符串，k8s会替用户进行编码
3. decribe 的时候，不会直接显示内容，而是只会显示字符串长度
4. 使用`kubectl get secret xxxx -o yaml`还是可以看到secret的内容
5. 编码过的字符串，通过`echo 'value' | base64 -d`即可还原

### Ingress
1. 对于本地Minikube环境需要激活插件
`minikube addons enable ingress`
2. 可以通过查看Pod确认是否开启成功
`kubectl get po -n kube-system`
3. 没有进行配置默认会使用Default Backend
4. 可以做Basic Authentication
5. 云上部署需要下载官方的yaml文件 k8s-ingress-controller
6. 查看部署是否成功
`kubectl get all -n ingress-nginx`
7. 配置支持Https教程: https://www.youtube.com/watch?v=gEzCKNA-nCg

### Other Workload Types
1. Job：执行结束就完成，不用长期运行的工作
2. CornJob：定时执行的任务
3. Daemonset: 保证集群中的每个Node都有相应的Pod运行

