<img width="1135" height="667" alt="K8&#39;s Architecture" src="https://github.com/user-attachments/assets/dca2458b-a5a4-4147-b2f5-8805dcc3a9d3" />


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

      1) Mater Node  (Control plane)
            we have 4 components
                1)API server
                2) controller manager
                3)scheduler
                4)etcd

      2) worker Node  (



