# Emissary Ingress Quickstart

```
# Add the Repo:
helm repo add datawire https://app.getambassador.io
helm repo update

# Create Namespace and Install:
kubectl create namespace emissary && \
kubectl apply -f https://app.getambassador.io/yaml/emissary/3.8.2/emissary-crds.yaml

kubectl wait --timeout=90s --for=condition=available deployment emissary-apiext -n emissary-system

helm install emissary-ingress --namespace emissary datawire/emissary-ingress && \
kubectl -n emissary wait --for condition=available --timeout=90s deploy -lapp.kubernetes.io/instance=emissary-ingress

kubectl apply -f https://app.getambassador.io/yaml/v2-docs/3.8.2/quickstart/qotm.yaml

# start tracing service
kubectl apply -f tracing.yaml
```

The quote service should be available here: http://localhost/backend/

# OpenTelemetry Collector

Note: Make sure to add your Honeycomb API Key to the `opentelemetry-collector-values.yaml` file.

```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts

helm install otelcol open-telemetry/opentelemetry-collector --set mode=deployment

helm upgrade otelcol open-telemetry/opentelemetry-collector --set mode=deployment --values opentelemetry-collector-values.yaml
```

You should see traces coming in to your Opentelemetry collector and to a dataset in your Honeycomb environment called `emissary`
