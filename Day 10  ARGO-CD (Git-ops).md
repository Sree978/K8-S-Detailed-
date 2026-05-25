.   ---------------->   GITOPS  <------------------
GitOps is a way of managing software infrastructure and deployments using Git as the source of truth.
Git as the Source of Truth:
    In GitOps, all configurations like Deployments, Services, Secrets, ConfigMaps, etc., are stored in a Git repository.
Automated Processes:
    Whenever we make changes in YAML files, GitOps tools like Argo CD or Flux detect those changes and automatically apply them to the Kubernetes cluster.
    It ensures that the live infrastructure always matches the configurations available in the Git repository.
Continuous Deployment:
    Whenever changes are pushed to Git, they are automatically reflected in the Kubernetes cluster without manual intervention.
Developer → Push Changes to Git
                    ↓
        Argo CD / Flux Detect Changes
                    ↓
      Automatically Sync to Kubernetes
Advantages of GitOps
      Automated deployments
    Easy rollback using Git
    Better version control
    Improved cluster consistency
    Reduced manual errors
    Faster deployments
    Common GitOps Tools
    Argo CD
    Flux
Simple Definition
GitOps = Managing Kubernetes using Git + Automation



whatever will make chamges in github file and commints its automaticly reflect in k8's cluster while we using argo CD

WITHOUT ARGO CD:
        Before ARGO CD, we deployed applications manually by installing some third party tools like kubectl, helm etc...
        If we are working with KOPS, we need to provide our configuration details (RBAC) or If we are working on EKS, we need to    provide our IAM credentials.
If we deploy any application, there is no GUI to see the status of the deployment.
so we are facing some security challenges and need to install some third party tools.

==> Points to be noted:  <==
1.Once if we implement ArgoCD, if we make any changes manually in our cluster using the kubectl command, Kubernetes will reject those request from that user. Because when we apply changes manually, ArgoCD will check the actual state of the cluster with the desired state of the cluster (GitHub).
2.If we make any changes in the GitHub like increasing the replicas in deployment ArgoCD will take the changes and applies in our cluster. So that we can track each and every change and it will maintain the history.
3.We can easily rollback using git if something went wrong.
4.If our entire cluster gets deleted due to some network or other issues, we don’t need to worry about it, because all our configuration files are safely stored in GitHub. So we can easily re-apply those configuration files.

----> Install Argo CD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get all -n argocd
kubectl get ns   - we created namespace for argocd 
kubectl get all -n argocd  
   will notice created pods ,services , deployment statefulset replica set these all services in cluster ip
   
   EXPOSE ARGOCD SERVER:

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
yum install jq -y
export ARGOCD_SERVER='kubectl get svc argocd-server -n argocd -o json | jq --raw-output .status.loadBalancer.ingress[0].hostname'
echo $ARGOCD_SERVER
kubectl get svc argocd-server -n argocd -o json | jq --raw-output .status.loadBalancer.ingress[0].hostname
 wait for a min and last command will provide DNS server over that wil access ARGOCD 

 click on adavace browser will work  un : admin

TO GET ARGOCD PASSWORD 
 export ARGO_PWD='kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d'
 echo $ARGO_PWD
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

you will get password and paste and login 
for chnage password userinfo & update password 


