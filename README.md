# Assignment-k8-Voting App
assignment
K8s Assignment submission (Aug 27th):
**Voting App Installlation**

Last login: Sat Aug 27 06:36:15 2022 from 49.37.167.46

   __|  __|_  )
   _|  (     /   Amazon Linux 2 AMI
  ___|\___|___|
[ec2-user@ip-172-31-1-221 ~]$ sudo su - 
Last login: Sat Aug 27 06:36:19 UTC 2022 on pts/1 
[root@ip-172-31-1-221 ~]# kubectl get nodes 
NAME STATUS ROLES AGE VERSION 
ip-172-31-1-221.ap-southeast-1.compute.internal Ready master 2d20h v1.18.5 
ip-172-31-3-15.ap-southeast-1.compute.internal Ready 2d19h v1.18.5 
[root@ip-172-31-1-221 ~]# 
cd $home [root@ip-172-31-1-221 ~]# pwd /root 
[root@ip-172-31-1-221 ~]# git clone https://github.com/ashishrpandey/example-voting-app 
[root@ip-172-31-1-221 example-voting-app]# cd k8s-specifications/ 
[root@ip-172-31-1-221 k8s-specifications]# kubectl apply -f . 
deployment.apps/db created service/
db created deployment.apps/redis created service/
redis created deployment.apps/result created service/
result created deployment.apps/vote created service/
vote created deployment.apps/worker created 

[root@ip-172-31-1-221 k8s-specifications]# kubectl get all 
NAME READY STATUS RESTARTS AGE 
pod/db-b54cd94f4-cpff4 0/1 ContainerCreating 0 14s 
pod/redis-868d64d78-sbd7j 0/1 ContainerCreating 0 14s 
pod/result-5d57b59f4b-wjhxx 0/1 ContainerCreating 0 13s 
pod/vote-94849dc97-hwflg 0/1 ContainerCreating 0 13s 
pod/worker-dd46d7584-tdcgx 0/1 ContainerCreating 0 13s

NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE 
service/db ClusterIP 10.108.51.223 5432/TCP 14s service/kubernetes ClusterIP 10.96.0.1 443/TCP 2d20h 
service/redis ClusterIP 10.106.88.37 6379/TCP 14s 
service/result NodePort 10.105.83.52 5001:31001/TCP 13s 
service/vote NodePort 10.100.209.53 5000:31000/TCP 13s

NAME READY UP-TO-DATE AVAILABLE AGE 
deployment.apps/db 0/1 1 0 14s 
deployment.apps/redis 0/1 1 0 14s 
deployment.apps/result 0/1 1 0 13s 
deployment.apps/vote 0/1 1 0 13s 
deployment.apps/worker 0/1 1 0 13s

NAME DESIRED CURRENT READY AGE 
replicaset.apps/db-b54cd94f4 1 1 0 14s 
replicaset.apps/redis-868d64d78 1 1 0 14s 
replicaset.apps/result-5d57b59f4b 1 1 0 13s 
replicaset.apps/vote-94849dc97 1 1 0 13s 
replicaset.apps/worker-dd46d7584 1 1 0 13s 

[root@ip-172-31-1-221 k8s-specifications]#


** DELETION OF VOTING POD**********

[root@ip-172-31-7-173 ~]# kubectl get po 
NAME READY STATUS RESTARTS AGE 
db-b54cd94f4-cpff4 1/1 Running 0 10m 
redis-868d64d78-sbd7j 1/1 Running 0 10m 
result-5d57b59f4b-wjhxx 1/1 Running 0 10m 
vote-94849dc97-hwflg 1/1 Running 0 10m 
worker-dd46d7584-tdcgx 1/1 Running 0 10m 

[root@ip-172-31-7-173 ~]# kubectl delete po vote-94849dc97-hwflg
pod "vote-94849dc97-hwflg" deleted

[root@ip-172-31-7-173 ~]# [root@ip-172-31-7-173 ~]# kubectl get po 
NAME READY STATUS RESTARTS AGE 
db-b54cd94f4-cpff4 1/1 Running 0 12m 
redis-868d64d78-sbd7j 1/1 Running 0 12m 
result-5d57b59f4b-wjhxx 1/1 Running 0 12m 
vote-94849dc97-4hqvh 1/1 Running 0 17s 
worker-dd46d7584-tdcgx 1/1 Running 0 12m 
[root@ip-172-31-1-221 ~]#**

**OBSERVATION ** ==> AFTER DELETION, THE DELETED POD GOT RECREATED AND THE APPLICATION IS WORKING IN THE BROWSER Ex:DOG 100% and CAT 0


***************** DELETION of WORKER POD ******************* 

[root@ip-172-31-7-173 ~]# kubectl get po **
NAME READY STATUS RESTARTS AGE 
db-b54cd94f4-cpff4 1/1 Running 0 17m 
redis-868d64d78-sbd7j 1/1 Running 0 17m 
result-5d57b59f4b-wjhxx 1/1 Running 0 17m 
vote-94849dc97-4hqvh 1/1 Running 0 5m34s 
worker-dd46d7584-znl45 1/1 Running 0 58s 
[root@ip-172-31-7-173 ~]# kubectl delete po worker-dd46d7584-znl45 
pod "worker-dd46d7584-znl45" deleted

[root@ip-172-31-7-173 ~]# kubectl get po 
NAME READY STATUS RESTARTS AGE 
db-b54cd94f4-cpff4 1/1 Running 0 18m 
redis-868d64d78-sbd7j 1/1 Running 0 18m 
result-5d57b59f4b-wjhxx 1/1 Running 0 18m 
vote-94849dc97-4hqvh 1/1 Running 0 6m45s 
worker-dd46d7584-q5q8s 1/1 Running 0 52s 
****OBSERVATION **** ==> AFTER DELETION OF WORKER POD, THE DELETED POD GOT RECREATED AND THE APPLICATION IS WORKING IN THE BROWSER Ex:DOG 0% and CAT 100%

***************** DELETION of DB POD ******************* 

[root@ip-172-31-1-221 ~]# kubectl get po 
NAME READY STATUS RESTARTS AGE 
db-b54cd94f4-cpff4 1/1 Running 0 20m 
redis-868d64d78-sbd7j 1/1 Running 0 20m 
result-5d57b59f4b-wjhxx 1/1 Running 0 20m 
vote-94849dc97-4hqvh 1/1 Running 0 8m42s 
worker-dd46d7584-q5q8s 1/1 Running 0 2m49s
 
[root@ip-172-31-1-221 ~]# kubectl delete po db-b54cd94f4-cpff4 
pod "db-b54cd94f4-cpff4" deleted 

[root@ip-172-31-1-221 ~]# kubectl get po 
NAME READY STATUS RESTARTS AGE 
db-b54cd94f4-2dbzh 1/1 Running 0 56s 
redis-868d64d78-sbd7j 1/1 Running 0 21m 
result-5d57b59f4b-wjhxx 1/1 Running 0 21m 
vote-94849dc97-4hqvh 1/1 Running 0 9m52s 
worker-dd46d7584-q5q8s 1/1 Running 1 3m59s 
[root@ip-172-31-1-221 ~]#

****OBSERVATION **** ==> AFTER DELETION OF DB POD, THE DELETED POD GOT RECREATED AND THE APPLICATION IS WORKING IN THE BROWSER BUT THE VALUES ARE COMING INCORRECT!!!! EX: DOG 50% and CAT 50%
