1. 如何查看pod部署在哪个node上:
``` shell
kubectl get pods -o wide
```

2. 快速apply整个文件夹的yaml文件：
```
kubectl apply -f .
```
3. 水平扩展命令
```
kubectl autoscale deployment xxx --cpu-percent 400 --min 1 --max 4
```
`400是指request cpu的百分比，400即400%，即实际cpu为request的cpu 4倍时触发扩展`
```
kubectl get hpa
```
`可以通过上述命令查看拓展命令`




