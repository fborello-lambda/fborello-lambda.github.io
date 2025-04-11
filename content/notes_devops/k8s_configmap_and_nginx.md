+++
title = "K8S ConfigMap and NGINX"
date = 2024-07-30

[taxonomies]
tags=["devops"]
+++

# ConfigMap as a "Volume"

We can use a `ConfigMap` to declare a file and then mount it as a volume to any container:

The ConfigMap is specified in the `volumes` section, the name of the volume has to match the `volumeMounts` name.

Two things to take into account, the `mountPath` could have been any path, the `subPath` must match the `ConfigMap`&rarr;`data`&rarr;`key/file`(index.html, in this case) defined.

The `Service` can be used to attach to an existing `Ingress` or any networking resource.

```yaml
  volumeMounts:
  - name: html-volume
    mountPath: /usr/share/nginx/html/index.html
    subPath: index.html
  ports:
  - containerPort: 80
volumes:
- name: html-volume
  configMap:
    name: html
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: html
data:
  index.html: |
    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Hello World</title>
    </head>
    <body>
      <h2>Hello</h2>
    </body>
    </html>
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: simple-web-server
spec:
  selector:
    matchLabels:
      app: simple-web-server
  template:
    metadata:
      labels:
        app: simple-web-server

    spec:
      containers:
      - name: simple-web-server
        image: nginx:latest
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        volumeMounts:
        - name: html-volume
          mountPath: /usr/share/nginx/html/index.html
          subPath: index.html
        ports:
        - containerPort: 80
      volumes:
      - name: html-volume
        configMap:
          name: html
---
apiVersion: v1
kind: Service
metadata:
  name: simple-web-server-service
spec:
  type: NodePort
  selector:
    app: simple-web-server
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
```
