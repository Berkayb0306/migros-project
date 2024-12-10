# Migros Project - 2048 Game Deployment

## Project Overview
This project demonstrates the deployment of the 2048 game application on a local Kubernetes cluster using KIND (Kubernetes in Docker). The application is made accessible via a web browser, following a well-structured deployment process to facilitate collaboration and management.

## Objectives
- Deploy the 2048 game application on a local Kubernetes cluster.
- Ensure the application is accessible via a web browser.
- Maintain a well-organized repository structure.
- Provide clear documentation for collaboration and usage.
- Manage files using Git and push the source code to a remote repository (GitHub or GitLab).

## Repository Structure
```
migros-project/
├── app/
│   ├── index.html         # Frontend HTML file for the 2048 game
│   ├── styles.css         # Stylesheet for the game
│   └── app.js             # JavaScript logic for the game
├── k8s/
│   ├── deployment.yaml    # Kubernetes deployment configuration
│   ├── service.yaml       # Kubernetes service configuration
│   └── ingress.yaml       # Kubernetes ingress configuration
├── Dockerfile             # Dockerfile to containerize the application
├── kind-config.yaml       # KIND configuration file for port mapping
├── README.md              # Documentation for the project
└── .gitignore             # Files and directories to ignore in version control
```

## Prerequisites
- **Docker Desktop**
- **KIND (Kubernetes in Docker)**
- **kubectl (Kubernetes CLI)**

## Setup Instructions

### 1. Clone the Repository
Clone the repository and navigate to the project directory:
```bash
git clone <repository-url>
cd migros-project
```

### 2. Build the Docker Image
Build the Docker image for the 2048 game application:
```bash
docker build -t migros-project:latest .
```

### 3. Create a Kubernetes Cluster with KIND
Ensure that the `kind-config.yaml` file exists in the root directory and includes port mapping for `NodePort`. Create the cluster:
```bash
kind create cluster --name migros-project --config kind-config.yaml
```

### 4. Load the Docker Image into the KIND Cluster
Load the Docker image into the KIND cluster:
```bash
kind load docker-image migros-project:latest --name migros-project
```

### 5. Apply Kubernetes Configurations
Apply the Kubernetes deployment, service, and ingress configurations:
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### 6. Test the Deployment

#### NodePort Access
Open a web browser and navigate to:
```plaintext
http://localhost:30008
```

#### Ingress Access (Optional)
Add the following entry to your `/etc/hosts` file:
```plaintext
127.0.0.1 migros.local
```
Then, access the application at:
```plaintext
http://migros.local
```

## Troubleshooting

### Common Issues

#### Pod Status Pending
- Ensure the Docker image is loaded into the KIND cluster:
  ```bash
  kind load docker-image migros-project:latest --name migros-project
  ```
- Verify resource limits in the `deployment.yaml`.

#### Ingress Not Working
- Ensure the NGINX Ingress Controller is installed and running:
  ```bash
  kubectl get pods -n ingress-nginx
  ```
- Check the ingress logs:
  ```bash
  kubectl logs -n ingress-nginx ingress-nginx-controller-XXXX
  ```

#### Port Conflict
- Ensure no other services are using the same port.
- Use `netstat` to identify conflicts:
  ```bash
  netstat -ano | findstr :30008
  ```

### Reset the Cluster
If issues persist, delete and recreate the cluster:
```bash
kind delete cluster --name migros-project
kind create cluster --name migros-project --config kind-config.yaml
```

## Contribution
Contributions are welcome! Feel free to fork the repository and submit pull requests.

## License
This project is licensed under the MIT License.

