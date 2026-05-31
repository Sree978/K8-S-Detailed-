What is a Network Policy?
    Network Policies in Kubernetes are like firewall rules for pods. They control which pods can communicate with each other and 
with other network endpoints (e.g., external services).
These policies are implemented at the network level by Kubernetes-compatible Container Network Interface (CNI) plugins, 
such as:
    Calico
    Cilium
    Weave Net
Order of Evaluation
    1) If a pod has no policy, all traffic is allowed.
    2))If a pod has a policy, then only the required traffic is allowed.
    3) If a pod has one or more policies, only allowed traffic can pass.

Some Technical Terms to Remember:
    INGRESS RULE: Who can come into the pod
    EGRESS RULE: Where the pod can go out
    POD SELECTOR: Target pods to which the policy applies
    NAMESPACE SELECTOR: Limit by namespace
<img width="1142" height="631" alt="image" src="https://github.com/user-attachments/assets/c5656dc3-a64c-4419-8e70-40e50cfc6926" />


<img width="1114" height="634" alt="image" src="https://github.com/user-attachments/assets/c7889638-981f-464f-b0db-a8d3cbf8c38d" />


Process

1) Install docker & minikube
2) kubectl

 note we wont run minikube start here 

will go through 

--> minikube start --network-plugin=cni --cni=calico --force

kubectl get pods -l k8s-app=calico-node -A      --> for check calico running or not
kubectl run pod-1 --image=nginx             ----> create a pod
kubectl run pod-2 --image=nginx      
kubectl get po -o wide                           ---> to get pod ip 

kubectl exec -it pod-2 -- bash    ---> to enter pod-2
     install ping 
         apt update -y
         apt install iputils-ping -y
ping 10.244.120.67   - will notice our ip pinging so its communicationg with each pod

kubectl exec -it pod-1 -- bash    ---> to enter pod-2
     install ping 
         apt update -y
         apt install iputils-ping -y
ping 10.244.120.68   - will notice our ip pinging so its communicationg with each pod


now we dont want communicate each  so will use network policy 


vim deny-all.yml
  apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
  name: deny-all

spec:
  podSelector: {}
  policyTypes:
    - Ingress
    - Egress

kubectl create -f deny-all.yml

will notice created network policy 

kubectl get netpol     --> to get list of network policies

check go to one pod and check other pod is pinging or not 
 will notice it won't ping 

 here sopped communicating each pod

 if we create pod in future its also won't communicate
 

kubectl delete netpol --all      --> to delete all network policies 
once will delete network policy will notice all network policies communicating each other


To allow & deny specific pod on specific namespace

Create pods 

vim pod.yml
apiVersion: v1
kind: Pod

metadata:
  name: swiggy

spec:
  containers:
    - name: cont-1
      image: nginx


Kubectl create -f pod.yml
kubectl create ns dev     --> create namespace   ---> to create namespace
kubectl label pod swiggy "app=swiggy"       ---> to create label for pod
kubectl get po  --show-labels            --> to get pods along wit label details

kubectl label ns dev "env=dev"     --> to create label for ns

create another pod 

apiVersion: v1
kind: Pod

metadata:
  name: zomato
  namespace: dev
  labels:
    env: zomato

spec:
  containers:
    - name: cont-1
      image: nginx

kubectl create -f pod.yml

will notice one more craeted pod


will notice two pods in two different namespaces
kubectl label ns default "env=default"      --> to change label for defauly namespace

create N/w Policy  for incoming request

vim ingress.yml

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
  name: allow-nginx

spec:
  podSelector:
    matchLabels:
      app: zomato

  policyTypes:
    - Ingress

  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              env: default
          podSelector:
            matchLabels:
              app: swiggy
kubectl create -f ingress.yml

  & egress
  Vim egress.yml
  
  apiVersion: networking.k8s.io/v1
kind: NetworkPolicy

metadata:
  name: client-egress

spec:
  podSelector:
    matchLabels:
      app: swiggy

  policyTypes:
    - Egress

  egress:
    - to:
        - namespaceSelector:
            matchLabels:
              env: dev
          podSelector:
            matchLabels:
              app: zomato

      ports:
        - port: 80
        
 kubectl create -f egress.yml
 
kubectl get po -0 wide
get ip here
kubectl exec -it -n dev zomato -- bash    or direct command ( kubectl exec -it zomato -- apt install iputils-ping -y)
install ping 

and ping the ip adress will notice its pinging





