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
    reasons : kubelet not ready , disk utilise complete also , n/w unavailability , if kubelet down in one workernode, node controller will check evrysec about node its check commuication b/w api server  and kubelet won't get response so node not ready state .
    solution : go to particular worker node and check either kubelet is there or not
               n/w issues also : if someone chnaged SG ,
    diskutilise : will delete unnessary data, if its imp will zip and move to s3

21) Pod unscheduled
     reason :  resources : when we didn't have sufficient resources from our worker nodes
              taint & tolerations : There is no tolerance
              node selector  :
              affinities  :    solution : prefered during schedule
              secretnot find :  solution : if the secret not in same spaces
              PVC pending :   solution : will change storage class type automate   ( will use ebs.csi.aws.com)


    Scenario Based Questions & A

    1) Production outage : podrashloop
        error rate : 1% to 40 % increased
        pod : crashloopbackoff
        HPA : creates more pods
       solution : Rollback to previoues version  -> if roll back fails will pause deployment
                  Freeze the HPA ,
                   Investigae : get the logs from pods
                                events from the pods
                                      OOOMKILLEd
                                       probfailed
       <img width="294" height="159" alt="image" src="https://github.com/user-attachments/assets/3f6b85ef-3526-4cee-8067-1c55b1ed5035" />

will use canary deployment 


2) node under pressure   CPU 90 % used , memory 85% used  happening pod eviction
     Kubectl get node
     kubectl top no
         new pod scheduled & taint 
     solution : memory pressure true
               Disk pressure  true
               processure id : true
       will shedule autoscalling for cluster

3)  how to design zero downtime deployment
      i need zero downtime
      rollback not more than 5 sec
      deploymets 500 pods at time
      minimal customer support
        ans :
    "To achieve zero downtime, I would use Kubernetes Rolling Updates with maxUnavailable=0 and an appropriate maxSurge value. I would configure readiness and liveness probes so traffic reaches only healthy pods. For 500 pods, I'd deploy in controlled batches while ensuring sufficient cluster capacity. The CI/CD pipeline would include testing, security scanning, and monitoring. I'd use Prometheus and Grafana to detect issues, and if failures occur, I'd execute kubectl rollout undo to quickly revert to the previous ReplicaSet. This approach provides zero downtime, rollback within seconds, and minimal customer impact."


4)   FE pod > not communication backend service 
  FE running
  DNS working
  service exist
       pod --> kubectl exec -it pod-name -bash
       curl http://backend:8080
       connection fail
   either this service expose that pod or not

     
    
    proper labels selectors and probes 

5) CI/CD + Kubernetes

Rollbacks are slow
Configuration drift issues
Manual approval causing deployment delays
   ans : 
   "For this scenario, I would improve the deployment process by using Kubernetes Rolling Update or Blue-Green deployment for fast rollback. To eliminate configuration drift, I would implement GitOps using Argo CD, where Git is the single source of truth and any manual changes are automatically corrected. To reduce deployment delays, I would automate approvals for non-production environments and keep only a single approval before production deployment. This approach provides faster releases, rollback within seconds, consistent environments, and minimal customer impact.
    
  
     
    

    
    
   
   
   


   
   

   
   
 


   


 
    
   
 




