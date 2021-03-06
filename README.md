[![License: CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/80x15.png)](https://creativecommons.org/licenses/by-sa/4.0/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)


<p align="center">
  <img width="300" src="./pics/cka-logo.png">
</p>


### Workloads & Scheduling – 15%
1. [Understand deployments and how to perform rolling update and rollbacks](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

    install metrics server from 
    
    kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
    try to explore the deployment and service
    
    Q-  Which namespace was it installed on? can you change this during installation?
    
    Q-  What is the status of the replica-set created by the deployment?
    
    Q-  Explore and relate the pods to the replicaset to the deployment
    
    Q-  What is wrong with the deployment? can you fix it?
    
    Q-  how many replicasets are associated now with the deployment?
    
    Q- can you tell from the rollout history why? how can you improve on it?
    
    
    
    
3. Use [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) and [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/) to configure applications
  - [configure a pod with a configmap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
  - [configure a pod with a secret](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)
4. Know how to [scale applications](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#scaling-a-deployment)
  - [scaling a statefulset](https://kubernetes.io/docs/tasks/run-application/scale-stateful-set/)
  - [scaling a replicaset](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/#scaling-a-replicaset)
5. Understand the primitives used to create robust, self-healing, application deployments
 - [Replicaset](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
 - [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
 - [Statefulsets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
 - [Daemonset](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
6. Understand [how resource limits can affect Pod scheduling](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#how-pods-with-resource-requests-are-scheduled)
7. Awareness of manifest management and common templating tools
  * [Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)
    - [Kustomize Blog](https://kubernetes.io/blog/2018/05/29/introducing-kustomize-template-free-configuration-customization-for-kubernetes/)
  * [manage kubernetes objects](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/)
  * [Install service catalog using helm](https://kubernetes.io/docs/tasks/service-catalog/install-service-catalog-using-helm/)
    - Non-k8s.io resource: CNCF Kubecon video: [An introduction to Helm - Bridget Kromhout, Microsoft & Marc Khouzam, City of Montreal](https://youtu.be/x2w6T0sE50w?list=PLj6h78yzYM2O1wlsM-Ma-RYhfT5LKq0XC)
   - Non-k8s.io resource: External resource: [templating-yaml-with-code](https://learnk8s.io/templating-yaml-with-code)


### Tips

- Always remember your Cluster context and namespace **in the exam**.

```
wal@Bei:~/code/gitrepos/vmug-cka$ kubectl config get-contexts
CURRENT   NAME            CLUSTER         AUTHINFO        NAMESPACE
          default         default         default         
*         kind-vmug-cka   kind-vmug-cka   kind-vmug-cka   

 ###* I can set namespace, and practice it so I do not forget during exam, or practice using -n namespace in commands *###

wal@Bei:~/code/gitrepos/vmug-cka$ kubectl config set-context --current --cluster=kind-vmug-cka --namespace=vmug-cka
Context "kind-vmug-cka" modified.
wal@Bei:~/code/gitrepos/vmug-cka$ kubectl config get-contexts
CURRENT   NAME            CLUSTER         AUTHINFO        NAMESPACE
          default         default         default         
*         kind-vmug-cka   kind-vmug-cka   kind-vmug-cka   vmug-cka
wal@Bei:~/code/gitrepos/vmug-cka$ kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
env-pod    1/1     Running   3          3h53m
nginx      1/1     Running   0          3h58m
test-pod   1/1     Running   4          4h30m

```
- [Use autocompletion](https://kubernetes.io/docs/tasks/tools/included/optional-kubectl-configs-bash-linux/)
  ```
  echo 'source <(kubectl completion bash)' >>~/.bashrc
  echo 'alias k=kubectl' >>~/.bashrc
  echo 'complete -F __start_kubectl k' >>~/.bashrc
  exec $SHELL    #for bashrc to get executed with the new mods
  ```
  
  You might need to source the profile shell script `source  /etc/profile.d/bash_completion.sh`
  
  ```
  k get pods
  NAME       READY   STATUS    RESTARTS   AGE
  test-pod   1/1     Running   0          2m18s
  ```
- Familiarize yourself with Kuberentios.io docuemntaion
  - Cheatsheet  https://kubernetes.io/docs/reference/kubectl/cheatsheet/
    - Familiarize yourelf how to generate or format outputs:  https://kubernetes.io/docs/reference/kubectl/cheatsheet/#formatting-output 
    - Free YAML and jsonpath course from kodekloud https://legacy.kodekloud.com/p/json-path-quiz
  - Tasks  https://kubernetes.io/docs/tasks/
  - Concepts https://kubernetes.io/docs/concepts/
  - Familiarize yourself with `kubectl explain <api-resource> --recursive`


## Practice cluster using KinD

```
$ cat kind/two-nodes.yaml 
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
name: vmug-cka
nodes:
- role: control-plane
  image: kindest/node:v1.21.2 
- role: worker
  image:  kindest/node:v1.21.2
  
# create a two worker nodes cluster from the above config file
$kind create cluster --config kind/two-nodes.yaml
```

Ensure cluster and kubectl are same version required in the exam, as per exam FAQ it is 1.21 for CKA as of yesterday
```
 $ curl -LO https://dl.k8s.io/release/v1.21.2/bin/linux/amd64/kubectl
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   154  100   154    0     0    175      0 --:--:-- --:--:-- --:--:--   174
100 44.2M  100 44.2M    0     0  4528k      0  0:00:10  0:00:10 --:--:-- 5515k
$ chmod +x kubectl 
$ sudo mv kubectl  /usr/local/bin/
[sudo] password for wal: 

wal@Bei:~/code/gitrepos/vmug-cka$ kubectl  version
Client Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.2", GitCommit:"092fbfbf53427de67cac1e9fa54aaa09a28371d7", GitTreeState:"clean", BuildDate:"2021-06-16T12:59:11Z", GoVersion:"go1.16.5", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"21", GitVersion:"v1.21.2", GitCommit:"092fbfbf53427de67cac1e9fa54aaa09a28371d7", GitTreeState:"clean", BuildDate:"2021-07-12T20:40:20Z", GoVersion:"go1.16.5", Compiler:"gc", Platform:"linux/amd64"}

```
##  scope:

You need to be familiar with **Deployments**, **ConfigMaps & Secrets**, and defining **resource limits and requestes** needed by the containers running the application. When creating a new Pod, the Kubernetes scheduler places the object on an available node. Scheduling rules like **node affinity and taints/tolerations** control and fine-tunes the behavior.


## Data requirements:

- decoupling across environments (e.g public, private, dev, qa, prod)
- scalable sharing of configuration
- Enables immutable images
- Security ( sharing of images, constraining data to specific accounts, keeping secrets out)

![Kelesy Hightower](pics/donot-hardocode.png)
https://twitter.com/kelseyhightower/status/932644210997465089


### passing data directly to command arguments

<details><summary> Q1: create a pod named test-pod running in vmug-cka namespace using image busybox configued with the sleep command to run for an hour     </summary>
   
 ```  

 $ export DO='-o yaml --dry-run=client'
 
 $ kubectl -n vmug-cka run $DO test-pod --image busybox -- sleep 1h|tee test-pod.yaml
 apiVersion: v1
 kind: Pod
 metadata:
   creationTimestamp: null
   labels:
     run: test-pod
   name: test-pod
   namespace: vmug-cka
 spec:
   containers:
   - args:
     - sleep
     - 1h
     image: busybox
     name: test-pod
     resources: {}
   dnsPolicy: ClusterFirst
   restartPolicy: Always
 status: {}
  
 $ kubectl create  -f test-pod.yaml 
 pod/test-pod created
 $ k get pods -n vmug-cka
 NAME       READY   STATUS    RESTARTS   AGE
 test-pod   1/1     Running   0          11s
```
   
 </details>

### Environment variables

<details><summary> Q2: create a pod named env-pod running in vmug-cka namespace using image busybox configued with the sleep command to run for an hour, and sets the following environment variables SERVER=vcsa.home.local, USER=admin@vsphere.local, PASS=N1N5ecur3   </summary>
         
```

	$ kubectl -n vmug-cka run $DO env-pod --image busybox --env SERVER=vcsa.home.local --env USER=admin@vsphere.local --env PASS=N0N5ecur3 -- sleep 1h|tee test-pod.yaml
	apiVersion: v1
	kind: Pod
	metadata:
	  creationTimestamp: null
	  labels:
	    run: env-pod
	  name: env-pod
	  namespace: vmug-cka
	spec:
	  containers:
	  - args:
	    - sleep
	    - 1h
	    env:
	    - name: SERVER
	      value: vcsa.home.local
	    - name: USER
	      value: admin@vsphere.local
	    - name: PASS
	      value: N0N5ecur3
	    image: busybox
	    name: env-pod
	    resources: {}
	  dnsPolicy: ClusterFirst
	  restartPolicy: Always
	status: {}

   ```
 
   </details>

  <details>
  <summary>
   Q3: list the user set enviornment variables only; if that was an exam question they might direct you to store them in a file
  </summary>


  ```

  $ kubectl set env pod/env-pod  -n vmug-cka --list
  # Pod env-pod, container env-pod
  SERVER=vcsa.home.local
  USER=admin@vsphere.local
  PASS=N0N5ecur3

  ```

  </details>


### Configmaps

### secrets





## Configurations

kubectl apply manifests/pod-envfrom-cm.yaml

```

### try as much as possible what could go wrong

wal@Bei:~/code/gitrepos/cert-magic$ kubectl get pods
NAME             READY   STATUS                       RESTARTS   AGE
configured-pod   0/1     CreateContainerConfigError   0          89s

### use events and describe pod to investiage errors

wal@Bei:~/code/gitrepos/cert-magic$ kubectl get events|grep  Warning
0s          Warning   Failed                    pod/configured-pod   Error: configmap "backend-config" not found



...
...
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  4m6s                  default-scheduler  Successfully assigned default/configured-pod to localhost
  Warning  Failed     113s (x12 over 4m7s)  kubelet            Error: configmap "backend-config" not found
  Normal   Pulled     98s (x13 over 4m7s)   kubelet            Container image "nginx:1.19.0" already present on machine



```

the fix?

```
wal@Bei:~/code/gitrepos/cert-magic$ kubectl apply -f manifests/backend-cm.yaml 
configmap/backend-config configured

wal@Bei:~/code/gitrepos/cert-magic$ kubectl exec  -it configured-pod -- env|grep data
database_url=jdbc:postgresql://localhost/test
database_user=saiyam
database_env=staging

```


## Secrets
![kh on secrets](pics/kh-secrets.png)
https://twitter.com/kelseyhightower/status/1324483384551239681
- []()

<details>
<summary>  Q1: Create a secret to hold the following govc credential, the secret name is called govc-creds, in name space vmug-cka, the secret should hold the following key value pairs:  server = vcsa.home.local, user =' administrator@vsphere.local', password = 'Admin!23', port = 443, insecure-flag = 1
	
</summary>
	
```
kubectl -n vmug-cka create secret generic govc-creds --from-literal=server=vcsa.home.local --from-literal=user='administrator@vsphere.local' --from-literal=password='Admin!23' --from-literal=port=443 --from-literal=insecure-flag=1 $DO
```
```	
apiVersion: v1
data:
  insecure-flag: MQ==
  password: QWRtaW4hMjM=
  port: NDQz
  server: dmNzYS5ob21lLmxvY2Fs
  user: YWRtaW5pc3RyYXRvckB2c3BoZXJlLmxvY2Fs
kind: Secret
metadata:
  creationTimestamp: null
  name: govc-creds
  namespace: vmug-cka

```
	
</details>

<details>
<summary>  Q2:
	use the kubectl --template option to print out the above secret govc-cred password key decoded in clear-text
</summary>
	
```
echo `kubectl get  secret govc-creds -n vmug-cka --template={{.data.password}}|base64 -d`
```
```	
Admin!23
```
	
```	
###*  the template argument was built based on the secret schema as you can see below if we use describe *###
kubectl describe -n vmug-cka secret govc-creds
Name:         govc-creds
Namespace:    vmug-cka
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
user:           27 bytes
insecure-flag:  1 bytes
password:       8 bytes
port:           3 bytes
server:         15 bytes


```
	
</details>

</details>

<details>
<summary>  Q:
	
</summary>
	
```

```
```	


```
	
</details>



### kubectl set   env --help
```
Examples:
  # Update deployment 'registry' with a new environment variable
  kubectl set env deployment/registry STORAGE_DIR=/local
  
  # List the environment variables defined on a deployments 'sample-build'
  kubectl set env deployment/sample-build --list
  
  # List the environment variables defined on all pods
  kubectl set env pods --all --list
  
  # Output modified deployment in YAML, and does not alter the object on the server
  kubectl set env deployment/sample-build STORAGE_DIR=/data -o yaml
  
  # Update all containers in all replication controllers in the project to have ENV=prod
  kubectl set env rc --all ENV=prod
  
  # Import environment from a secret
  kubectl set env --from=secret/mysecret deployment/myapp
  
  # Import environment from a config map with a prefix
  kubectl set env --from=configmap/myconfigmap --prefix=MYSQL_ deployment/myapp
  
  # Import specific keys from a config map
  kubectl set env --keys=my-example-key --from=configmap/myconfigmap deployment/myapp
  
  # Remove the environment variable ENV from container 'c1' in all deployment configs
  kubectl set env deployments --all --containers="c1" ENV-
  
  # Remove the environment variable ENV from a deployment definition on disk and
  # update the deployment config on the server
  kubectl set env -f deploy.json ENV-
  
  # Set some of the local shell environment into a deployment config on the server
  env | grep RAILS_ | kubectl set env -e - deployment/registry
```

### kubectl set   env --help
	
## Scheduling

### resource limits and requests
	
https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/#specify-a-cpu-request-and-a-cpu-limit

** Create a deployment of two replicas of nginx called prod01, enforcing container limits and request as follow max memory used by a container: 256Mi
    while max cpu 1/2 vcpu, minimal specs to run the nginx container as follow: memory: 100Mi and 100m cpu
    ensure that this runs on a node **vmug-cka-worker2**
**
	
### Node affinity

- Why control plane node does not have many pods as worker nodes?
  
  can you describe it and check for effective taints?
	
  can you run a daemonset across all nodes including control plane node?
  create a busybox sleep 1 hour daemonset across all nodes
	
	
** Add a label *env=prod* to one node and create nginx deployment called *prod* with 2 replicas and node Affinity rule to place the pods onto the labeled node.**

    <details><summary>show</summary><p>

    [Affinity and anti-affinity documentation](https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#affinity-and-anti-affinity)

        kubectl label node <node-name> env=prod	

    Generate your own yaml or use this one. There is something wrong with it(indentation or something is missing).

    ```YAML
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: blue
    spec:
      replicas: 5
      selector:
        matchLabels:
          run: nginx
    template:
        metadata:
          labels:
            run: nginx
        spec:
          containers:
          - image: nginx
            imagePullPolicy: Always
            name: nginx
          affinity:
            requiredDuringSchedulingIgnoredDuringExecution:
                nodeSelectorTerms:
                - matchExpressions:
                  - key: env
                    operator: In
                    values:
                    - prod
    ```

    Check that all pods are scheduled onto the labeled node.

    **Take-away**: Node affinity is a set of rules used by the scheduler to determine where a pod can be placed.


