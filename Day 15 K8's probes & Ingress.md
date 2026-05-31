PROBES (HEALTHCHECK):

PROBES: used to determine the health and readiness of containers running within pods. Probes are 3 types:
Readiness probes are used to indicate when a container is ready to receive traffic. If the readiness probe fails, Kubernetes removes the pod’s IP from the Service’s endpoint list.
Liveness probes are used to determine whether a container is still running and responding to requests.
Startup Probe are used to determines whether the application within the container has started successfully. It's used to delay the liveness and readiness probes until the application is ready to handle traffic.

request → container
