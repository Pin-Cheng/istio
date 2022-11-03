
# Istio Install

#### 1. Get cluster KUBE_CONFUG
```shell
gcloud container cluster get-credentials <cluster-anme> --region <region> --project <project_id>
```
#### 2. Modify manifests gateways istio-ingress values.yaml  and mark secretVolumes
```yaml
podAnnotations: 
type: NodePort

secretVolumes: []
# - name: ingressgateway-certs
#     secretName: istio-ingressgateway-certs
#     mountPath: /etc/istio/ingressgateway-certs
# - name: ingressgateway-ca-certs
#     secretName: istio-ingressgateway-ca-certs
#     mountPath: /etc/istio/ingressgateway-ca-certs
```

#### 3. Helm install
 ```shell
 kubectl create namespace istio-system 
 cd istio/
 helm install istio-base manifests/charts/base -n istio-system
 helm install istio-ingress manifests/charts/gateways/istio-ingress -n istio-system
 helm install istiod manifests/charts/istio-control/istio-discovery -n istio-system
 ```
#### 4. Install isito operator
```shell
 cd istio/
 helm install istio-operator manifests/charts/istio-operator -n istio-system
```
#### 5. Install gtstudio setting
```shell
kubectl apply -f ../gtstudio_config/monitor-infra/istio-ilbgateway.yaml

kubectl apply -f ../gtstudio_config/monitor-infra/service_ingress.yaml
```
authorizationPolicy & monitor_network 依需求 kubectl apply