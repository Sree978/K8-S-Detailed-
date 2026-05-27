The volumes reside inside the Pod which stores the data of all containers in that pod.

TYpes of volumes:
  1) EmptyDir
  2) HostPath
  3) Persistent Volume
  4) Persistent Volume Claim(PVC)
1) EMPTY DIR:
  • This volume is used to share the data between multiple containers within a pod instead of the host machine or any Master/Worker Node.
  • EmptyDir volume is created when the pod is created and it exists as long as a pod.
  • There is no data available in the EmptyDir volume type when it is created for the first.
  • Containers within the pod can access the other containers' data. However, the mount path can be different for each container.
  • If the Containers get crashed then, the data will still persist and can be accessible by other or newly created containers.

ec2 server & install minikube

In real time will single application on single POD

we have two conatiner 
1) Application container 
2) helper conatiner /sidecarconatiner    -  will help get 1st container backup logs, to provide for application container & to run script on main conatiner  & this conatiner will be always in running state. 

fiest container will run application 2nd conatiner will support for first conatiner

vim deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flm
spec:
  replicas: 1
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
          image: nginx
          volumeMounts:
            - mountPath: "/flm"
              name: myvolume

        - name: cont-2
          image: nginx
          command: ["/bin/bash", "-c", "while true; do echo this is k8s; sleep 5; done"]
          volumeMounts:
            - mountPath: "/devops"
              name: myvolume

      volumes:
        - name: myvolume
          emptyDir: {}

kubectl create -f deployment.yml

will notice created deployment

---> to go inside here we have to two container ( if we wont mnetion particular container its go to default fiest conatiner)
if we make chnages in container one same changes will reflect in container 2 & if we make chnages in container two same changes will reflect in container one

kubectl exec -it flm-7d9d7959f7-4dxhz -c cont-2 -- bash

if we delete pod Replicaset (RS) will create one more pod default
but we loss the data which is in conatiners ( container which in pods)
these type of volumes is emptyDir 

to over this drawback will use  hostpath volume 

---------->HostPath volume  <---------------
here will get data even pods deleted replica set will create dafault pods and our hostPath volume will help to get same data which is in conatiners of 

delete never delete even pod deleted 
--> same deployment file but will change only volume name

 volumes:
        - name: myvolume
          hostPath:
            path "tmp/mydta"


 drawback
 -> we can't handle for two servers  (multi node cluster )
 Id pod create in worker node 1 its fine if its craetes in worker node2 the volume wont apllicable so data also delete
 -> if server delete volume also delete
   
    toover come this drawback will use

will use PV & PVC volume persistent volume & persistent volume claim

----------> PV & PVC  Volume <-------------
 here will use EBS volume 

 even if dleted volumes, pods & cluster also will get same data from EBS volume
 
PV can't able to attach directly to pod will use PVC 

<img width="1092" height="532" alt="image" src="https://github.com/user-attachments/assets/7b19907d-3075-45ae-bb88-c9d5cad97cc0" />



Create a EBS volume & get Volume id

vim pv.yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-1
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  awsElasticBlockStore:
    volumeID: vol-0f3275cf8019de550
    fsType: ext4

 kubectl create -f pv.yml

 will notice created PV 

 Kubectl get pv

 Kubectl describe pv pv-1  

 Create PV 10 GB again
 

 --> create a Pod i want PV is 7 gb

 we have to create pvc
 

 vim pvc.yml
 apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-1

spec:
  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 7Gi

 kubectl create -f pvc.yml
      will notice created pVC

kubectl get pvc   wil notice list of PVC's


--> This PVC will attch to POD now 

vim deployment.yml

same deployment file

will chnag only
volumes:
        - name: myvolume
          persistentVolumeClaim:
            claimName: pvc-1
 kubectl create -f deplyment.yml

 Now our data will be safe even volume, pod cluster delete also beacuse the data stored in EBS volume
 



 <img width="1077" height="205" alt="image" src="https://github.com/user-attachments/assets/cac3b0ca-6182-4ed9-9a65-d72070efd6a0" />

 

 

          


  
