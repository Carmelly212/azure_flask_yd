# Flask Application Deployment Guide

This guide outlines how to deploy a simple Flask application using Docker, Kubernetes, Helm, and Cert Manager, with the application serving a greeting message.

## Components

- **app.py**: Flask application script.
- **Dockerfile**: Instructions for building the Docker image.
- **flaskapp/values.yaml**: Helm chart values for the Flask application deployment.

## Building and Storing the Docker Image

1. **GitHub Actions** is used for automating the building and storage of the Docker image.

## Kubernetes Deployment

1. **Creating a Docker Registry Secret**:  
   Create a secret in Kubernetes to allow your deployment to pull the Docker image:
   ```shell
   kubectl create secret docker-registry myacr-secret \
   --docker-server=alon001.azurecr.io \
   --docker-username=alon001 \
   --docker-password=<PASSWORD> \
   --docker-email=alon212@gmail.com


## Deploying with Helm:
Deploy your application using Helm with the custom values specified in flaskapp/values.yaml. To set the ingress controller's externalTrafficPolicy to Local, include this setting in your Helm command:
**helm create flaskapp**
then:
**helm install alon015 . -f flaskapp/values.yaml --set controller.service.externalTrafficPolicy=Local**

## Certificate Management
- Applying Cert Manager
Install Cert Manager in your cluster to handle SSL/TLS certificates. This step is crucial for enabling HTTPS for your application.
**kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.9.1/cert-manager.yaml**

- Next, apply the issuer configuration. This will setup Cert Manager to issue certificates for your domain using Let's Encrypt.


**kubectl apply -f letsencrypt-staging-issuer.yaml**

## Why Not Use Port 5000?
Using port 80 for HTTP and port 443 for HTTPS is standard practice for web applications. These ports are automatically used by web browsers for web traffic, making your application easily accessible without needing to specify a port number in the URL. For Kubernetes deployments, the Ingress resource is used to manage external access to services within the cluster on these standard ports, simplifying the configuration and ensuring compatibility with the widest range of network environments.

## Final Notes
Ensure the DNS name alon.ydevops.io correctly points to your application. The ingress className and clusterIssuer should match your specific cluster setup. Adjust the values.yaml as necessary to fit your environment.

Remember to replace <PASSWORD> with the actual password for your Docker registry secret. It's important to keep this and any other sensitive information secure.