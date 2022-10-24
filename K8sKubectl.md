# K8s Kubeclt

| ***Date*** | 24-10-2022 |
| --- | --- |
| ***Author*** | Thanakorn P. |

Command kubectl

```zsh
kubectl create deploment nginx-depl --image=nginx

kubectl get replicaset

kubectl edit deployment

kubectl describe pod mongo-depl-8fbdb868c-5whzs

kubectl logs mongo-depl-8fbdb868c-5whzscl

kubectl exec -it mongo-depl-8fbdb868c-5whzs -- bin/bash

kubectl delete deployment mongo-depl

kubectl apply -f nginx-deployment.yaml
```

| CRUD command ||
| -- | -- |
| Create deployment | kubectl create deployment [name] |
| Edit deployment | kubectl edit deployment [name] |
| Delete deployment | kubectl delete deployment [name] |

### Status of different K8s components

kubectl get nodes | pod | services | replicaset | deployment

| Debugging pods||
| -- | -- |
| Log to console | kubectl logs [pod name] |
| Get interactive terminal | kubectl exec -it [pod name] -- bin/bash |
| Get info about pod | kubectl describe pod [pod name] |

| Use configuration file for CRUD||
| -- | -- |
| Apply a configuration file | kubectl apply -f [file name] |
| Delete with configuration file | kubectl delete -f [file name] |