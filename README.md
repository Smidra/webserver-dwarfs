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

Deploy with Docker
------------------
For deployment locally use:
``` bash
docker pull smidra/blue-dwarf
docker run -p 8099:80 --name blue-dwarf smidra/blue-dwarf
```
The dwarf shall be available at http://localhost:8099

Dockerfiles are present in each of the color-folders.
Conatiner images are public at Docker hub:
* [Blue](https://hub.docker.com/repository/docker/smidra/blue-dwarf)
* [Red](https://hub.docker.com/repository/docker/smidra/red-dwarf)
* [Yellow](https://hub.docker.com/repository/docker/smidra/yellow-dwarf)
* [Green](https://hub.docker.com/repository/docker/smidra/green-dwarf)


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

The dwarf shall be available at http://cluster-ip:30010


Is it any good?
---------------
[Yes](https://news.ycombinator.com/item?id=3067434)

