PROBES (HEALTHCHECK):

PROBES: used to determine the health and readiness of containers running within pods. Probes are 3 types:
Readiness probes are used to indicate when a container is ready to receive traffic. If the readiness probe fails, Kubernetes removes the pod’s IP from the Service’s endpoint list.
Liveness probes are used to determine whether a container is still running and responding to requests.
Startup Probe are used to determines whether the application within the container has started successfully. It's used to delay the liveness and readiness probes until the application is ready to handle traffic.

request → container


Install cluster 
----> create pod   
vim pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-1

spec:
  containers:
    - name: cont-1
      image: ubuntu
      command: ["/bin/bash", "-c", "touch /opt/myfile; sleep 1000;"]

      livenessProbe:
        exec:
          command:
            - cat
            - /opt/myfile
        initialDelaySeconds: 5
        periodSeconds: 5
        timeoutSeconds: 3

  kubectl create -f pod.yml

   kubectl exec -it pod-1 -- bash
    go to inside 
     ll /opt
     will notice created folder & we menioned in manifest file he willl be running till 1000 sec
     
it will check every 5 sec if its fail means recreates one more conatiner



  -----------------> INGRESS <--------------

  we have 
  1) path based routing 
  2) host based routing for evrry application  both suppoerted by ingress controller

What is Ingress?

Ingress is a Kubernetes object that manages external HTTP/HTTPS traffic and routes it to the appropriate Service inside the cluster.

Without Ingress:

Internet
    |
LoadBalancer Service
    |
Application

For 10 applications, you may need 10 LoadBalancers (expensive).

With Ingress:

Internet
    |
Ingress Controller (NGINX/ALB)
    |
+------------------+
| /app1 -> svc1    |
| /app2 -> svc2    |
| app1.com -> svc1 |
| app2.com -> svc2 |
+------------------+


Install ingress


kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.15.1/deploy/static/provider/aws/deploy.yaml

Clone git repo : git clone https://github.com/devops0014/kubernetes.git
cd kubernetes
cd ingress
will notice manifest files


kubectl apply -f .




  
