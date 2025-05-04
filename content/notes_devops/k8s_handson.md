+++
title = "K8S HandsOn"
date = 2024-06-25

[taxonomies]
tags=["devops"]
+++

## Kubernetes

To test Kubernetes locally, we need a `Kubernetes Cluster`. We can create a cluster using `Docker`. Docker Desktop provides a quick way to run a Kubernetes cluster. However, `Minikube` offers detailed tutorials and thorough documentation.

So, the necessary tools are:

- `docker` &rarr; runs the `Minikube` container.
- `minikube` &rarr; helps create the Kubernetes cluster (Docker Desktop can also create a Kubernetes context/cluster).
- `kubectl` &rarr; CLI used to `orchestrate` our `pods`.

## Pods, Nodes, and The Control Plane

The Control Plane is the main `node` responsible for supervising our `nodes`. Nodes, also known as kubelets, are VMs or physical computers with the Docker engine running. Each Node hosts Pods, where a single pod represents an instance of any given app.

In summary, our cluster is managed by the `Control Plane`. We can have multiple `Nodes` (docker engines) where apps can be deployed, and we will have $N$ `Pods` depending on the number of instances we decide to deploy.

The magic behind Kubernetes lies in its pods; a single pod represents an application, and each application can consist of multiple Docker containers. In simple terms, multiple pods can handle many containers. If one container or even the entire pod fails, Kubernetes will manage this situation by reloading the containers and/or pods.

## Hands On

If using `Minikube`, we start our `Minikube` context:

```sh
minikube start
```

`kubectl` works as follows: `kubectl <action> <resource>`

|     Action     |                                           Description                                            |
| :------------: | :----------------------------------------------------------------------------------------------: |
|     `get`      |                        Retrieves one or more resources from the cluster.                         |
|   `describe`   |          Provides detailed information about a specific resource or group of resources.          |
|    `apply`     |             Applies configuration changes to resources by creating or updating them.             |
|    `delete`    |                         Deletes one or more resources from the cluster.                          |
|     `logs`     |                        Retrieves the logs of a specific pod or container.                        |
|     `exec`     |                            Executes a command inside a specific pod.                             |
| `port-forward` |                   Sets up a port forward from a local port to a port on a pod.                   |
|    `scale`     |      Scales the number of replicas of a deployment, replication controller, or replica set.      |
|    `label`     |                               Adds or updates labels on resources.                               |
|   `annotate`   |                            Adds or updates annotations on resources.                             |
|     `edit`     | Edits a resource in the default editor or the editor specified by the KUBE_EDITOR or EDITOR env. |

To ask for our context:

```sh
kubectl config current-context
```

To get our nodes:

```sh
kubectl get nodes
```

Output:

```sh
❯ kubectl get nodes
NAME             STATUS   ROLES           AGE   VERSION
docker-desktop   Ready    control-plane   22m   v1.29.2
```

Here the `docker-desktop` node acts as the `control-plane`.

The easiest way to have an app managed by the `control-plane` is by deploying a docker-image:

```sh
kubectl create deployment first-deploy --image=<img url>
```

We can also deploy more neatly by using a file:

`k8s_deployment`:

```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world
spec:
  replicas: 3
  selector:
    matchLabels:
      app: hello-app
  template:
    metadata:
      labels:
        app: hello-app
    spec:
      containers:
        - name: hello-app
          image: gcr.io/google-samples/hello-app:1.0
          imagePullPolicy: IfNotPresent
          ports:
            - containerPort: 8080
```

To deploy with this file:

```sh
kubectl apply -f k8s_deployment.yml
```

Now if we take a look at the deployments:

```sh
❯ kubectl get deployments
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
hello-world     3/3     3            3           3m16s
```

We can observe our deployment with 3/3 pods ready. If we change the number of `replicas` and then we run the command, the kubernetes `control-plane` will automatically adjust the number of pods.

To get the pods with extra info:

```sh
kubectl get pods -o wide
```

If, by any chance, a pod stops, Kubernetes will automatically restart it, even if the pod is deleted:

```sh
kubectl delete pod <pod's NAME>
```

A resize can be performed as follows:

```sh
kubectl scale deployment.apps/hello-world --replicas=4
```

The pods are attached to the deployment, which is always supervised.

To delete the deployment:

```sh
kubectl delete deployment <deployment's NAME>
```

We can delete everything that a file has created with the following command:

```sh
kubectl delete -f <file>
```

## Networking & Load Balancing

The above deployment file contains 3 pods with `containerPort` set to 8080. Since we can resize our pods, the IPs are dynamic. It's preferable to have a method that preserves access to any number of pods.

Before demonstrating the preferred method, let's explore `kubectl proxy`.

Ensure the deployment is running and open a new terminal:

```sh
kubectl proxy
```

Output: `Starting to serve on 127.0.0.1:8001`

Now, if we ask for the name of the pods:

```sh
kubectl get pods
```

Output:

```sh
❯ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-world-d4c945676-fzpwc   1/1     Running   0          22m
hello-world-d4c945676-sz66l   1/1     Running   0          22m
hello-world-d4c945676-xzd7m   1/1     Running   0          22m
```

We can access to each pod individually with the following URL:

```
http://localhost:8001/api/v1/namespaces/default/pods/<POD_NAME>:8080/proxy/
```

### Services

From the Docs:

> A Service in Kubernetes is an abstraction which defines a logical set of Pods and a policy by which to access them.

> A service routes traffic across the defined pods.

Table of services:

|     Type     |                                                                     Description                                                                     |
| :----------: | :-------------------------------------------------------------------------------------------------------------------------------------------------: |
|  ClusterIP   |                                                Exposes the Service on an internal IP in the cluster.                                                |
|   NodePort   | Exposes the Service on each Node’s IP at a static port (NodePort). Makes a Service accessible from outside the cluster using `<NodeIP>:<NodePort>`. |
| LoadBalancer |                                       Exposes the Service externally using a cloud provider's load balancer.                                        |
| ExternalName |                    Maps the Service to the contents of the externalName field, returning a CNAME record. No proxying is set up.                     |

A service can be created with the `kubectl expose <deployment> --type=<type> --port=<port>`. However, it can also be defined in a `.yml` file.

For example, if we want to use a `LoadBalancer` service with our existing deployment file, add the following configuration at the end and add the role label to the deployment:

```sh
      labels:
        app: hello-app
        role: hello-role <-- add the role label to the deployment
---
apiVersion: v1
kind: Service
metadata:
  name: hello-service
spec:
  type: LoadBalancer
  selector:
    role: hello-role
  ports:
    - port: 8080
      targetPort: 8080
```

Now if we apply the file:

```sh
kubectl apply -f k8s_deployment.yml
```

And then we do a curl, we should have access to the LoadBalancer:

```sh
❯ curl http://127.0.0.1:8080
Hello, world!
Version: 1.0.0
Hostname: hello-world-5486f9dfc5-6bkhr
```

> [!NOTE]
> If using minikube, the `service` has to be exposed `minikube service hello-service --url`

The `LoadBalancer` service handles the case where it doesn't matter which pod you are accessing and it even takes into account the network traffic.

## Ingress

Ingress is an Nginx server that simplifies the creation of paths and can distribute traffic load.

What happens if there are two nodes running multiple pods, each hosting different APIs, and the goal is to manage all traffic through a single URL?

For example, if we want to have 2 versions of a given URL and another for developing we cmay want the following scheme:

- `url/v1`
- `url/v2`
- `url/dev`

In such case, we will have a deployment for each app, which may have multiple replicas as pods, then a service to manage the pods' networking and finally the Ingress controller.

### Helm

> The package manager for Kubernetes
>
> Helm is the best way to find, share, and use software built for Kubernetes

With `helm`, the Ingress controller can be installed with the following command:

```sh
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

---

Now, paste the definition of the Ingress controller inside the deployment file:

```yml
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-ing
  labels:
    name: hello-ing
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: "nginx"
  rules:
    - host: localhost
      http:
        paths:
          - path: /v1
            pathType: Prefix
            backend:
              service:
                name: hello-service
                port:
                  number: 8080
```

Apply the changes:

```sh
kubectl apply -f k8s_deployment.yml
```

> [!NOTE]
> The Ingress Node is in another namespace: [Namespaces | Kubernetes](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

Finally, the curl command returns the expected output:

```sh
❯ curl localhost/v1
Hello, world!
Version: 1.0.0
Hostname: hello-world-5486f9dfc5-lqclc
```

# Resources

- [Learn Kubernetes Basics | Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [minikube start | minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Fx86-64%2Fstable%2Fbinary+download)
- [Kubernetes Explained in 15 Minutes | Hands On (2024 Edition) - YouTube](https://www.youtube.com/watch?v=r2zuL9MW6wc)
- [Kubernetes 101: Deploying Your First Application! - YouTube](https://www.youtube.com/watch?v=XltFOyGanYE)
- [KUBERNETES De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL) - YouTube](https://www.youtube.com/watch?v=DCoBcpOA7W4)

## Ingress

- [Installation Guide - Ingress-Nginx Controller](https://kubernetes.github.io/ingress-nginx/deploy/#quick-start)
- [Ingress | Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource)
- [Ingress Made Easy! Install and Configure the Ingress NGINX Controller for Kubernetes - YouTube](https://www.youtube.com/watch?v=g9EqZiTWLGA)
- [Kubernetes Ingress Tutorial: Day 32 of 100DaysOfKubernetes - YouTube](https://www.youtube.com/watch?v=13y1tWEK2ZY)
