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

## Key Notes:
### Ingress Controller: It is the bridge between the Ingress resource and the external load balancer.
### Load Balancer Type: For AWS, the ALB is commonly used, which is dynamically configured by the Ingress Controller.
### Service and Pod Communication: Kubernetes uses kube-proxy or other networking solutions to forward traffic from the service to pods efficiently.

# AWS Elastic Kubernetes Service (EKS) Deep Dive

AWS Elastic Kubernetes Service (EKS) is a managed Kubernetes service that simplifies running Kubernetes on AWS without managing the control plane or infrastructure. This guide covers EKS concepts, setup, and a real-world example of deploying a scalable web application.

---

## Features

- **Fully Managed Control Plane**: Automatically manages the availability and scalability of Kubernetes control plane nodes.
- **Seamless AWS Integration**: Integrates with AWS services like IAM, VPC, and CloudWatch.
- **Flexibility in Compute**: Use Amazon EC2, AWS Fargate, or external nodes.
- **Multi-Region Clusters**: High availability with automatic failover across multiple regions.
- **Security**: Supports IAM for authentication, network isolation, and encryption.
- **Custom Add-ons**: Easily integrate Kubernetes-native tools (e.g., Prometheus, Helm).

---

## Key Concepts

### 1. **Cluster**
A logical grouping of Kubernetes resources where applications run. The control plane manages scheduling, scaling, and cluster state.

### 2. **Nodes**
The worker machines that run containerized applications. Can be:
- **Amazon EC2 instances**
- **AWS Fargate serverless containers**

### 3. **Pod**
The smallest deployable unit in Kubernetes, consisting of one or more containers.

### 4. **Namespace**
Logical partitions within a cluster to isolate resources, typically for multi-tenant workloads.

### 5. **Deployment**
Defines the desired state of application pods and automatically ensures the desired number of replicas.

### 6. **Service**
Exposes application pods to external traffic (e.g., LoadBalancer or ClusterIP).

### 7. **Ingress**
Manages external HTTP/S traffic routing to services using rules.

---

## Workflow Overview

### 1. **Create an EKS Cluster**
Set up an EKS control plane with a managed node group or self-managed EC2 instances.

### 2. **Configure kubectl**
Use `kubectl` to interact with the cluster.

### 3. **Deploy Applications**
Define deployments, services, and ingress rules using YAML manifests.

### 4. **Monitor and Scale**
Use tools like CloudWatch, Prometheus, or Grafana for monitoring. Kubernetes' Horizontal Pod Autoscaler (HPA) can scale pods automatically.

---

## Real-World Example: Deploying a Scalable Python Web Application

### **Scenario**
A company wants to deploy a Flask-based web application that:
1. Scales automatically during traffic spikes.
2. Is accessible over HTTPS using an Application Load Balancer (ALB).
3. Logs metrics to CloudWatch for monitoring.

---

### Step 1: Create an EKS Cluster
#### Using eksctl (Easiest Way):
```bash
eksctl create cluster \
  --name flask-app-cluster \
  --region us-east-1 \
  --nodegroup-name flask-nodes \
  --nodes 3 \
  --nodes-min 1 \
  --nodes-max 5 \
  --managed
Step 2: Deploy the Application
1. Create a Deployment YAML
flask-deployment.yaml:

yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
  labels:
    app: flask-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: <your_ecr_repo>/flask-app:latest
        ports:
        - containerPort: 5000
        env:
        - name: FLASK_ENV
          value: production
2. Apply the Deployment
bash
Copy code
kubectl apply -f flask-deployment.yaml
Step 3: Expose the Application
Create a Service YAML
flask-service.yaml:

yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: LoadBalancer
  selector:
    app: flask-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
Apply the Service:
bash
Copy code
kubectl apply -f flask-service.yaml
Step 4: Secure with Ingress and TLS
Create an Ingress YAML
flask-ingress.yaml:

yaml
Copy code
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: flask-ingress
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP":80,"HTTPS":443}]'
    alb.ingress.kubernetes.io/certificate-arn: <your_certificate_arn>
spec:
  rules:
  - host: flask-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: flask-service
            port:
              number: 80

Apply the Ingress:
bash
Copy code
kubectl apply -f flask-ingress.yaml
Step 5: Monitor and Scale
Enable Horizontal Pod Autoscaler (HPA)

kubectl autoscale deployment flask-app --cpu-percent=50 --min=2 --max=10

```

### Use CloudWatch for Monitoring
View metrics such as CPU utilization, memory usage, and ALB request counts in the AWS Management Console.
Benefits of EKS
### Cost Optimization: Pay for only the control plane and worker nodes used.
### High Availability: Multi-AZ deployments ensure resilience.
### Ease of Management: No need to manage Kubernetes control plane updates or security patches.
### Scalability: Handles small to large-scale workloads effortlessly.

## Best Practices

#### Use Node Groups Wisely: Choose between managed, self-managed, or Fargate nodes based on your workload.
#### Monitor Regularly: Use tools like CloudWatch, Prometheus, and Grafana to monitor cluster performance.
## Secure Clusters:
1. Enable RBAC for granular permissions.
2. Use private networking for sensitive workloads.
Optimize Costs: Use Spot Instances or smaller EC2 types (e.g., t3.medium) for cost efficiency.
CI/CD Integration: Automate deployments using tools like GitHub Actions or AWS CodePipeline.

