
#Overview

Use kubectl explain to see available keys/fields for a deployment manifest.
```
kubectl explain deployment --recursive=true
kubectl explain deployment.spec.template --recursive=true
```

Create a deployment to bring up 3 “whoami” pods.
```
kubectl run whoami --image quay.io/john_mckenzie/hello-whoami:2 --image-pull-policy Always --port 80 --replicas 3
```

#Deployment

Once the Deployment is created, view the status of the Rollout.
```
kubectl rollout status deployment/whoami
```

View the basic information for the Deployment resource.
```
kubectl get deployment whoami -o wide
```

Next, view the detail for the Deployment resource, notice the ReplicaSet that was created.
```
kubectl describe deployment whoami
```

View the manifest for the Deployment resource.
```
kubectl get deployment whoami -o yaml
```

#ReplicaSet

View the basic information for the ReplicaSet resource that was created by the deployment.
```
kubectl get replicaset -l run=whoami -o wide
```

Next, view the detail for the ReplicaSet resource, notice the pods that were created.
```
kubectl describe replicaset -l run=whoami
```

Finally, view the manifest for the ReplicaSet resource.
```
kubectl get replicaset -l run=whoami -o yaml
```

#Pods

View the Pods that were created by the deployment.
```
kubectl get pods -o wide -l run=whoami --show-labels
```

Run a separate pod to access the nginx pods via the cluster IP address.
```
kubectl run busybox --rm -it --image busybox
```

Wait for the command prompt and attempt to access the Pods.

```
ping -c 3 <POD IP>
wget -qO - http://<POD IP>
```

Leave this deployment running, as we will use it in the following exercises.
