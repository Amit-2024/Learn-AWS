# EKS (Elastic Kuberenetes Service)
## workflow of a request in EKS.

## Load Balancer (Configured by the Ingress Controller)
When you deploy an Ingress Controller (like AWS Load Balancer Controller, NGINX, or Traefik), it typically provisions an external load balancer (e.g., an AWS ALB or NLB) to handle incoming requests. This load balancer is the public-facing entry point to your cluster.

## Ingress Resource
The Ingress resource defines the routing rules for incoming requests. It specifies:

Hostnames (e.g., example.com)
Path-based routing (e.g., /api routed to one service, /web routed to another)
Optional TLS settings for HTTPS termination.
The Ingress Controller watches the Ingress resources and configures the load balancer accordingly to route traffic.

## Service
The Ingress resource forwards requests to a Kubernetes Service. The service acts as a stable endpoint that directs traffic to a group of pods (backend applications).
Depending on the service type, it can use different mechanisms:

## ClusterIP: For routing traffic within the cluster.
## NodePort: For exposing pods via node IPs and ports.
## LoadBalancer: (Rare in this case, as the Ingress Controller uses its own load balancer).
## Pod
The service forwards traffic to one of the pods matching the selector in its configuration. Pods are the smallest deployable units in Kubernetes and contain your application containers. The traffic finally reaches the desired application instance.

## Visual Flow Diagram:
> Client Request → Load Balancer (Ingress Controller) → Ingress Resource → Service → Pod (Application)
Key Notes:
Ingress Controller: It is the bridge between the Ingress resource and the external load balancer.
Load Balancer Type: For AWS, the ALB is commonly used, which is dynamically configured by the Ingress Controller.
Service and Pod Communication: Kubernetes uses kube-proxy or other networking solutions to forward traffic from the service to pods efficiently.
