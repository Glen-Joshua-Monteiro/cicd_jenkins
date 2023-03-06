# cicd_jenkins
An attempt to automate the Containerization to Deployment lifecycle using Jenkins. 
This is for a setup consisting of: 
- Jenkins > 2.17x
- GitLab/Github for SCM and Version Control
- Any Kubernetes Cluster (Setup using kubedm/EKS/AKS/GKE)

**Incase you plan on using the script without any alterations, please refer to the prerequisites mentioned below*.*

**Prerequistes:**
Once Jenkins has been setup, ensure connectivity (Inbound and Outbound) to the server or domain where your SCM and Version control tools are hosted.
Also ensure connectivity to the Kubernetes cluster.
Kubectl component of the relevant version and Git utility must be installed on the Jenkins server.

**1) On Jenkins**
**The following plugins need to be installed:** 
("Manage Jenkins -> Manage Plugins -> Available Plugins")

- Hidden Parameter plugin 
- Docker API Plugin
- Docker Commons Plugin
- Docker Pipeline
- Docker plugin
- Kubernetes
- Kubernetes CLI Plugin
- Kubernetes Client API
- Kubernetes Credentials Plugin
- Kubernetes Credentias Provider

**Create the folowing credentials at a System level**
("Manage Jenkins -> Manage Credentials -> Add Credentials")

- GitLab/Github token (For Integrating GitLab/Github with the Jenkins)

- GitLab/Github Credentials (In username-password format for git-checkout)
Name: git_credentials

- Registry Credentials (In username-password format for pushing the containerized image)
Name: docker_credentials

- Kube-Config file (In secret file format for cluster access and authentication )
Name: KUBECONFIG

**Create the following Variables at a Global level**
("Manage Jenkins -> Configure System -> Global Properties option -> Add")

- Variable: image_registry
Value: URL of the image registry

- Variable: k8s_cluster
Value: URL of the Kubernetes Cluster

**In your pipeline - Add the following string parameters**
- REPO_URL -> Application Repository URL
- BRANCH_NAME -> The default branch that must be deployed
- REGISTRY_URL -> The Image Registry URL where your containerized Image will be pushed
- BUILD_TYPE -> The Containerization method that you want to use to create your application Image (Default Values - docker/buildx/buildpack)
- EMAIL_RECIPIENTS -> Comma seperated email ids of the recipients. Leave the default value blank incase you don't want to use this feature.

**2) On GitLab or Github**
Follow the following structure in your application repository before implementing this pipeline script.
![new_1](https://user-images.githubusercontent.com/70566326/222963728-cdd0dd89-d930-4188-ad91-5002d7364836.PNG)

Make sure that you are maintaining your Kubernetes Deployment Manifests in the "deploy" folder.
Club all your manifests into one file named : deployment.yaml 

