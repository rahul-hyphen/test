CI/CD pipeline using Jenkins, Ansible, Docker, and AWS for automated app deployment.

1. Objective
--> Automate the deployment of a web application and server configuration using Jenkins and Ansible, with Docker for containerized deployment.
--> Instead of manual setup, Jenkins triggers Ansible playbooks that build Docker images and run containers automatically on the target node.


2. Tech Stack

-> Jenkins → CI/CD automation
-> Ansible → Server configuration & deployments
-> Linux Server (Ubuntu/) → Master for configuration & Target node for app deployment
-> GitHub → Stores code, Jenkinsfile, Ansible playbooks, and Dockerfile
-> Docker → Builds image and runs application inside a container


3. Architecture Flow

Step 1 → Developer pushes code to GitHub
Step 2 → Jenkins (on Master) triggers pipeline
Step 3 → Jenkins runs Ansible playbook
Step 4 → Ansible connects via SSH to Node server
Step 5 → Docker installed, image built, container deployed
Step 6 → Application accessible at:  http://<NODE_IP>:8080


4. Repository Structure 

jenkins-ansible-docker/
│
├── Jenkinsfile      # Jenkins pipeline definition  
├── playbook.yml     # Ansible playbook to configure Docker & deploy app  
├── inventory        # Ansible inventory with target server details  
├── Dockerfile       # Build containerized application  
├── index.html       # Sample web application (deployed inside container)  
└── README.md        # Project documentation  


5. Tools & Plugins Required

--> On Jenkins Master
Ansible Plugin
Git
Pipeline Plugin 
SSH Agent Plugin 
GitHub Integration Plugin
Blue Ocean (Optional For better view of pipeline)


##  Author  ## 
Ankit Choudhary
💡 Exploring real-world DevOps with Jenkins, Ansible, and Docker — making deployments smarter, faster, and hands-on.
