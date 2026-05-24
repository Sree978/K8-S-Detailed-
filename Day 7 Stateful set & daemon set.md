<img width="631" height="91" alt="image" src="https://github.com/user-attachments/assets/1de29e57-ef7c-4372-9cf6-93b794b62fc4" />
Stateful set : to deploy Database
to create database pods

we have two applciation s

we wont use stateless for Db

• It can't store the data permanently.
• The word STATELESS means no past data.
• It depends on non-persistent data means data is removed when Pod, Node or Cluster is stopped.
• Non-persistent mainly used for logging info (ex: system log, container log etc..)
• In order to avoid this problem, we are using stateful application.
• A stateless application can be deployed as a set of identical replicas, and each replica can handle incoming requests independently without the need to coordinate with other replicas.

---> will use deployment component

--> Stateful applications are applications that store data and keep tracking it.
Example of stateful applications:
• All RDS databases (MySQL, SQL)
• Elastic Search, Kafka, MongoDB, Redis etc...
• Any application that stores data
To get the code for stateful application use this link:
https://github.com/devops0014/k8s-stateful-set-application.git

--> will use statefullset component

   two pods

   1) Primary pod ( Read & write )
   2) secondary ( Read)

For statefullset manifest file same as fr deployment 

vim statefullset.yml

apiVersion: apps/v1
kind: Deployment

metadata:
  name: flm

spec:
  replicas: 3

  selector:
    matchLabels:
      app: zomato

  template:
    metadata:
      labels:
        app: zomato

    spec:
      containers:
      - name: test-1
        image: shaikmustafa/cycle

        ports:
        - containerPort: 80

   kubectl create -y statefullset.yml

will notices created statefull set

kubectl get statefulset 
kubecetl get nodes

will see nodes name sequencial ( ex flm0, flm1 & flm3)

Scale out 

kubectl scale statefulset flm --replicas=6

will notice pods increasing count
kubectl get po

here pods created one by one ( once created only then its create one more pod )  in deployment all pods create at time


scale down
kubectl scale statefulset flm --replicas=2


will noticed deleted pods which created latest ( LIFD) 

---------------> daemon set   <------------

its used to create an agent (pod) on each worker nodes

There is no replicas for daemonset

will create pod to get data from server so daemonset create pods here


if will create wokernode in cluster in future  daemonset will create pod automatically
-- > to get CPU & Memnory info
--> log info
--> to runthe script 

write a manifest file 
 its same as it is like deployment file only Kind will change

 Vim daemonset.yml
 apiVersion: apps/v1
kind: daemonset

metadata:
  name: mydaemon ( name is our wish)

spec:
  selector:
    matchLabels:
      app: zomato

  template:
    metadata:
      labels:
        app: zomato

    spec:
      containers:
      - name: test-1
        image: shaikmustafa/cycle

        ports:
        - containerPort: 80

Kubectl create -f daemonset.yml

kubectl get ds    to get list of daemon set

in one node only in pod we can't replica here


to increase server (worker nodes )

we have to to run these command and edit min & max 

kops edit ig --name=ccit.k8s.local nodes-us-east-1a

kops edit ig --name=ccit.k8s.local nodes-us-east-1b

---> to update  <----

kops update cluster --name ccit.k8s.local --yes --admin


Kubectl get no

will notice nodes created

kubectl get po

will notice clusters created











