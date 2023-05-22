# hello-helm
Deployment Using Helm and K8s of a Simple React application that displays a “Hello World” message.


# Set Up the Environment


## Install Helm
https://helm.sh/docs/intro/install/


For Linux use the `./get_helm.sh` command to run the installation script:
- List all the files in /code.
- Run the Bash file to install Helm.
- Use the Helm help command to verify the installation.

# Set up a Cluster
To run Kubernetes, you need a cluster. A cluster is a group of computers working together that can be viewed as a single system.

For this excercise we do not need a complete cluster. Instead, we can use any one of the following:

- minikube
- kind
- Docker Desktop
- kubeadm

Install kind and create a cluster
```
kind create cluster
```

# Helm Chart

You can create the chart using the following command:

```
helm create your-chart-name
```

To deploy your own application, configure the Helm chart to use your own Docker image.

- Use the docker images command to get your image’s name and tag.
- The resultant fields in the values.yaml file would be the following:
```
repository: your-image-name
```
```
tag: your-image-tag
```

# Configure Kubernetes Service 

In Kubernetes, a Service provides an abstraction that defines access to a Pod. It sits in front of the Pod and delivers requests to the Pods behind it. 

Addt the service type and port in the `your-chart-name/values.yaml`

- Change the Service type to `NodePort`.
- Configure the chart to use the `31111` port.

```
service:
  type: NodePort
  port: 31111
```


# Verify the Values
Check if the application is configured correctly, meaning that it is using the correct Service, running on the desired port, and is the correct image name.

```
helm install --dry-run --debug your-chart-name your-chart-name
```

# Configure the Deployment

Every application has unique functionality and may run on the same or different ports depending on the application type. In a deployment.yaml file, a containerPort is the port on which your application is accessible inside the container.

Change the containerPort in the deployment.yaml file.

```
containerPort: 3000
```

# Deploy the Chart


```
helm install your-app-name your-chart-name
```

```
kubectl get pod
```

# Access the Application

Access deployed application over the internet. To do that, map your Service port to `31111`

- Use the kubectl get service command to check the status of your Service.
- Use the following command to map your Service to your localhost

```
kubectl port-forward svc/your-service-name --address 0.0.0.0 31111:31111
```

