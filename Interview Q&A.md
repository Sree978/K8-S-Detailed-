1) Production level frequent error in K8's

1.1) CrashloopBackoff pods:
reasons : Misconfigeration of env variables, Dependencies miss, bad application code

Troubleshoot :
    1> Describe pod    -> will get config , env varable 
    
1.2) Imagepullbackoff :
Reason : wrong image, docker hub wrong credetails, wrong image tag, private image
 Troubleshoot :
    1> Describe pod    -> will get config , env varable 
    2> check secret of Dokerhub
    3>image registry ( image pesent in the regidtry or not )
    
1.3) OOMKILLed: out of memory , when conatiner uses more memory, Conatiner exceed memory limit

   Troubleshoot:
   1> Increase pvc
   2> delete unwanted data from the container
  
 
1.4) CPUthrottlling: ove CPU utilization
  Troubleshooot
  1> keep limits and requests to that container
 
1.5) Insifficient Ip address
   Trouble shoot:
 1> need to update subnets on VPC 

1.6) PV is pending state: mismatach of stoarge class or EBS exceed
   Troubleshoot:
  1> increase EBS volumesize
  2> stoarge class check 

1.7) NODE DISK pressure : node running out of disc causing pod eviction
 troubleshoot:
   1> kubectl top nodes ( to get info )
1.8) NODE NOT READY  : when the node is not readt state
   1> Kubectl describe node node-id
1.9 ) Pod pending state  :when no sufficient resources available on worker node
   1> we can autoscale the server 
   2> reducue the limitations of pod 
1.10) unauthorised access : when we don't have permissions to access the resources 
1.11) SECRET MISMANAGEMENT : this will happen when API gets expired, exposing the secrets on logs


1) What K8's Networking and how does it works
     every pod have unique ip address ,pod will communicate with each other and same namespace or different namespace, same server or different server with out nat gateway
   
2) PV & PVC

PV peace of storage provided by admin , PVC is request storage from the user

   It's used to decouple the storage from the pod, to store data in volume not in pods

3) Whats RBAC in K8's
 Its securoty mechanism that controls the access to resources using roles and cluster roles and binding

   
4)  How does autoscalling works K8's 
    it supports two types of auto scalling
   > Horizontal & vertical    - will use Horizontal in real time, here the pods will get created when , the CPU hits the average utilization  ,
if the no of request getting high automatically no of pods created 
if the no of request getting hLow automatically no of pods deleted  

   


 
    
   
 




