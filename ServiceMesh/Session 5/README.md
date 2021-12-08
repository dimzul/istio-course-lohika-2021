#### Initial project instalation
0. Create a namespace: kubectl apply -f k8s/namespace.yaml
1. Enable sidecar for the istio-course namespace: kubectl label namespace istio-course istio-injection=enabled
2. Build java services: ./scripts/build-components.sh
3. Build docker images and push them: ./scripts/publish-components.sh
4. Apply services into k8s: ./scripts/deploy-components.sh
5. Delete all istio-course resources from k8s: ./scripts/delete-all.sh

#### Security
##### mTLS for service-to-service communication
To enable mTLS for service-to-service communication please create PeerAuthentication with ```spec.mtls.mode:STRICT``` in order to enable s2s communication only via mTLS (see ```/ServiceMesh/Session 5/k8s/gateway/security.yml:PeerAuthentication```). 

For other modes please see: https://istio.io/latest/docs/concepts/security/#authentication
<br/>
For other examples of implementation details please see: https://istio.io/latest/docs/tasks/security/authentication/authn-policy/#enable-mutual-tls-per-namespace-or-workload
<br/>

Screenshot from Kiali showing s2s communication is performed using mTLS (lock icon on s2s calls):
<img width="867" alt="kiali-mtls" src="https://user-images.githubusercontent.com/3698215/145100889-891c8a60-1e99-41c7-bfe6-a49f7b4a0b48.png">

##### End-user authentication/authorization with JWT
To enable end-user request authentication please create RequestAuthentication with ```spec.jwtRules``` section decribing desired JWT rules. This will allow requests only with JWT issued by issuer testing@secure.istio.io (see ```/ServiceMesh/Session 5/k8s/gateway/security.yml:RequestAuthentication```). 
After that it's required to create AuthorizationPolicy to deny requests to frontend service without request principals (see ```/ServiceMesh/Session 5/k8s/gateway/security.yml:AuthorizationPolicy```).

For documentation on Auth0 please see: https://auth0.com/docs/security/tokens/json-web-tokens/json-web-key-sets
<br/>
For documentation on Istio's authorization concept please see: https://istio.io/latest/docs/concepts/security/#authorization
<br/>
For other examples of implementation details please see: https://istio.io/latest/docs/tasks/security/authentication/authn-policy/#end-user-authentication and https://istio.io/latest/docs/tasks/security/authorization/authz-jwt/
<br/>

Screnshot showing that end-user requests with mock auth token are not allowed:
<img width="1294" alt="end-user-request-with-wrong-jwt-auth-not-authorized" src="https://user-images.githubusercontent.com/3698215/145101084-873a8501-c7ac-40a6-bb3f-ef1d4da47068.png">
<br/>

Screnshot showing that end-user requests with valid auth token are allowed:
<img width="1281" alt="end-user-request-with-valid-jwt-auth-authorized" src="https://user-images.githubusercontent.com/3698215/145101167-a21dd3d0-43da-49bc-8928-78cadfec697f.png">
