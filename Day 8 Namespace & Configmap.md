<img width="282" height="389" alt="image" src="https://github.com/user-attachments/assets/52bc1ea8-faf4-4fa3-99f9-fe17d1322cfa" />Take ec2 server 

and install docker & kubectl & minikube

----------> Namespaces <-------------

<img width="719" height="468" alt="image" src="https://github.com/user-attachments/assets/cf401ead-eea1-4f68-af9d-f88b5d18c75c" />

3 name space with their resources 
Namespaces :
Namespaces in Kubernetes are used to logically separate and manage resources across different environments 
such as dev, test, and production.

we have some default namespaces in k8s
Kubectl get ns
1)default :
   Used when no namespace is specified.
   Normal user workloads run here by default.
2)kube-system
   Contains Kubernetes system components.
  Example:
     CoreDNS
     kube-proxy
     scheduler
     controller manager
3) kube-public
    Publicly readable namespace.
    Mostly used for cluster-wide public information.
4) kube-node-lease
    Stores node heartbeat information.
    Helps Kubernetes detect node availability quickly.
kubectl get po -n kube-public   -----> to get pods in particular namespaces
Namespaces : will create in two ways 1) imperative & 2) declarative
> over commnd <

 kubectl create ns dev ( namespace)

 over manifest file

 vim namespace.yml
 
apiVersion: v1
kind: Namespace
metadata:
  name: test

  Kubectl create -f namespace.yml
  
 --->   How to create resources in namespace  <----

 kubectl run mypod --image=nginx -n dev   --    will create in dev namespace
 kubectl run mypod --image=nginx   -- will create in default namspace
 Kubectl get po -n dev            -- to get pods in created namespace

 or Manifest 

 vim pod.yml
 
 apiVersion: v1
kind: Pod

metadata:
  name: pod-1
  namespace: test 

spec:
  containers:
    - name: cont-1
      image: nginx
      ports:
        - containerPort: 80
 kubectl create -f pod.yml

 kubectl get po -n test   to get list of nodes
 
   or
 kubectl create -f pod.yml -n test 

 ---- deployment ----
 vim deplotment.yml
 
apiVersion: apps/v1
kind: Deployment

metadata:
  name: nginx-deployment
  labels:
    app: nginx

spec:
  replicas: 1

  selector:
    matchLabels:
      app: nginx

  template:
    metadata:
      labels:
        app: nginx

    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80


Kubectl create -f deployment.yml


Kubectl create -f deployment.yml -n dev   --- to create same deployment file in particular namespace

kubectl config view | grep -i "namespace"    ---> to find present name space
kubectl config set-context --current --namespace=dev     --> to switch one namespace to another namespace

If we delete one name space all resources deleted whichever in namespace  we never do in real time


 ------------->  CONFIGMAPS  <-------------------

 
