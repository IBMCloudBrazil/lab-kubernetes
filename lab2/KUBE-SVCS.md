
Overview

Use kubectl explain to see available keys/fields for a service manifest.

kubectl explain service --recursive=true
kubectl explain service.spec.ports --recursive=true

ClusterIP Services

Using the whoami deployment from the previous exercise, expose the deployment with a ClusterIP service.

kubectl expose deployment whoami --port 80 --target-port 80

View the basic information for the Service resource.

kubectl get svc whoami -o wide

Next, view the detail for the Service resource, notice the Endpoints.

kubectl describe svc whoami

Run a separate pod to access the service.

kubectl run busybox -it --rm --image busybox

Environment

Wait for the command prompt and view the environment variables present in the container.

env
env | grep WHOAMI

Verify that the service can be accessed using the IP address specified in the environment variable.

wget -qO - $WHOAMI_SERVICE_HOST

DNS

Verify that the service can be accessed with a hostname using DNS.

nslookup whoami
wget -qO - whoami

Exit out of the busybox pod.

exit

NodePort Services

Define a service that is accessible via a port listening on every node in the cluster (a range of 30000-31000).

cat > service-nodeport.yaml <<EOF
kind: Service
apiVersion: v1
metadata:
  name: service-nodeport
spec:
  type: NodePort
  selector:
    run: whoami
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
EOF

Create the service based on the manifest that we just created.

kubectl create -f service-nodeport.yaml

View the detail for the service.

kubectl describe svc service-nodeport

Access the service from within the cluster.

kubectl run busybox -it --rm --image busybox --restart Never -- wget -qO - http://service-nodeport

External Access

Attempt to curl the external IP address of one of the nodes on the NodePort.

If you are in an AWS environment, a custom TCP rule for the NodePort range of 30000-32767 will need to be set for the master security-group.
Master Node

Find the master nodes in the cluster.

kubectl get nodes -o wide -l node-role.kubernetes.io/master

Save the external (public) IP address for the first master node in an environment variable.

MASTER_NODE=$(kubectl get nodes -l node-role.kubernetes.io/master -o jsonpath={.items[0].status.addresses[?(@.type == \"ExternalIP\")].address})
echo $MASTER_NODE

NodePort

Save the NodePort to an environment variable.

NODE_PORT=$(kubectl get svc service-nodeport -o jsonpath={.spec.ports[*].nodePort})
echo $NODE_PORT

Access Service

Use the wget utility to access the service using the MASTER_NODE and NODE_PORT environment variables that we set.

wget -qO - $MASTER_NODE:$NODE_PORT

ExternalName Services

Define a service that is an alias to an external resource residing outside the cluster.

cat > service-external.yaml <<EOF
kind: Service
apiVersion: v1
metadata:
  name: ucanhazip
spec:
  type: ExternalName
  externalName: icanhazip.com
EOF

Create the service based on the manifest that we just created.

kubectl create -f service-external.yaml

View the detail for the service.

kubectl describe -f service-external.yaml

Run another pod to access the external service.

kubectl run alpine -it --rm --image alpine --restart Never -- wget -qO - http://ucanhazip

Clean Up

Clean up the services and deployment that we created.

kubectl delete svc whoami service-nodeport ucanhazip
kubectl delete deploy whoami

View any remaining resources.

kubectl get deploy,pod,svc

