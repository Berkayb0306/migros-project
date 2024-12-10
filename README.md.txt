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
├── README.md              # Documentation for the project
└── .gitignore             # Files and directories to ignore in version control
```

## Prerequisites
- **Docker Desktop**
- **KIND (Kubernetes in Docker)**
- **kubectl (Kubernetes CLI)**

## Setup Instructions
### 1. Clone the Repository
```bash
git clone <repository-url>
cd migros-project
```

### 2. Build the Docker Image
```bash
docker build -t migros-project:latest .
```

### 3. Create a Kubernetes Cluster with KIND
```bash
kind create cluster --name migros-project
```

### 4. Load the Docker Image into the KIND Cluster
```bash
kind load docker-image migros-project:latest --name migros-project
```

### 5. Apply Kubernetes Configurations
```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/ingress.yaml
```

### 6. Test the Deployment
- **NodePort Access:**
  Open a web browser and navigate to:
  ```
  http://localhost:30008
  ```

- **Ingress Access:**
  Add the following entry to your hosts file:
  ```
  127.0.0.1 migros.local
  ```
  Then, access the application at:
  ```
  http://migros.local
  ```

### 7. Port Forwarding (Optional)
If needed, forward the service to a specific port:
```bash
kubectl port-forward service/migros-service 80:80
```
Access the application at:
```
http://localhost
```

## Troubleshooting
### Common Issues
1. **Ingress Not Working:**
   - Ensure the NGINX Ingress Controller is running:
     ```bash
     kubectl get pods -n ingress-nginx
     ```
   - Check the ingress logs:
     ```bash
     kubectl logs -n ingress-nginx ingress-nginx-controller-XXXX
     ```

2. **Port Conflict:**
   - Ensure no other services are using the same port.
   - Use `netstat` to identify conflicts:
     ```bash
     netstat -ano | findstr :80
     ```

### Reset the Cluster
If issues persist, delete and recreate the cluster:
```bash
kind delete cluster --name migros-project
kind create cluster --name migros-project
```

## Contribution
Contributions are welcome! Feel free to fork the repository and submit pull requests.

## License
This project is licensed under the MIT License.

