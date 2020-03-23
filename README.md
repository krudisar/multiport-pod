# multiport-pod - how to expose multiple ports from a single Kubernetes pod

Delete existing deployment
> kubectl delete -f deployment.yml && kubectl delete -f service.yml

Build a new container image, tag it and push back to image registry
> docker build -t krudisar/multiport-pod . 
> docker push krudisar/multiport-pod  

Create a new deployment along with corresponding service 
> kubectl apply -f deployment.yml && kubectl apply -f service.yml
> kubectl get service


