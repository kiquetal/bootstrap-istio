### Deploying sockshop

git clone https://github.com/microservices-demo/microservices-dem


### Execute

kubectl apply  -f microservices-demo/deploy/kubernetes/manifests/00-sock-shop-ns.yaml 

kubectl create -f  sockshop/devops/deploy/kubernetes/manifests/* -n sock-shop


### We will modify the frontend that is currently using the NodePort to access via Ingres

 kubectl apply -f ingress.yaml


### Create namespace

	kubectl create ns chapter4
	kubectl label namespace chapter4 istio-injection=enabled --overwrite
	kubectl create configmap envoy-dummy --from-file=../chapter3/envoy-config-1.yaml


### Create new ingress 2-istio-ingress.yaml

  kubectl apply -f 2-istio-ingress.yaml
  
![istio-controller-ingress.png](../images/istio-controller-ingress.png)

### We prefer to use Gateway instead of ingress

 Istio Gateway is like a load balancer running at the edge of the mesh receiving incoming or outgoing HTTP/TCP connections. It configures exposed ports, protocols, etc. Istio Gateway also supports traffic management features such as load balancing, routing, authentication, and authorization.

### Remove the ingress

      kubectl delete -f 2-istio-ingress.yaml
      kubectl delete -f ingress.yaml

### Create the gateway


      kubectl apply -f 3-istio-gateway.yaml

### Traffic routing and canary release

    We will use DestinationRule for traffic routing and virtualService
    We will use another envoy-proxy.

Create a new config map for a new configuration for the envoyproxy

    kubectl create configmap envoy-dummy-2 --from-file=envoy-config-2.yaml -n chapter4

Create a new envoyproxy deployment and service

    kubectl apply -f 02-envoy-proxy.yaml

Now we will create another virtual host to select between the 2 versions

    kubectl apply -f 4a-istio-gateway.

![traffic-mirroring-chapter4.png](../images/traffic-mirroring-chapter4.png)
![istio-subset-services-vs.png](../images/istio-subset-services-vs.png)

    kubectl delete -f 4a-istio-gateway.yaml

### Traffic mirroring

    Is a technique that allows you to send a copy of production traffic to a test or staging environment. This is useful for testing new versions of your application before releasing them to production.


   The repo is: https://github.com/PacktPublishing/Bootstrap-Service-Mesh-Implementations-with-Istio/blob/main/Chapter4/4b-istio-gateway.yaml
   
   We will install nginx
   
    kubeclt apply -f nginx.yaml

   The we will modify the virtualService to mirror 100 from the envoy

    kubeclt apply -f 4b-istio-gateway.yaml

    curl -H "Host: mockshop.com" http://10.108.141.169 <IP OF THE LoadBalancer istio>

![istio-mirroring-explain.png](../images/istio-mirroring-explain.png)
