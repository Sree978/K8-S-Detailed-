Install cluster and write Deployment file ( manifest file )

1  export TMOUT0
Vim deployment.yml
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
  Kubectl create -y deoployment.yml
  will notice deploy created 

----> to expose we need to write a service file 
 vim svc.yml
 
apiVersion: v1
kind: Service

metadata:
  name: mysvc

spec:
  type: LoadBalancer

  selector:
    app: zomato

  ports:
  - port: 80
    targetPort: 80


kubectl create -f svc.yml

will notice svc created
o/p = ad339e59a2ee44f67bdf34d9fcc36abe-786411514.us-east-1.elb.amazonaws.com  will access over browser


  1) IMAGE UPDATE DIRECTLY
     
  Over manifest file

  same deployment file 
  image: shaikmustafa/bus 

  Kubectl apply -y deoployment.yml

  will go to browswer and notice update image
  
  over commn
  
    5  kubectl set image deploy flm
    7  kubectl set image deploy flm test-1=shaikmustafa/paytm:bus
    will notice image directly update 
2)  rollout   
    
    8  kubectl rollout status deploy flm
    9  kubectl ge trs
   10  kubectl get rs
   11  kubectl rollout undo deploy flm --to-revision=1
   12  kubectl get rs
   13  kubectl rollout history deploy flm
   14  kubectl rollout undo deploy flm --to-revision=4
   15  kubectl rollout history deploy flm
   16  kubectl rollout undo deploy flm --to-revision=3
   17  kubectl rollout history deploy flm
   18  kubectl rollout undo deploy flm --to-revision=2
   19  kubectl rollout history deploy flm
   20  kubectl annotate deploy flm kubernetes.io/change-cause="my detailed change description"
   21  kubectl rollout history deploy flm
   22  kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
   23  clear
   24  vim hpa.yml
   25  history


<img width="743" height="202" alt="image" src="https://github.com/user-attachments/assets/b45d8cad-f567-4b19-98b3-0430c96c31bc" />

Metric server  install :


kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

scallinfg two types vertical scalling & Horizontacl scalling

will use Horizonatl pod autosacle real time

ckubectl api-resources | grep -i "hpa"     ---- > to get api version of HPA
vim hpa.yml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: myhpa

spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: flm

  minReplicas: 3
  maxReplicas: 20

  metrics:
  - type: Resource

    resource:
      name: cpu

      target:
        type: Utilization
        averageUtilization: 60

    kubectl create -f hpa.yml
    
[root@deploy ~]# kubectl get hpa
NAME    REFERENCE        TARGETS       MINPODS   MAXPODS   REPLICAS   AGE
myhpa   Deployment/flm   cpu: 0%/60%   3         20        3          83s

kubectl exec -it podname --bash   --> to go inside bash
    (  to creatte dummy storage will use stress
   apt install stress -y    )

   inside pod and apply stress 
   cmd: stress -c 2 -t 300 -w

   will notice pod created 

   control + c for stop autoscalling 
   
   
   
   
   






