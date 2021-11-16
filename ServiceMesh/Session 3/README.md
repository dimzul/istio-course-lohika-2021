#### Initial project instalation
0. Create a namespace: kubectl apply -f k8s/namespace.yaml
1. Enable sidecar for the istio-course namespace: kubectl label namespace istio-course istio-injection=enabled
2. Build java services: ./scripts/build-components.sh
3. Build docker images and push them: ./scripts/publish-components.sh
4. Apply services into k8s: ./scripts/deploy-components.sh
5. Delete all istio-course resources from k8s: ./scripts/delete-all.sh

#### Canary deployment
To roll canary deployment, please create DestinationRule with subsets of app versions and VirtualService with weighting for every subset. See ```book-v2.yaml``` or ```authors-v2.yaml```.
To view traffic distribution, open Kiali using ```istioctl dashboard kiali``` and go to Graph page.

Applied canary deployment settings for authors-v2 and books-v2:
<br/>
<img width="641" alt="canary-apply-authors-v2" src="https://user-images.githubusercontent.com/3698215/142068089-c32d4236-3fac-4a8d-b10d-cc0c32676431.png">
<br/>
<img width="631" alt="canary-apply-books-v2" src="https://user-images.githubusercontent.com/3698215/142068110-33542fa9-4e0c-45c1-aed7-58487dd041e6.png">
<br/>

Different responses for v1 and v2 verions:
<br/>
<img width="438" alt="canary-response-from-v1" src="https://user-images.githubusercontent.com/3698215/142068179-84f6882b-2840-4648-98f2-1a16a6389544.png">
<br/>
<img width="435" alt="canary-response-from-v2" src="https://user-images.githubusercontent.com/3698215/142068193-7ec36ec4-0415-4a24-8450-073685627787.png">

Traffic distribution in Kiali between v1/v2 as 90/10:
<img width="693" alt="canary-traffic-distribution-90-10" src="https://user-images.githubusercontent.com/3698215/142068261-801cb979-1e1f-451b-8665-ad08066a094b.png">

<br/>
Traffic distribution in Kiali between v1/v2 as 50/50:
<img width="733" alt="canary-traffic-distribution-50-50" src="https://user-images.githubusercontent.com/3698215/142068300-bfb272e4-f96c-411c-862e-e1fe3364ee76.png">

<br/>
Traffic distribution in Kiali between v1/v2 as 0/100:
<img width="861" alt="canary-traffic-distribution-0-100" src="https://user-images.githubusercontent.com/3698215/142068329-3f657b27-ad18-4e07-b79f-b5f61a8fb6bf.png">

#### Development environment
To route traffic to develpment environment deployed in parallel to production environment depending on request header, please create DestinationRule with subsets of app versions and VirtualService with HttpMatchRequest rule. This rule could have the header matcher with defined exact value. If rule matches header in request, traffic routed to corresponding subset from DestinationRule. In other case, to default destination. See ```fronted-v2.yaml``` for implementation details.

<br/>
Response for request with header containig developer name:
<img width="909" alt="frontend-v2-with-developer" src="https://user-images.githubusercontent.com/3698215/142066647-4a242a87-dd8e-4d7c-b654-a7d975b530c7.png">

<br/>
Response for request without developer name:
<img width="713" alt="frontend-v2-without-developer" src="https://user-images.githubusercontent.com/3698215/142066987-8a160a81-bb3c-42fc-acc1-a679768f70f0.png">
<br/>
Traffic routing in Kiali:
<img width="863" alt="frontend-v2-kiali-traffic-routing" src="https://user-images.githubusercontent.com/3698215/142067264-bd1dcc83-eadb-4161-ae5b-dc4ec51b5663.png">

#### Service resiliency
To introduce service resiliency via load balancing and/or circuit breaking, please create DestinationRule with sections:
* for load balancing: ```spec.trafficPolicy.loadBalancer.simple.LOAD_BALANCER_TYPE``` section;
* for circuit breaking: ```spec.trafficPolicy.connectionPool.http``` section for service overload settings or ```spec.trafficPolicy.connectionPool.http.outlierDetection``` for circuit breaker settings;

See ```course-service-load-balance-and-circuit-brake.yaml``` for implementation details.

Documentation:
* https://istio.io/latest/docs/reference/config/networking/destination-rule/#ConnectionPoolSettings
* https://istio.io/latest/docs/reference/config/networking/destination-rule/#OutlierDetection
* https://istio.io/latest/docs/tasks/traffic-management/circuit-breaking/
