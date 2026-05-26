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
  
