# istio-course-lohika-2021

## How to install Istio on local machine

* Download and install Docker Desktop 
  * manually from https://www.docker.com/products/docker-desktop
  * using brew: `brew install --cask docker`
* Install k9s using `brew install derailed/k9s/k9s`
* Install Kubernetes Dashboard https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
* Install Istio https://istio.io/latest/docs/setup/getting-started
  <img width="1062" alt="Installed Istio" src="https://user-images.githubusercontent.com/3698215/139463689-2f465cb5-1adc-459a-a596-e0722a59e9a1.png">
  
  * Enable Kiali in helm template during installation https://istio.io/docs/tasks/telemetry/kiali/
    * using `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.11/samples/addons/kiali.yaml`
    <img width="1007" alt="Installed Kiali" src="https://user-images.githubusercontent.com/3698215/139464530-3e427ba0-ca26-4ae2-b4dc-62d922049065.png">
    
  * Configure Jaeger tracing https://istio.io/docs/tasks/observability/distributed-tracing/jaeger/
    * using `kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.11/samples/addons/jaeger.yaml`
    <img width="1010" alt="Installed Jaeger" src="https://user-images.githubusercontent.com/3698215/139465366-c0e5dc02-c911-403a-8d3d-321220e12dbd.png">

Example of output of installed Istio:
<img width="1010" alt="Istio pods" src="https://raw.githubusercontent.com/dimzul/istio-course-lohika-2021/main/homework-1/homework_1.png">
