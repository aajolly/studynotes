### Virtual Service

What is a Virtual Service
![Istio Virtual Service](/images/istio/istio_service.png)

Dissecting a Virtual Service YAML

```yaml
kind: VirtualService
apiVersion: networking.istio.io/v1alpha3
metadata:
  name: a-set-of-routing-rules-we-can-call-this-anything  # "just" a name for this virtualservice
  namespace: default
spec:
  hosts:
    - fleetman-staff-service.default.svc.cluster.local  # The Service DNS (ie the regular K8S Service) name that we're applying routing rules to.
  http:
    - route:
        - destination:
            host: fleetman-staff-service.default.svc.cluster.local # The Target DNS name
            subset: safe-group  # The name defined in the DestinationRule
          weight: 90
        - destination:
            host: fleetman-staff-service.default.svc.cluster.local # The Target DNS name
            subset: risky-group  # The name defined in the DestinationRule
          weight: 10
---
kind: DestinationRule       # Defining which pods should be part of each subset
apiVersion: networking.istio.io/v1alpha3
metadata:
  name: grouping-rules-for-our-photograph-canary-release # This can be anything you like.
  namespace: default
spec:
  host: fleetman-staff-service # Service
  subsets:
    - labels:   # SELECTOR
        version: safe # find pods with label "safe"
      name: safe-group
    - labels:
        version: risky
      name: risky-group
```

### Load Balancing in Istio
#### Session Affinity (`Stickiness`)

Q. Is it possible to use the weighted destination rules to makle a single user `stick` to a canary?
A. No, not using weightings

![Consistent Hashing](/images/istio/consistent_hashing.png)

### Istion Ingress Gateways

Dissecting the Ingress Gateway configuration
```yaml
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: ingress-gateway-configuration
spec:
  selector:
    istio: ingressgateway # use Istio default gateway implementation
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*"   # Domain name of the external website
---
kind: VirtualService
apiVersion: networking.istio.io/v1alpha3
metadata:
  name: fleetman-webapp
  namespace: default
spec:
  hosts:      # which incoming host are we applying the proxy rules to???
    - "*" # Copy the value in the gateway hosts - usually a Domain Name
  gateways:
    - ingress-gateway-configuration
  http:
    - route:
        - destination:
            host: fleetman-webapp
            subset: original
          weight: 90
        - destination:
            host: fleetman-webapp
            subset: experimental
          weight: 10
---
## Prefix based routing
---
kind: VirtualService
apiVersion: networking.istio.io/v1alpha3
metadata:
  name: fleetman-webapp
  namespace: default
spec:
  hosts:      # which incoming host are we applying the proxy rules to???
    - "*"
  gateways:
    - ingress-gateway-configuration
  http:
    - match:
      - uri:  # IF
          prefix: "/experimental"
      - uri:  # OR
          prefix: "/canary"
      route: # THEN
      - destination:
          host: fleetman-webapp
          subset: experimental
    - match:
      - uri :
          prefix: "/"
      route:
      - destination:
          host: fleetman-webapp
          subset: original
---
## Host based or sub-domain routing
---
apiVersion: networking.istio.io/v1alpha3
kind: Gateway
metadata:
  name: ingress-gateway-configuration
spec:
  selector:
    istio: ingressgateway # use Istio default gateway implementation
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*.fleetman.com"   # Domain name of the external website
    - "fleetman.com"
---
kind: VirtualService
apiVersion: networking.istio.io/v1alpha3
metadata:
  name: fleetman-webapp
  namespace: default
spec:
  hosts:      # which incoming host are we applying the proxy rules to???
    - "fleetman.com"
  gateways:
    - ingress-gateway-configuration
  http:
    - route:
      - destination:
          host: fleetman-webapp
          subset: original
---
kind: VirtualService
apiVersion: networking.istio.io/v1alpha3
metadata:
  name: fleetman-webapp-experiment
  namespace: default
spec:
  hosts:      # which incoming host are we applying the proxy rules to???
    - "experimental.fleetman.com"
  gateways:
    - ingress-gateway-configuration
  http:
      - route:
        - destination:
            host: fleetman-webapp
            subset: experimental
```

