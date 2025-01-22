# FairTrade Pipeline with Tekton
This project demonstrates a Tekton-based CI/CD pipeline that deploys two applications: a Django app and a Flask ML model. The pipeline uses Tekton tasks to clone a Git repository and deploy the applications to a Kubernetes cluster. The applications are exposed through Kubernetes services.

## Prerequisites
1. Kubernetes cluster
2. Tekton installed
3. kubectl configured to access cluster

## Pipeline Overview

The pipeline consists of the following stages:

1. Clone the repository: Clones the source code from the GitHub repository to the workspace.
2. Deploy Flask ML Application: Deploys the Flask-based machine learning app to the cluster.
3. Deploy Django Application: Deploys the Django-based web app to the cluster.
4. Tekton Tasks and Pipeline
5. Clone Task

### Clone Repo Task
This task checks if the workspace is empty and clones the FairTradeWithTekton repository from GitHub.

### Deploy Django Task
This task deploys the Django app using the Kubernetes deployment YAML.

### Deploy Flask-ML Task
This task deploys the Flask ML application using the Kubernetes deployment YAML.

### Pipeline Definition
The pipeline defines the sequence of tasks to clone the repository, deploy the Flask-ML app, and then deploy the Django app.

### PipelineRun
The PipelineRun is responsible for executing the pipeline. It uses a Persistent Volume Claim (PVC) to share data between tasks.

## Steps to Run

### Create the Persistent Volume Claim (PVC):

kubectl apply -f pvc.yaml

### Apply the Clone, Django, and Flask tasks:

kubectl apply -f clone-task.yaml
kubectl apply -f deploy-django-task.yaml
kubectl apply -f deploy-flask-task.yaml

### Deploy the pipeline:

kubectl apply -f fairtrade-pipeline.yaml

### Run the pipeline:

kubectl apply -f fairtrade-pipelinerun.yaml

### Forward the ports to access the applications:

Django: kubectl port-forward service/django 8000:8000
Flask ML: kubectl port-forward service/flask-ml 5000:5000

## Kubernetes Resources
1. Django Deployment: Deploys the Django app on port 8000.
2. Flask-ML Deployment: Deploys the Flask-ML app on port 5000.
3. Persistent Volume Claim: A 1Gi volume shared between Tekton tasks.