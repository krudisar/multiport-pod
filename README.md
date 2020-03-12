# multiport-pod

 kubectl delete -f deployment.yml && kubectl delete -f service.yml
 docker build -t krudisar/multiport-pod . 
 docker push krudisar/multiport-pod  
 kubectl apply -f deployment.yml && kubectl apply -f service.yml
 kubectl get service
