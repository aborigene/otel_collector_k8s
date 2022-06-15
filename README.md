#  otel_collector_k8s

## What is this repository?

This is an example deploy file for deploying an Opentelemetry Collector do Kubernetes. This is meant to help as a starting point for more complex configurations.

## What will be deployed

Deploying this file as-is will create a collector that listens for jaeger traces on port 8080 and sends traces to Dynatrace otlphttp collector. You can tweak the config map to add more receivers and exporters.

## How to deploy?

1. Create a namespace for your collector: `kubectl create ns opentelemetry`
2. Create a Dynatrace token for ingesting OpenTelemetry tracing
3. Create a secret to hold that token: `kubectl -n opentelemetry create secret generic otel-collector-secret --from-literal "OTEL_ENDPOINT_URL=<TENANT-BASEURL>/api/v2/otlp" --from-literal "OTEL_AUTH_HEADER=Api-Token <API-TOKEN>"`
4. Deploy the file to that namespace: `kubectl apply -f opentelemetry-collector.yaml -n opentelemetry`

This will create a POD, with configuration to Dynatrace defined on its environment variables and a service that exposes the port 8080 on the cluster. Point your instrumented applications traces to this endpoint. The service is being exposed as LoadBalancer, so services outside the Kubernetes cluster can reach the collector through the IP defined on the Cloud Provider LoadBalancer.