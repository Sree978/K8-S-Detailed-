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













      



 

     





