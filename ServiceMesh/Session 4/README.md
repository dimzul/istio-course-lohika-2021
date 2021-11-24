#### Initial project instalation
0. Create a namespace: kubectl apply -f k8s/namespace.yaml
1. Enable sidecar for the istio-course namespace: kubectl label namespace istio-course istio-injection=enabled
2. Build java services: ./scripts/build-components.sh
3. Build docker images and push them: ./scripts/publish-components.sh
4. Apply services into k8s: ./scripts/deploy-components.sh
5. Delete all istio-course resources from k8s: ./scripts/delete-all.sh

#### Fault injection
##### Delay injection
To introduce delay to service calls at the application layer, please create VirtualService with ```spec.http.fault.delay``` section describing desired delay. 
For implementation details see ```/ServiceMesh/Session 4/k8s/gateway/author-delay.yml``` .
For documentation see https://istio.io/latest/docs/concepts/traffic-management/#fault-injection and https://istio.io/latest/docs/tasks/traffic-management/fault-injection/#injecting-an-http-delay-fault.
<br/>
Response examples with and without delay injection:
img1
<br/>
img2

##### Fault injection
To introduce application faults to service calls at the application layer, please create VirtualService with ```spec.http.fault.abort``` section describing desired service failre response. 
For implementation details see ```/ServiceMesh/Session 4/k8s/gateway/author-fault.yml``` .
For documentation see https://istio.io/latest/docs/concepts/traffic-management/#fault-injection and https://istio.io/latest/docs/tasks/traffic-management/fault-injection/#injecting-an-http-abort-fault.
<br/>
Response examples with and without fault injection:
img1
<br/>
img2