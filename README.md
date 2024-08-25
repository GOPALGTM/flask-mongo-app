# Flask Application with MongoDB on Kubernetes

This repository contains the Flask application and MongoDB setup for deployment on a Minikube Kubernetes cluster. Below are the steps to build the Docker image, push it to a container registry, and deploy the application on Minikube, along with explanations of the configurations and testing scenarios.


## Prerequisites
Docker installed on your machine.

Minikube installed and running.

kubectl command-line tool installed.

Access to a container registry (e.g., Docker Hub).

### Steps:

#### Build the Docker Image:

$ docker build -t dockerhub-username/flask-app:latest .

#### Push the Docker Image:

$ Push the Docker dockerhub-username/flask-app:latest .

#### Start Minikube:

$ minikube start

#### Apply the MongoDB Persistent Volume and Persistent Volume Claim:

$ kubectl apply -f k8s/mongo-pv.yaml

$ kubectl apply -f k8s/mongo-pvc.yaml

#### create secret for mongodb:

$ kubectl apply -f mongo-secret.yaml

#### Deploy MongoDB StatefulSet and Service:

$ kubectl apply -f k8s/mongodb-statefulset.yaml.yaml

#### Deploy the Flask Application Deployment and Service:

$ kubectl apply -f k8s/flask-deployment.yaml

#### Apply the Horizontal Pod Autoscaler:

$ kubectl apply -f flask-hpa.yaml




## DNS Resolution in Kubernetes
Kubernetes uses CoreDNS to manage DNS resolution within the cluster. Each pod can access other services using their DNS names, which are constructed as <service-name>.<namespace>.svc.cluster.local. This allows the Flask application to connect to MongoDB using its service name (mongodb).

## Resource Requests and Limits
Resource requests and limits ensure that your application has the necessary resources to run efficiently.

Requests: The minimum amount of CPU/memory required for the pod.
Limits: The maximum amount of CPU/memory the pod can use.
Setting these helps the Kubernetes scheduler make informed decisions when placing pods on nodes and prevents a single pod from monopolizing resources.

## Design Choices
StatefulSet for MongoDB: StatefulSet is used for MongoDB because it maintains a unique identity for each pod and allows for stable network identities, which is crucial for databases.

ClusterIP Service for MongoDB: A ClusterIP service is used to ensure MongoDB is only accessible within the Kubernetes cluster, enhancing security.

NodePort for Flask: NodePort is used for the Flask application to allow external access while keeping the configuration simple.

### Alternatives Considered

ReplicaSet for MongoDB: Not used because MongoDB requires stable storage and network identities.

LoadBalancer for Flask: Could be used for production but requires a cloud provider.


## Testing Scenarios

Horizontal Pod Autoscaling (HPA) Testing:

Tool Used: Apache JMeter

Objective: Verify that the Flask application scales correctly under load.

Approach: Simulated high traffic by sending multiple POST requests to the /data endpoint using JMeter.

Results: HPA successfully scaled the number of Flask pods in response to increased load, demonstrating effective autoscaling.


