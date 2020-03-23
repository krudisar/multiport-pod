# multiport-pod - how to expose multiple ports (hhtp/80 & ssh/20) from a single Kubernetes pod

Delete existing deployment
```
kubectl delete -f deployment.yml && kubectl delete -f service.yml
```

Build a new container image, tag it and push back to image registry
```
docker build -t krudisar/multiport-pod . 
docker push krudisar/multiport-pod  
```

Create a new deployment along with corresponding service 
```
kubectl apply -f deployment.yml && kubectl apply -f service.yml
kubectl get service
```
Dockerfile:
```
FROM ubuntu:16.04
RUN apt-get update && apt-get install -y openssh-server apache2 supervisor net-tools
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd /var/log/supervisor

RUN echo 'root:Password1234' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord"]
```
deployment.yml:
```
      ...
      ports:
        - containerPort: 80
          protocol: TCP
        - containerPort: 22
          protocol: TCP
      ...
```

service.yml:
```
...
  ports:
  - nodePort:
    name: www 
    protocol: TCP
    port: 80
    targetPort: 80
  - nodePort:
    name: sshd 
    protocol: TCP
    port: 22
    targetPort: 22
...
```
