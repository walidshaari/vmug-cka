# cert-magic - episode 7

## Tips

- Always remmber your context and namespace
- Use autocompletion
- 

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