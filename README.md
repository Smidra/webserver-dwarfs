Webserver Dwarfs
================
Tired of seeing "hello nginx" when deploying test containers? Why not use dwarfs?

What is this?
-------------
This repository defines four distinquished webpages with colored dwarfs and dockerfiles for deployment with nginx.

<img src="blue-dwarf/static/dwarf-blue-small.png" width="60" alt="Ori"><img src="red-dwarf/static/dwarf-red-small.png" width="60" alt="Kili"><img src="yellow-dwarf/static/dwarf-yellow-small.png" width="60" alt="Dvalin"><img src="green-dwarf/static/dwarf-green-small.png" width="60" alt="Bifur">

Why is this?
-------------
When deploying contianers, for example to Kubernetes, one often deploys a small webserver "just to see it works". This project provides a playful alternative to the default webpage that says "Welcome to nginx" or "It works!".

Deploy with docker
------------------
TBA

Deploy with Kubernetes
----------------------
Blue dwarf service:
``` yaml
apiVersion: v1
kind: Service
metadata:
  name: dwarf-svc
spec:
  selector:
    app: webserver-dwarf
  type: NodePort
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30010
```

Blue dwarf deployment:
``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue-dwarf
  labels:
    app: webserver-dwarf
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webserver-dwarf
  template:
    metadata:
      labels:
        app: webserver-dwarf
    spec:
      containers:
      - name: blue-dwarf
        image: smidra/blue-dwarf
        ports:
        - containerPort: 80
      terminationGracePeriodSeconds: 4
```


Is it any good?
---------------
[Yes](https://news.ycombinator.com/item?id=3067434)

