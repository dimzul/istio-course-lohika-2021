### Helper

0. Create a namespace: kubectl apply -f k8s/namespace.yaml
1. Enable sidecar for the istio-course namespace: kubectl label namespace istio-course istio-injection=enabled
2. Build java services: ./scripts/build-components.sh
3. Build docker images and push them: ./scripts/publish-components.sh
4. Apply services into k8s: ./scripts/deploy-components.sh
5. Delete all istio-course resources from k8s: ./scripts/delete-all.sh

#### Canary deployment
To roll canary deployment, please create DestinationRule with subsets of app versions and VirtualService with weighting for every subset. See book-v2.yaml or authors-v2.yaml
To view traffic distribution, open Kiali using 'istioctl dashboard kiali' and go to Graph page.

#### Development environment
To route traffic to develpment environment deployed in parallel to production environment depending on request header, please create DestinationRule with subsets of app versions and VirtualService with HttpMatchRequest rule. This rule could have the header matcher with defined exact value. If rule matches header in request, traffic routed to corresponding subset from DestinationRule. In other case, to default destination. See fronted-v2.yaml for implementation details.

#### Service resiliency
To inroduce service resiliency via load balancing and/or circuit breaking, please create DestinationRule with sections:
	* for load balancing: spec.trafficPolicy.loadBalancer.simple.LOAD_BALANCER_TYPE section;
	* for circuit breaking: spec.trafficPolicy.connectionPool.http section for service overload settings or spec.trafficPolicy.connectionPool.http.outlierDetection for circuit breaker settings;
See course-service-load-balance-and-circuit-brake.yaml for implementation details.
Documentation:
* https://istio.io/latest/docs/reference/config/networking/destination-rule/#ConnectionPoolSettings
* https://istio.io/latest/docs/reference/config/networking/destination-rule/#OutlierDetection
* https://istio.io/latest/docs/tasks/traffic-management/circuit-breaking/