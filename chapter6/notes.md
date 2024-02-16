#### Securing Microservices Communications


- Isitio security architecture
- Authenticating using mutual TLS
- How to configure custom authentication and authorization policies



#### Authentication using mutual TLS.

- We create a curl first, using file : 06-curl-client-no-sidecar.yaml
- We create a http-bin file:  01-httpbin-deployment.yaml

We enter to the curl container

kubectl -n utilities -it exec curl -- curl -v http://httpbin.chapter6.svc.cluster.local:8000/get
we obtain success because we are not using mutual TLS

-We apply mtl using the file: 02-httpbin-strictTLS.yaml

no the previous curl will return
```bash
* Host httpbin.chapter6:8000 was resolved.
* IPv6: (none)
* IPv4: 10.106.43.213
*   Trying 10.106.43.213:8000...
* Connected to httpbin.chapter6 (10.106.43.213) port 8000
> GET /get HTTP/1.1
> Host: httpbin.chapter6:8000
> User-Agent: curl/8.6.0
> Accept: */*
>
* Recv failure: Connection reset by peer
* Closing connection
  curl: (56) Recv failure: Connection reset by peer
  command terminated with exit code 56
```

We now enable the side card with file: 07-curl-client-with-sidecar.yaml
we execute the previous curl now,the response is

```bash
{
  "args": {}, 
  "headers": {
    "Accept": "*/*", 
    "Host": "httpbin.chapter6.svc.cluster.local:8000", 
    "User-Agent": "curl/8.6.0", 
    "X-B3-Parentspanid": "6513e5dccee06aee", 
    "X-B3-Sampled": "1", 
    "X-B3-Spanid": "f34ba808718de0b2", 
    "X-B3-Traceid": "20d52e6f2efe05f76513e5dccee06aee", 
    "X-Envoy-Attempt-Count": "1", 
    "X-Forwarded-Client-Cert": "By=spiffe://cluster.local/ns/chapter6/sa/httpbin;Hash=57f98fb0b31d3518f9516052903e22313b1350f16dd1d99ccc926fc6e4bab00e;Subject=\"\";URI=spiffe://cluster.local/ns/utilities/sa/curl"
  }, 
  "origin": "127.0.0.6", 
  "url": "http://httpbin.chapter6.svc.cluster.local:8000/get"
}
```
jq -r '.. |."secret"?' | jq -r 'select(.name == "default")' | jq -r '.tls_certificate.certificate_chain.inline_bytes' | base64 -d - | step certificate inspect  --short
