# Istio04: Observability - Telemetry & Visualization

<!-- TOC -->
- [Istio04: Observability - Telemetry & Visualization](#istio04-observability---telemetry--visualization)
  - [Generate Load on Bookinfo](#generate-load-on-bookinfo)
  - [Grafana](#grafana)
  - [Prometheus](#prometheus)
  - [Kiali](#kiali)

## Generate Load on Bookinfo
Let's generate HTTP traffic against the BookInfo application, so we can see interesting telemetry. Grab the ingress gateway port number and host:

```sh
INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
```

Now, let us generate a small load on the sample app by using [fortio](https://github.com/istio/fortio) which is a load testing library created by the `Istio` team:

The command below will run load test by making 5 calls per second for 5 minutes:
```sh
docker run istio/fortio load -t 5m -qps 5 http://$GATEWAY_URL/productpage
```

Let's now checkout the generated metrics.


## Grafana

If you have not already exposed grafana, please follow [Module Istio01](istio-01-setup.md). Once you have exposed grafana, please access to its dashboard. You can then navigate to the `Istio Dashboard`.

![](../assets/Grafana_Istio_Dashboard.png)


## Prometheus
If you have not already exposed prometheus, please follow [Module Istio01](istio-01-setup.md). Once you have exposed Prometheus, please access to its dashboard. Browse to `/graph` and in the `Expression` input box enter: `istio_request_count`. Click the Execute button.

![](../assets/Prometheus.png)

For a more interactive graph, navigate to `force/forcegraph.html`.

## Kiali

If you have not already exposed kiali, please follow [Module Istio01](istio-01-setup.md). Once you have exposed kiali, please access to its dashboard. You can then navigate to the `Kiali Dashboard`.

![](../assets/kiali.png)

Go to [official Kiali site](https://www.kiali.io) for more detail

---
[Istio Top](aks-202-istio-top.md)| [Back](istio-03-ingress-gateway.md) | [Next](istio-05-distributed-tracing.md)