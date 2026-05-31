What is a Network Policy?
    Network Policies in Kubernetes are like firewall rules for pods. They control which pods can communicate with each other and 
with other network endpoints (e.g., external services).
These policies are implemented at the network level by Kubernetes-compatible Container Network Interface (CNI) plugins, 
such as:
    Calico
    Cilium
    Weave Net
Order of Evaluation
    1) If a pod has no policy, all traffic is allowed.
    2))If a pod has a policy, then only the required traffic is allowed.
    3) If a pod has one or more policies, only allowed traffic can pass.

Some Technical Terms to Remember:
    INGRESS RULE: Who can come into the pod
    EGRESS RULE: Where the pod can go out
    POD SELECTOR: Target pods to which the policy applies
    NAMESPACE SELECTOR: Limit by namespace
<img width="1142" height="631" alt="image" src="https://github.com/user-attachments/assets/c5656dc3-a64c-4419-8e70-40e50cfc6926" />


<img width="1114" height="634" alt="image" src="https://github.com/user-attachments/assets/c7889638-981f-464f-b0db-a8d3cbf8c38d" />
