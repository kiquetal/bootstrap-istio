### Deploying sockshop

git clone https://github.com/microservices-demo/microservices-dem


### Execute

kubectl apply  -f microservices-demo/deploy/kubernetes/manifests/00-sock-shop-ns.yaml 

kubectl create -f  sockshop/devops/deploy/kubernetes/manifests/* -n sock-shop

