
Docker Swarm   Vs  K8's
we do not have auto-scalling/ no rollback / downtime (can't able ato access application) indocker swarm
will use to overcome all aboveprefer K8's

1) auto saclling
2)rollback to any version
3)no downtime

Docker swarm    - will use for small application  (company websites , bike showroom  for lesss customers) less cost

K8's            - (e -cmommerce & insurance company IRCTC youtube users etc)

Arhitecture
<img width="1135" height="667" alt="K8&#39;s Architecture" src="https://github.com/user-attachments/assets/dca2458b-a5a4-4147-b2f5-8805dcc3a9d3" />
we have two nodes 

      The Components of Kubernetes Architecture
There are mainly two components of the architecture:

Master nodes (Also known as Control Plane)

Worker nodes (Also known as slave nodes or minions)

Lets understand this architecture with movie-set team example

1. The Movie Set (Kubernetes Cluster)
The movie set is your Kubernetes Cluster, where all the action happens. It consists of:

Actors and Crew (Worker Nodes): The people who do the actual work—filming scenes (running applications).

Director and Production Department (Control Plane): They manage the entire production to ensure everything runs as planned.

2. The Production Team (Control Plane)
It is known as the brain of the Kubernetes system, as all the decisions are taken here. Kubernetes master receives inputs from a CLI or user interface via an API. Using the master node, you define pods, deployments, configurations, replication sets that you want Kubernetes to manage and maintain.

The production team handles all the behind-the-scenes tasks:

Director (API Server): The point of contact for all requests, ensuring the right instructions are passed to the crew.

APIs help different applications talk to each other. On the master node, there's a part that makes the Kubernetes API available. It acts as the main point of contact for all the management tasks in Kubernetes.

Assistant Director (Scheduler): Decides which actor or crew member (node) will perform each task (app).

Decides which node should run each pod based on available resources.

Continuity Team (Controller Manager): Checks if the scenes are being shot correctly. If something is missing (e.g., an actor), they fix it immediately.

It is the component of the master node that runs the controller processes. Each controller is a separate process and performs a particular function.

These controllers include:

Node controller: Responsible for noticing and responding when nodes go down.

Replication controller: Responsible for maintaining the correct number of pods for every replication controller object in the system.

Endpoints controller: Populates the Endpoints object (that is, joins Services & Pods).

Service Account & Token controllers: Create default accounts and API access tokens for new namespaces.

Script Supervisor (etcd): Keeps track of everything—scenes completed, locations, and cast details.

It is the consistent and highly available key-value store used as Kubernetes’ backing store for all cluster data. It is only accessible from the API server. It can be part of the Kubernetes master node, or it can be configured externally.

3. The Workers and Crew (Worker Nodes)
The workers (worker nodes) are where the filming (app running) takes place.

Scenes (Pods): These are individual scenes being filmed in different parts of the set, and each pod represents a specific task or part of the film (an app).

A Pod is the smallest unit in Kubernetes. It’s a group of one or more containers that share the same network space. Pods are where your applications actually run in Kubernetes.

Kubernetes will automatically restart pods if they fail. Pods can also be scaled up or down depending on demand.

set designer (kubelet): communicates with director and design the set as per the scenes and directors requirement.

It is the agent which communicates with the master node and executes the worker nodes. It gets the pod specifications through the API server (kube-apiserver) and executes the containers associated with the pod and ensures that the containers described in those pods are running and healthy.

Agent (kube-proxy): Serves the artists to particular movie based on characters that need

Handles the network, ensuring that requests get routed to the correct pods.

Example in Action
The producer (user) wants to film a scene (deploy an app).

The Director (API Server) receives the request and passes it on.

The Assistant Director (Scheduler) assigns the scene to the appropriate filming location (worker node).

The Station Director (kubelet) ensures everything is ready for the scene.

The Waiters (kube-proxy) deliver the scene's final product to the audience (users).

The Kitchen Equipment (Container Runtime) ensures that the tools (containers) are running smoothly to make the scene happen.



