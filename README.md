# multiport-pod - how to expose multiple ports from a single Kubernetes pod

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
