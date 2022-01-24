# Shortly about Kubernetes
## Main elements
### Node
Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.  
Typically you have several nodes in a cluster; in a learning or resource-limited environment, you might have only one node.  
The components on a node include the kubelet, a container runtime, and the kube-proxy.  
### Pod
Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
### Replication Controller
A ReplicationController ensures that a specified number of pod replicas are running at any one time.
### ReplicaSet
A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.
### DaemonSet
A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected.
Some typical uses of a DaemonSet are:
- running a cluster storage daemon on every node
- running a logs collection daemon on every node
- running a node monitoring daemon on every node
### Job
A Job creates one or more Pods and will continue to retry execution of the Pods until a specified number of them successfully terminate. As pods successfully complete, the Job tracks the successful completions. When a specified number of successful completions is reached, the task (ie, Job) is complete. Deleting a Job will clean up the Pods it created. Suspending a Job will delete its active Pods until the Job is resumed again.
### CronJob
A CronJob creates Jobs on a repeating schedule.
One CronJob object is like one line of a crontab (cron table) file. It runs a job periodically on a given schedule, written in Cron format.
### Service
An abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.
### Ingress
An API object that manages external access to the services in a cluster, typically HTTP.  
Ingress may provide load balancing, SSL termination and name-based virtual hosting.
### Volumes
On-disk files in a container are ephemeral, which presents some problems for non-trivial applications when running in containers.  
One problem is the loss of files when a container crashes. The kubelet restarts the container but with a clean state.  
A second problem occurs when sharing files between containers running together in a Pod.  
The Kubernetes volume abstraction solves both of these problems. At its core, a volume is a directory, possibly with some data in it, which is accessible to the containers in a pod. How that directory comes to be, the medium that backs it, and the contents of it are determined by the particular volume type used.
### Storage Classes
A StorageClass provides a way for administrators to describe the "classes" of storage they offer. Different classes might map to quality-of-service levels, or to backup policies, or to arbitrary policies determined by the cluster administrators. Kubernetes itself is unopinionated about what classes represent. This concept is sometimes called "profiles" in other storage systems.
***
_This info from https://kubernetes.io/docs/._
