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
