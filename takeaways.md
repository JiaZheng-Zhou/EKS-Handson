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
6. 
