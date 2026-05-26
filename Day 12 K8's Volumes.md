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
            - mountPath: "/dev"
              name: myvolume

      volumes:
        - name: myvolume
          emptyDir: {}

kubectl create -f deployment.yml

will notice created deployment

          


  
