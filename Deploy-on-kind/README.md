# Deployment of Opentelemetry Demo app on Kind Kubernetes cluster
- OpenTelemetry Astronomy Shop, a microservice-based distributed system intended to illustrate the implementation of OpenTelemetry in a near real-world environment.
#
### Pre-requisites for this project:
- EC2 instance (Ubuntu) with 4 CPU, 16GM of RAM (t2.xlarge) and 28GB of storage
- Kubernetes 1.24+
- 6 GB of free RAM for the application
- Kind Cluster
- Kubectl
#
### Steps to implement for deployment
1. Update your instance
  ```bash
  sudo apt update -y
  ```

2. Install docker and provide the necessary permisssions
  ```bash
  sudo apt install docker.io -y
  sudo usermod -aG docker $USER && sudo newgrp docker
  ```

3. Clone the repository
  ```bash
  git clone https://github.com/DevMadhup/Opentelemetry.git
  ```

4. Navigate to Deploy-on-kind directory
  ```bash
  cd Opentelemetry/Deploy-on-kind
  ```

5. Create kind.sh script and install kind
  ```bash
  #!/bin/bash
  # For AMD64 / x86_64
  [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.20.0/kind-linux-amd64
  chmod +x ./kind
  sudo cp ./kind /usr/local/bin/kind
  rm -rf kind
  ```

6. Setup Kind cluster
  ```bash
  kind create cluster --config=config.yml
  ```

7. Install kubectl
  ```bash
  curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
  chmod +x ./kubectl
  sudo mv ./kubectl /usr/local/bin
  kubectl version --short --client
  ``` 

8. Check cluster information:
  ```bash
  kubectl cluster-info --context kind-kind
  kubectl get nodes
  kind get clusters
  ```

9. Navigate to kubernetes directory
  ```bash
  cd ../kubernetes/
  ```

10. Apply manifest
  ```bash
  kubectl apply -f opentelemetry-demo.yaml
  ```

11. Once all the pods are up and running, check the services
  ```bash
  kubectl get svc
  ```

12. To access the application use port-forwarding
  ```bash
  kubectl port-forward svc/
  ```
