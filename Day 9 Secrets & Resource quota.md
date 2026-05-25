------------->  Secrets  <-------------

will used to store Sensitive or confidential data
vim secrets.yml
  apiVersion: v1
kind: Secret
metadata:
  name: sec1
data:
  NAME: "mustafa"
  Place: "Hyd"
  Company: "FLM"

echo "mustafa" | base64             --> to encode data

In secret.yml file we have to give encoded data 

 \apiVersion: v1
kind: Secret
metadata:
  name: sec1
data:
  NAME: "bXVzdGFmYQo="
  Place: "aHlkCg=="
  Company: "RkxNCg=="

kubectl create -f secrets.yml
will notice created secret
kubectl get secrets

kubectl describe secret sec1
kubectl get secret sec1 -o yaml    - will notice encrypted data here 

 echo "RkxNCg==" | base64 -d        ----> for decrypt encypted data

                     ( or )    real time will use mostly 
                    

  kubectl create secret generic sec2 --from-literal=username=mustafa --from-literal=password=admin@123      to encrypt values 

  kubectl get secret sec2 -o yaml

                (or)
vim app.env 

course=devops
cloud=aws-
year=2025

kubectl create secret generic sec3 --from-env-file=app.env         -> to create secret 
kubectl get secret
kubectl get secret sec3 -o yaml

will notice here encrypted data


-->  to attah secrets to pods
vim pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
spec:
  containers:
    - name: cont-1
      image: nginx
      ports:
        - containerPort: 80
      envFrom:
        - secretRef:
            name: sec1

        - secretRef:
            name: sec2
kubectl create -f pod.yml

will notice created pod

kubectl get po
kubectl exec -it pod-1 -- bash      to go inside 
printenv

will notice all data which we given

we can attatch indivudval values from indidval secrets to pods


---------------------->  Resource Quota <------------------

Kubernetes ResourceQuota is used to limit resource usage inside a namespace.
  Practically companies use it to prevent:
    One team consuming all cluster resources
    Too many pods getting created accidentally
    High CPU/Memory usage affecting other applications
• By default pod in Kubernetes will run with no limits on CPU and memory.
• You can specify the RAM, Memory, or CPUs for each container and pod.
• The scheduler decides which node will create pods, if the node has enough CPU resources available then, the node will place the pods.
• CPU is specified in units of cores and memory is specified in units of bytes.

  1) request  - min required resources to create pod   ( ex request.cpu=2)
  2) limits  - max utilization  ( limits.cpu=4)

create a namespace & create pods in namespace

kubectl create ns food
vim resourcequota.yml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: food-rq
  namespace: food
spec:
  hard:
    requests.cpu: "500m"
    limits.cpu: "1000m"
    requests.memory: "50Mi"
    limits.memory: "100Mi"
kubectl create -f resourcequota.yml
 will notice created resourcequota

 kubectl get quota -n food        --> to get details 

if you have limits for namespace for quota also limits should  be

vim pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
  namespace: food
spec:
  containers:
    - name: cont-1
      image: nginx
      ports:
        - containerPort: 80
      resources:
        requests:
          cpu: "50m"
          memory: "10Mi"
        limits:
          cpu: "100m"
          memory: "15Mi"

    kubectl create -f pod.yml

will notice created pod



