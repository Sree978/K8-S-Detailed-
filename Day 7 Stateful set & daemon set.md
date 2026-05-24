
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


