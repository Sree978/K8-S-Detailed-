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
        

   4  kubectl get pods
    5  kubectl set image deploy flm
    7  kubectl set image deploy flm test-1=shaikmustafa/paytm:bus
    will notice image directly update 
    
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




