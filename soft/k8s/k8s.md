# k8s

## 查询`pod`
kubectl get pods | grep server_name
kubectl get pods | grep server_name | awk '{print $1}'

## 端口映射
kubectl port-forward pods/pod_node 8080:8080
kubectl port-forward pods/`kdd get pods | grep server_name | awk '{print $1}'` 8080:8080

## 打印日志
kdd logs -f --tail=300 `kdd get pods | grep server_name | awk '{print $1}'`

## 应用、删除新的deployment.yaml
kdd apply -f ./deployment.yaml
kdd get deployment | grep server_name
kdd get replicaset | grep server_name
kdd get pod -o wide | grep server_name
kdd delete -f ./deployment.yaml

## 查看k8s中docker镜像
kdd get pods --all-namespaces -o jsonpath="{...image}" | tr -s '[[:space:]]' '\n' | sort | uniq -c