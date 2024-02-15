### Managing Application Resiliency

- Istio supports:

HTTP delay: timing failure. Injected by virtual services
HTTP abort: aborts the processing of the request: you can also specify the error code that needs to be returned downstream.


```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: catalog
spec:
  hosts:
  - "catalogue.sock-shop.svc.cluster.local"
  gateways:
    - mesh
  http:
  - name: "catalogue"
    match:
     - uri:
        prefix: /catalogue
    fault:
      delay:
        percent: 100
        fixedDelay: 7s
      abort:
        percent: 100
        httpStatus: 500
    route:
    - destination:
        host: catalogue.sock-shop.svc.cluster.local
        port:
          number: 80
  - name: default
    route:
    - destination:
        host: catalogue.sock-shop.svc.cluster.local
        port:
          number: 80
```
