#### Components of Istio control plane

The control plane is: a set of Istio services that are responsible for the operations of the Istio data plane.

- istiod

	Is one of the Istio control plane components, providing service discovery, configuration, and certificate management.
	
- Sidecar injection: The istio control plane also manages sidecar injectionvia mutating webhooks.
- Istio node agent: Node agents are deployed along with Envoy and take care of communication with the istio CA, providing the cert and keys to Envoy.
- Identity directory and registry: The istio control plane manages a directory of identities for various type of workloads that willl be used by the Istio CA to issue key/certs for
requested identities.
- End-user context propagation: Istio provides a secure mechanism to perfom end user authentication on Ingress and the propagate the user context to other services and apps
within the Service Mesh. The user context is propagated in JWT format, which helps to pass on user information to services within the mesh without needing to pass the
end user credentials.


#### Exploring Envoy, the Istio data plane

Envo is a lightweight, highly perfomant layer 7, and layer 4 proxy with an easy-to-use configuration system, making
it highly configurable and suitable for serving as a standalone edge-proxy in the API gateway architecture pattern, as well as running as a sidecar in the service mesh
architecture pattern.

Envoy also supports:

- Automatic retries
- Circuit breaking
- Global rate limit
- Traffic mirroring
- Outlier detection
- Request hedging


