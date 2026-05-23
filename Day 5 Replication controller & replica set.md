---------------->  REPLICAS IN KUBERNETES   <---------------
Before Kubernetes, other tools did not provide important and customized features like scaling and replication.
When Kubernetes was introduced, replication and scaling were the premium features that increased the popularity of this container orchestration tool.
Replication means that if the pod's desired state is set to 3 and whenever any pod fails, then with the help of replication, the new pod will be created as soon as possible. This will lead to a reduction in the downtime of the application.
Scaling means if the load becomes increases on the application, then Kubernetes increases the number of pods according to the load on the application.

----> scalling <------

To increase or decrease pods depends on user request will call it scalling
two type:
1) manuval
2) automatic


--> k8's will provide these two features scalling & replication

------->  REPLICATION CONTROLLE   <------------
Replication controller can run specific number of pods as per our requirement.
It is responsible for managing the pod lifecycle.
It will make sure that pods are always up and running.
If there are too many pods, RC will terminate the extra pods.
If there are too few pods, RC will create new pods.
This Replication Controller has self-healing capability, which means it automatically creates new pods when required.
If a pod is failed, terminated, or deleted, then a new pod will get created automatically.
Replication Controllers use labels to identify the pods that they are managing.
We need to specify the desired number of replicas in the YAML file.

Create a cluster ( refer day 3 file for create luster steps)

post that write manifest file for replication controller 

RC - PODS -CONTAINER 
RC specification are pods specifications pods specifications are container specifications
<img width="515" height="269" alt="image" src="https://github.com/user-attachments/assets/cb481c12-d8ab-47b3-a211-54907538cbb0" />

Vim replica.yml

cmd : kubectl create -f replica.yml
will notice created replica
kubctl get rc   - to see list of rc's
kubectl describe rc rcname -- to get full deatils
     pod naming convention  rcnae-randomid
kubectl get po --show-label

expose based on labels  wrote a service file

vim svc.yml
-----------

---
apiVersion: v1
kind: Service

metadata:
  name: mysvc

spec:
  type: LoadBalancer

  selector:
    app: swiggy

  ports:
    - port: 80
      targetPort: 80

      -----------------------

kubectl create -f svc.yml

kubectl get svc  to csee list of services 

will notice DNS server

check in browser make sure  it should be http 

scalling  do it manuvaly 

go to manifest file and increase replica count 
kubectl apply -f replica.yml

decrease replica count 
kubectl apply -f replica.yml

Kubectl crete & apply
create ---->    while creating object firsttime
apply ---- >  to change existed object 

cmd : kubectl delete pod podname    to delete it 

if deleted also by automatic pod creeated   even for all pods also applicable 

------->  scalling over commnd
kubectl scale rc rcname --replica=4
kubectl get pods
will notice 4 

same command for scale down for decrease count

kubectl delete rc rcname   to delete perminently 

client expexting  new changes in application

1) start from developers
2) will perform static code analysis for code & build & test , create image with docker & scan -> will push image to docker hub( Jenkins  will use it for automate this process )

   will go to manifest file edit image version

   cmd: kubectl apply -f rc rcname  we never see changes its sould be previous
   

    we can't update image directly in Replication controller

   we have to delete RC and update new  
    kubectl delete rc rcname
     & 
   will go to manifest file edit image version

   cmd: kubectl create -f rc rcname

   Draw backs in RC
   to delete & create new one there is downtime
   there is no autu scalling
   no rollback
   can't create image directly

   Will use Replica set (RS)

   RC - will work on equality based selector  (old version of k8's)
   RS - will work set based selectors    - (  new version of k8's)



   1  hostnamectl hostname repliac
    2  exit
    3  clear
    4  export TMOUT0
    5  curl -Lo kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    6  chmod +x kops
    7  sudo mv kops /usr/local/bin/kops
    8  kops version
    9  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   10  chmod +x kubectl
   11  mv kubectl /usr/local/bin/
   12  kubectl version
   13  clear
   14  aws s3 ls
   15  export KOPS_STATE_STORE=S3://kops-bucket-day4
   16  kops create cluster --name flm.k8s.local --zones=us-east-1a,us-east-1b --control-plane-count=1 --control-plane-size=c7i-flex.large --control-plane-volume-size=30 --node-count=2 --node-size=t3.micro --node-volume-size=20 --image=ami-0236922087fa98b6e
    21  kops update cluster --name flm.k8s.local --yes --admin
   22  kops get cluster
   26  kops delete cluster --name flm.k8s.local --yes
   vim rc.yml
apiVersion: v1
kind: ReplicationController

metadata:
  name: flm

spec:
  replicas: 3

  selector:
    app: swiggy

  template:

    metadata:
      labels:
        app: swiggy

    spec:
      containers:
        - name: cont-1
          image: shaikmustafa/dm
          ports:
            - containerPort: 80

       
   33  kubectl create -f rc.yml
   will notice three pods which we created 
   
   35  kubectl get rc
   36  kubectl get po
   39  kubectl rm -f rc.yml
   40  kubectl delete rc flm
   41  kubectl create -f rc.yml
   42  kubectl get rc
   43  kubectl describe rc flm
   44  history

--> write a service file
vim svc.yml

apiVersion: v1
kind: Service
metadata:
  name: mysvc
spec:
  type: LoadBalancer
  selector:
    app: swiggy
  ports:
    - port: 80
      targetPort: 80

      Kubectl create -f svc.yml

      will notice service creates

      kubectl get svc

      will get DNS:   abaf8e045403040518790d80a1fbe79d-71458234.us-east-1.elb.amazonaws.com

      its take min couple of min to browse
      
     
http:// url 
will able to browse

Pod scalling  RC by using manifestfile

 vim rc.yml
  
   50  vi rc.yml           ---- scale up replica count        
   51  kubectl apply -f rc.yml
   52  kubectl get pods     -- will notice increadsed replicas
   53  vi rc.yml            --- scale down replica couunt
   54  kubectl apply -f rc.yml
   55  kubectl get pods     --- > will notice decreased replicas

   0r   CMD 
   kubectl scale rc flm  --replicas=4      ( replica count we can inxreas or secrease )

   Drawback in repplica controller  RC & RS ( replica set)
   
   
  1) no auto scalling here  &  2) downtime exsited & 3) we can't update image directly  4) ro rollback

   

to over downtime   ( delete old rc and create new RC adding image ) for this while creating new there is downtime
 wont use real time

-->  Replica  set   RS  ( new version obejct  in nk8's)



cp rc.yml rs.yml

vim rs.yml
apiVersion: apps/v1
kind: ReplicaSet

metadata:
  name: flm

spec:
  replicas: 2
  selector:
    matchLabels:
     app: swiggy

  template:

    metadata:
      labels:
        app: swiggy

    spec:
      containers:
        - name: cont-1
          image: shaikmustafa/paytm:bus
          ports:
            - containerPort: 80
cmd: kubectl create -f rs.yml


# RS

# to get list of rs :
kubectl get rs

# to get full info about rs :
kubectl describe rs rs-name

# to delete an rs :
kubectl delete rs rs-name

# scale :
kubectl scale rs rs-name --replicas=count



   

   
   
   



  

   














      



 

     





