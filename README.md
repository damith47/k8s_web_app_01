✅ Project: Jenkins CI/CD Pipeline for Dockerized Kubernetes Deployment

![Alt text](https://github.com/damith47/k8s_web_app_01/blob/b5c17dab218278ad22c9294d5e45dc6810328915/Diagram.png)

--CI/CD pipeline using Jenkins, Docker, and Kubernetes--

1. Builds a Docker image for a frontend web app.

2. Pushes it to Docker Hub.

3. Deploys or updates a pod on your local Kubernetes cluster.

4. Exposes the app on a fixed NodePort so you can access it via browser.
___________________________________________________________________________________

🔧 Tools and Technologies Used

Tool	                Purpose

Jenkins	                Automates the build, Docker image creation, and deployment
Docker	                Packages your web app into an image
Docker Hub	            Stores the built image remotely
Kubernetes (k8s)	    Orchestrates container deployment
YAML	                Defines k8s Deployment & Service
VirtualBox VMs	        Used to run your local Kubernetes cluster (master + workers)
___________________________________________________________________________________

🚀 How to Host the Web App 

🖥️ 1. Develop Your Web App

- Simple HTML + CSS frontend.

- Store in a GitHub or local Git repo.

- Include a Dockerfile to containerize the app.
___________________________________________________________________________________

⚙️ 3. Setup Jenkins Build Pipeline

Jenkinsfile:

- Builds Docker image with tag lorexhub/lorexweb1:v1

- Pushes it to Docker Hub

- Replaces the image tag in your k8s YAML

- Deploys to Kubernetes
___________________________________________________________________________________

☸️ 4. Kubernetes YAML (as you shared)

Deployment:

    - Always replaces old pod when a new image is deployed.

    - Keeps only one running pod.

Service:

    - NodePort (port 30080) exposes the app externally.
___________________________________________________________________________________

🌐 5. Access Your App
Once deployed, visit in your browser:

- http://<your-node-ip>:30080
eg:
- http://192.168.8.103:30080
___________________________________________________________________________________

🧪 Test a Change
Edit index.html

Commit & push

Trigger Jenkins build

Jenkins:

Rebuilds Docker image

Pushes to Docker Hub

Updates Kubernetes pod

Visit http://nodeIP:30080 to see the updated app.

