# 簡化版Deployment實現

## 文章
[如何從零開始編寫一個CRD](https://mp.weixin.qq.com/s/z_xM8QqpRUASDM1UyMzEeA "With a Title").

## 目的
這是一個簡化版的Deployment的實現，實現了部分Kubernetes中Deployment的功能。

## 測試用例
- 啟動後自動創建出一個MyDeployment的CRD
    - 【觸發】啟動應用
    - 【期望】可以看到創建出來的CRD
    - 【測試】kubectl get CustomResourceDefinition -o yaml
- 創建一個MyDeployment: Nginx實例
    - 【觸發】kubectl create -f my-deployment-instance.yaml
    - 【期望】可以看到級聯創建出來的3個pod
    - 【測試】kubectl get pods
- 手工刪掉一個pod
    - 【觸發】kubectl delete pods $pod_name
    - 【期望】pod被重建
    - 【測試】kubectl get pods -w
- 暴露一個服務
    - 【觸發】kubectl create -f my-deployment-service.yaml
    - 【期望】可以通過curl來訪問nginx服務
    - 【測試】minikube service my-nginx-app --url 然後 curl
- 更新鏡像
    - 【觸發】kubectl replace -f my-deployment-instance-update-image-1.9.1.yaml
    - 【期望】pod的nginx版本被更新為1.9.1
    - 【測試】kubectl get pods -o yaml
- 擴容
    - 【觸發】kubectl replace -f my-deployment-instance-update-scaleup-1.9.1.yaml
    - 【期望】pod被擴容到5個
    - 【測試】kubectl get pods
- 縮容
    - 【觸發】kubectl replace -f my-deployment-instance-update-scaledown-1.9.1.yaml
    - 【期望】pod被縮容到2個
    - 【測試】kubectl get pods
- 擴容並更新鏡像
    - 【觸發】kubectl replace -f my-deployment-instance-update-image-and-scaleup-1.14.yaml
    - 【期望】pod被擴容5個，且nginx版本被更新為1.14
    - 【測試】kubectl get pods 然後 kubectl get pods -o yaml
- 刪除一個MyDeployment
    - 【觸發】kubectl delete mydeployments my-nginx-app
    - 【期望】MyDeployment被刪掉，並且關聯的pod也被級聯刪掉
    - 【測試】kubectl get mydeployments 然後 kubectl get pods
- 查看狀態(TODO)
- 回滾(TODO)
- 狀態更新【current，update-to-date，available】(TODO)
- describe EVENTS(TODO)
