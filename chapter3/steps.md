### Download envoy

	docker pull envoyproxy/envoy:v1.22.2

	docker run -v ${PWD}/envoy-config-1.yaml:/envoy-custom.yaml -p 9901:9901 -p 10000:10000 --rm -it envoyproxy/envoy:v1.22.2 -c /envoy-custom.yaml

	let's run docker run -p 8080:80 nginxdemos/hello:plain-text


