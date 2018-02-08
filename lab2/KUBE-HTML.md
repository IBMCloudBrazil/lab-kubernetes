# Laboratório 2.1 Kubernetes Página HTML

1. Crie uma página HMTL
```
cat > intro.html <<EOF
<html>
  <head><title>Hello K8s</title></head>
  <body>
    <h1>Your Name Here</h1>
    <h2>Your Company</h2>
    <h3>Your Role</h3>
    <p>Describe your experience with containers</p>
    <p>Describe your experience with Kubernetes</p>
    <p>What I hope to learn in this class:</p>
    <ul>
      <li>Tell us what you hope to learn here</li>
      <li>What else?</li>
      <li>Don't be shy, what else?</li>
    </ul>
  </body>
</html>
EOF
```

2. Salve o conteúdo da sua página HMTL em um ConfigMap no Kubernetes
```
kubectl create configmap nginx-config --from-literal index.html="`cat intro.html`"
```

3. Visualize o conteúdo salvo no Kubernetes
```
kubectl describe configmap nginx-config
```

4. Crie um arquivo para descrever o deployment dessa página no Kubernetes
```
cat > intro-app.yaml <<EOF
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: intro-app
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: intro-app
    spec:
      containers:
        - name: intro-app
          image: nginx:1.13
          ports:
          - containerPort: 80
          volumeMounts:
          - mountPath: /usr/share/nginx/html
            name: config-volume
      volumes:
        - name: config-volume
          configMap:
            name: nginx-config
---
apiVersion: v1
kind: Service
metadata:
  name: intro-app
spec:
  type: NodePort
  selector:
    app: intro-app
  ports:
    - port: 80
      protocol: TCP
      targetPort: 80
EOF
```

5. Execute o deployment a partir do arquivo criado
```
kubectl create -f intro-app.yaml
```

