

kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.0/deploy/static/provider/cloud/deploy.yaml

kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.yaml

kubectl apply -f letsencrypt-staging-issuer.yaml


Web applications typically use port 80 for HTTP and port 443 for HTTPS. These are the default ports that web browsers use to access websites. If a web application uses a non-standard port like 5000, users would have to specify the port number in the URL (like http://example.com:5000), which is not a common practice and could lead to confusion or errors.

In Kubernetes, the Ingress resource is used to manage external access to services in a cluster, typically HTTP and HTTPS requests. By default, an Ingress controller listens on ports 80 and 443. While it's possible to configure an Ingress controller to listen on other ports, this is not a common practice and could lead to compatibility issues with some Ingress controllers.

If we use a NodePort service to expose the application on port 5000, Kubernetes will automatically allocate a port from a default range (30000-32767) for external access. This port is not easy to predict or control, and it's not in the range of ports typically used for web traffic.

Using a LoadBalancer service could allow us to expose the application on a specific port, but this type of service is typically provided by cloud providers and may not be available in all environments.

In summary, while it's technically possible to expose a web application on port 5000 in Kubernetes, it's not a common practice and could lead to user confusion and compatibility issues. It's generally recommended to use port 80 for HTTP traffic and use an Ingress resource to manage external access to the application.