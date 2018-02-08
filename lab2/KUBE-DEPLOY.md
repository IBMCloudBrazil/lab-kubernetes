
# Visão geral

Use o kubectl para ver as chaves / campos disponíveis para um manifesto de implantação.
```
kubectl explain deployment --recursive=true
kubectl explain deployment.spec.template --recursive=true
```

Crie uma implantação para criar 3 "whoami" pods.
```
kubectl run whoami --image quay.io/john_mckenzie/hello-whoami:2 --image-pull-policy Always --port 80 --replicas 3
```

# Deployment (Implantação)

Uma vez que a Implantação é criada, veja o status do Rollout.
```
kubectl rollout status deployment/whoami
```

Veja as informações básicas para o recurso de implantação.
```
kubectl get deployment whoami -o wide
```

Em seguida, veja o detalhe do recurso de implantação, observe o ReplicationSet que foi criado.
```
kubectl describe deployment whoami
```

Veja o manifesto para o recurso de implantação.
```
kubectl get deployment whoami -o yaml
```

# ReplicaSet

Veja as informações básicas para o recurso ReplicaSet que foi criado pela implantação.
```
kubectl get replicaset -l run=whoami -o wide
```

Em seguida, veja os detalhes para o recurso ReplicaSet, observe os pods que foram criados.
```
kubectl describe replicaset -l run=whoami
```

Finalmente, ver o manifesto para o recurso ReplicaSet.
```
kubectl get replicaset -l run=whoami -o yaml
```

# Pods

Veja os Pods que foram criados pela implantação.
```
kubectl get pods -o wide -l run=whoami --show-labels
```

Execute um pod separado para acessar os canais nginx através do endereço IP do cluster.
```
kubectl run alpine --rm -it --image alpine
```

Aguarde o prompt de comando e tente acessar os Pods.

```
ping -c 3 <POD IP>
wget -qO - http://<POD IP>
```

Deixe esta implantação em execução, pois a utilizaremos nos seguintes exercícios.
