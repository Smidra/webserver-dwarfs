Webserver Dwarfs
================
*Tired of seeing "hello nginx" when deploying test containers? Why not use dwarfs?*

What is this?
-------------
This repository defines four distinquished webpages with colored dwarfs and docker containers for deployment with nginx.

<img src="blue-dwarf/static/dwarf-blue-small.png" width="60" alt="Ori"><img src="red-dwarf/static/dwarf-red-small.png" width="60" alt="Kili"><img src="yellow-dwarf/static/dwarf-yellow-small.png" width="60" alt="Dvalin"><img src="green-dwarf/static/dwarf-green-small.png" width="60" alt="Bifur">

Why is this?
-------------
When deploying contianers, for example to Kubernetes, one often deploys a small webserver "just to see it works". This project provides a playful alternative to the default webpage that says "Welcome to nginx" or "It works!".

Docker hub images
-----------------
The docker images conatain a plain nginx image with the HTML files copied inside. Dockerfiles are present in each of the color-folders. Container images are public at Docker hub:
* [Blue dwarf container](https://hub.docker.com/r/smidra/blue-dwarf)
  * ``` smidra/blue-dwarf ```
* [Red dwarf container](https://hub.docker.com/r/smidra/red-dwarf)
  * ``` smidra/red-dwarf ```
* [Yellow dwarf container](https://hub.docker.com/r/smidra/yellow-dwarf)
  * ``` smidra/yellow-dwarf ```
* [Green dwarf container](https://hub.docker.com/r/smidra/green-dwarf)
  * ``` smidra/green-dwarf ```


Deploy with Docker
------------------
For deployment of the **blue dwarf** locally use:
``` bash
docker pull smidra/blue-dwarf
docker run -p 8099:80 --name blue-dwarf smidra/blue-dwarf
```
The dwarf shall be available at http://localhost:8099


Deploy with Kubernetes (imperative)
-----------------------------------
``` bash
# Deploy resources
kubectl create deployment blue-dwarf --image=smidra/blue-dwarf
kubectl expose deployment blue-dwarf --name=blue-dwarf-svc --type=NodePort --port=80
```
If deploying with minikube...
``` bash
# Watch for the assigned XYZ high port
kubectl get services
# Get minikube cluster ip
minikube ip
```

The dwarf shall be available at http://cluster-ip:XYZ


Deploy with Kubernetes (declarative)
------------------------------------
Add to cluster with ```kubectl apply -f ...```

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
      nodePort: 30073
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

The dwarf shall be available at http://cluster-ip:30073

Is it any good?
---------------
[Yes](https://news.ycombinator.com/item?id=3067434)

Credits
-------
[Garden Gnome Clip art by LA](https://sweetclipart.com/garden-gnome-clip-art/) is used under the tems of [CC BY-NC-SA 3.0](https://creativecommons.org/licenses/by-nc-sa/3.0/)

HTML index is based on the [bootstrap sign in example](https://getbootstrap.com/docs/4.0/examples/sign-in/)

This work is lincensed under the MIT licence, mainly because the Greater Lunduke Lincese is not a real thing.
