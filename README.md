#  otel_collector_k8s

## What is this repository?

This is an example deploy file for deploying an Opentelemetry Collector do Kubernetes. This is meant to help as a starting point for more complex configurations.

## What will be deployed

Deploying this file as-is will create a collector that listens for jaeger traces on port 8080 and sends traces to Dynatrace otlphttp collector. You can tweak the config map to add more receivers and exporters.

## How to deploy?

1. Create a namespace for your collector: `kubectl create ns opentelemetry`
2. Deploy the file to that namespace: `kubectl apply -f opentelemetry-collector.yaml -n opentelemetry`

This will create a POD and a service that exposes the port 8080 on the cluster, point your instrumented applications traces to this endpoint. The service is being exposed as LoadBalancer, so services outside the Kubernetes cluster can reach them through the Cloud Provider LoadBalancer.