Deploying a Python Web Application on Kubernetes Cluster for Warner Sisters Inc- Capstone Project
Prerequisites
1.	Docker Desktop – We will use Docker to build and run Jenkins as a Docker Container.
2.	GitHub account – You will use GitHub as your Source Code Management (SCM) Git repository. You will push your Jenkinsfile, application, and deployment files to your GitHub repository.
3.	Kubernetes Cluster – A Kubernetes cluster is a collection of interconnected computing resources, often consisting of multiple physical or virtual machines, that 
In this tutorial, we will use the Minikube Kubernetes Cluster. It is the most popular local Kubernetes cluster. We will use Minikube since it’s free to use and easy to set up on your computer.
4.	Kubectl – It is the command line tool interface for Kubernetes. It allows DevOps practitioners to run commands and deploy Kubernetes objects to the Kubernetes cluster. 
5.	Docker Hub account – Jenkins CI/CD Pipeline will build the Docker image and push it to your Docker Hub repository. It will also pull the Docker image from the Docker Hub registry and create a containerized application.

1. Create Python Django Web Application:
      I have created Python Django application. Application can be found at below location.
      https://github.com/Ashvini379/Django-Docker-Jenkins-kubernetes.git

2. Containerize Python Django Web Application:
    Dockerfile is created and can be found at same location as source code.
    https://github.com/Ashvini379/Django-Docker-Jenkins-kubernetes.git
   The Dockerfile will contain commands that the Jenkins CI/CD pipeline will use to build the Docker image for the simple python Django application. In the project folder, create a Dockerfile and paste the following Docker snippet as metioned in file in source code.
3. Create EC2 instance for Jenkins.
    Setup Jenkins on Ec2 instance using Ubuntu and t3.medium
    First we have to install Docker on ec2 instance. 
a.	Execute below commands 
sudo apt update
sudo apt install docker-ce
Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:
sudo systemctl status docker
b.	Install and configure Jenkins on same EC2 instance using below command
 docker run -u 0 --privileged -v /var/run/docker.sock:/var/run/docker.sock --network minikube -p 8080:8080 jenkins/jenkins:lts
c.	Go to http://<Public IP Address>:8080 and configure Jenkins.
d.	Login using admin password 
e.	Customize Jenkins
 
f.	After all plugins installation. Install Docker Pipeline and Kubernetes plugin.
 
 
g.	Add credentials to Jenkins Credential Manager.
4. Create EC2 Instance for MInikube and Install Minikube.
5. Start Minikube using below command
minikube start
6. Add Minikube credentials to Jenkins. Use ./Kube/config file to add credentials.
7. Deploy Python Django Web Application on Minikube Kubernetes: 
   Create kubernetes deployment files for deployment and service. Both deployment and service files are located at 
https://github.com/Ashvini379/Django-Docker-Jenkins-kubernetes.git
8. Create Jenkinsfile.
The Jenkinsfile will have multiple stages for defining our CI/CD Jenkins Pipeline. In the jenkins-kubernetes-deployment folder, create a Jenkinsfile and paste the following Jenkins pipeline snippet. Remember to update the GitHub link with your username and project!
The Jenkinsfile will create a Jenkins Pipeline with four stages:
•	Checkout Source
•	Build image
•	Pushing Image
•	Deploying Django container to Kubernetes

•	Checkout Source Stage
This Jenkins Pipeline stage will use `https://github.com/Ashvini379/Django-Docker-Jenkins-kubernetes.git as the GitHub repository. It will pull and scan all the files in this GitHub repository.
•	Build Image Stage
This Jenkins Pipeline stage will use the created Dockerfile to build a Docker image named ashvini34/django-app.
•	Pushing Image Stage
This Jenkins Pipeline stage will push the bravinwasike/react-app Docker image to Docker Hub using the dockerhub-credentials
•	Deploying Django Container to Kubernetes Stage
It will pull ashvini34/django-app Docker image from the Docker Hub repository and create a containerized application. It will then deploy the React.js container to Kubernetes

9. Create Multibranch pipeline.
 
•	Scroll down and Select the Multibranch Pipeline. Give it a name (jenkins-kubernetes-deployment will be assumed), then click OK
 

10.  Configure the Multi-branch Pipeline
Configure branch sources.
 

Add repository URL and cgithub credentials
 
Click on scan repository now.
 
Build Multibranch pipeline
 
 
The Jenkins CI/CD pipeline outputs a SUCCESS message. The Jenkins CI/CD pipeline performed the following tasks:
•	Build the Docker image.
•	Push the Docker image to Docker Hub.
•	Pull the Docker image from the Docker Hub repository and create a Dockerized application.
•	Deploy the containerized application to the Kubernetes cluster. 
11. Monitoring and Logging
a. Setting Up Prometheus
First, create a namespace for Prometheus by running the following command:
kubectl create namespace Prometheus
Next, create a ConfigMap for Prometheus by running the following command:
kubectl create configmap prometheus-server-config --from-file=prometheus.yml
 After creating the ConfigMap, deploy Prometheus by running the following command:
kubectl apply -f https://raw.githubusercontent.com/prometheus-community/helm-charts/main/charts/kube-prometheus-stack/values.yaml -n prometheus

b. Setting up Grafana
First, create a namespace for #Grafana by running the following command:
kubectl create namespace grafana
Next, create a ConfigMap for #Grafana by running the following command:
kubectl create configmap mahiratechnology-grafana-config - from-file=grafana.ini
After creating the ConfigMap, deploy #Grafana by running the following command:
kubectl apply -f https://raw.githubusercontent.com/grafana/helm-charts/main/charts/grafana/values.yaml -n grafana
c. Accessing Prometheus and Grafana
Expose Prometheus by running the following command:
kubectl expose service prometheus-server -n prometheus --type=NodePort - target-port=9090 -name=prometheus-service
Expose #Grafana by running the following command:
kubectl expose service grafana -n grafana - type=NodePort - target-port=3000 - name=grafana-service

Get the NodePort for Prometheus and #Grafana by running the following commands:
kubectl get service prometheus-service -n prom


 











	


	
