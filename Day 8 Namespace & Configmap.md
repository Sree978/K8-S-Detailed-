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

a ConfigMap is used to store non-sensitive configuration data as key-value pair format.
It helps separate configuration from the application container image.
Why ConfigMaps are used : 
Instead of hardcoding values inside your application or Docker image, you can store them externally.
Example:
   Database URL
      Application mode
      Port numbers
      Environment variables
      Configuration files
This makes applications:
      portable
      reusable
      easier to update
<img width="929" height="466" alt="image" src="https://github.com/user-attachments/assets/55eb4895-3348-42ad-9cb4-8a89aba01638" />

vim configmap.yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cm1
data:
  PORT: "3306"
  DB_NAME: "flmdb"
  COURSE: "DevOps"
  Cloud: "AWS"


Kubectl create -f congifmap.yml

will notice created configmap

kubectl get cm
kubectl describe cm cm1 (config name )
will notice all data
    imperative way 
kubectl create cm cm2 --from-literal=Name=Mustafa --from-literal=Company=FLM --from-literal=Place=Hyd     to add environ ments 
 or 

 vim app.env 

 app=uxapps
 company=tcs
 porject=banking
 
 kubectl create cm cm3 --from-env-file=app.env

 kubectl describe cm cm3

 will notice all data which we added


real time wil use mostly declarative & env file

we created config file to attach for pod
   Possible Ways to Attach a ConfigMap to a Pod
      We can attach a complete ConfigMap to a Pod.
      We can attach multiple ConfigMaps to a Pod.
      We can attach specific key/value pairs from a ConfigMap to a Pod.
      We can attach different values from different ConfigMaps to a Pod
--> config  map to add ( multiple config or single configmap)
write a manifest file for pod 
vim pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: flm-pod

spec:
  containers:
    - name: cont-1
      image: nginx
      ports:
        - containerPort: 80
      envFrom:
        - configMapRef:
            name: cm1
        - configMapRef:
            name: cm2

kubectl create -f pod.yml
kubectl get po
kubectl exec -it flm-pod -- bash
printenv

To add different specific value from deffirent configmap

vim pod.yml

apiVersion: v1
kind: Pod

metadata:
  name: flm-pod

spec:
  containers:
    - name: cont-1
      image: nginx

      ports:
        - containerPort: 80

      env:
        - name: mycloud
          valueFrom:
            configMapKeyRef:
              name: cm1
              key: Cloud

        - name: myport
          valueFrom:
            configMapKeyRef:
              name: cm1
              key: PORT

        - name: myarea
          valueFrom:
            configMapKeyRef:
              name: cm2
              key: Place

kubectl create -f pod.yml

kubectl exec -it flm-pod1 -- bash
printenv 

will notice different values from different Configmaps

![Uploading image.png…]()




 
 





 

 
