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
   > Horizontal & vertical    - will use Horizontal in real time, here the pods will get created when , the CPU hits the average utilization  , no of request or memory utilization
if the no of request getting high automatically no of pods created 
if the no of request getting hLow automatically no of pods deleted

 VPA 
-> no of pod wont create , instead of creating no of pods here the resources (CPU RAM ) will get increase in the same pod,pod get restart for increasing the resources 

5) How do you debug te kubernetes Pods
   ans: DESCRIBE
        LOGS
        EXEC
        EVENTS
        KUBECTL TOP
6) How does rolling update works in K8's
    whenever we update image for deployment , the Deployment will craete a new replicaset from thid replicaset new pods created, when new pod created from repliacset old pod deleted from old replica set .
   so now traffic routed to the newly created pods
   
7) Whats ingress in k8's & how does it works
   ans :
   Ingress is used to manage the external traffic , (http https ) based on roles
   ingress supports bith host based and path based routing and aso it supports the ssl and tsl termination

9) what happen if your pod resurces need to grow beyond the assigned limits ?

   OOOkilled error
   
11) what are sidecar containers & helper conatiners
Usuvally pod contains two containers  primary ( where application going to run)
    second container which helps the first container to run the application
    2nd container used for multiple purpose one is taking backup from the main application from the conatiner
     second one is excuting the script on application Containers
    3rd Storing configmaps , passing API keys etc

12)  QOS ( Quality of service )

    <img width="709" height="404" alt="image" src="https://github.com/user-attachments/assets/4c26d7f7-b672-42ab-828a-84e3afbc0b0d" />

When a Node runs out of memory, Kubernetes decides which Pod to remove first based on its Quality of Service (QoS) class.
QoS Classes
Guaranteed → Strongest priority, least likely to be evicted.
Burstable → Medium priority, may be evicted if needed.
BestEffort → Weakest priority, evicted first under pressure.
Example Scenario
NODE‑1 is running out of memory.
Kubernetes checks QoS of all running Pods.
Pod3 has BestEffort QoS (no fixed CPU/memory requests).
Since BestEffort Pods are lowest priority, Pod3 gets evicted first ❌. 
The good news?  
When a Pod is evicted due to resource pressure, Kubernetes doesn’t just stop there — it reschedules Pod3 to NODE‑2 where there is enough space.

🔑 Key Takeaway
If you want your Pods to be less likely to get evicted:
Always define proper CPU/memory requests.
This ensures Pods fall into at least the Burstable class, or ideally the Guaranteed class.
that way, Kubernetes prioritizes keeping them alive during node pressure.

Pod Affinity → Ensures Pods are scheduled together on the same node (e.g., for apps that benefit from locality).

Pod Anti‑Affinity → Ensures Pods are scheduled apart on different nodes (e.g., for high availability).

Node Affinity → Restricts Pods to run on specific nodes that match labels (e.g., GPU nodes, SSD storage).

Node Anti‑Affinity → Prevents Pods from running on certain nodes (e.g., avoid low‑memory nodes).

13) Headless service

14) Crashloopbackoff

 k8s created pod > inside pod we have conatiner > once start container its crash > k8 will try to restart and try to create conatiner > again conatiner start and crash > again k8 will try to create container   --> its called loopbackoff
  why : container crashed 
  identify & solution : 1) describe pod   
  reasons : prob fail , permision denied , oomkill 

15) Imagepullbackoff
       kublet try pull the image but its failed, again try its again failed   --> its called backoff
    solution : kubectl get po  or kubctl describe pod podname
    reasons : wrong image tag 0r name ( ex myapp v1.1.4) ,
           private : pull access denied , secret container
16)   Errimagepull
       will get instance error,  kublet try pull the image but its failed will get Errimagepull

17) CreateConatinerConfigError

 Backend whats hapoen       failed create container
                            conatiner never start
                            image never pull
                            no app code run 
solution : describe 
    dueto config maps , if won't pass Environment variables properly, volume mount 

18) createcontainer error

    dependecies not installes
    db conatiner not started
    permission issues
    if application run non-root user
19 ) OOMKILLed
      Reasons : resources requests: 500 mi  and limits: 1 gi   -> will get resources from node   sigkill will kills conatiner 
     solution : will increase resources
20) nodenotready
     while creating cluster
    reasons : kubelet not ready , n/w unavailability , if kubelet down in one workernode, node controller will check evrysec about node its check commuication b/w api server  and kubelet won't get response so node not ready state .
    solution : go to particular worker node and check either kubelet is there or not 
    
    
    
  
     
    

    
    
   
   
   


   
   

   
   
 


   


 
    
   
 




