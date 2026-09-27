<h1>How to install ARGO-CD on Kuberenetes Cluster</h1>

To install it, it's quite simple.
```
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/refs/tags/stable/manifests/ha/install.yaml --server-side
```
Then check that status of your pods:

I modified the section of the install.yml file that defines the "Service" resource for the Webserver "front-end/UI" to ArgoCD and added the "Type: LoadBalancer" key/value pair so that it would have an external IP automatically.
```
32786 ---
32787 apiVersion: v1
32788 kind: Service
32789 metadata:
32790   labels:
32791     app.kubernetes.io/component: server
32792     app.kubernetes.io/name: argocd-server
32793     app.kubernetes.io/part-of: argocd
32794   name: argocd-server
32795 spec:
32796   ports:
32797   - name: http
32798     port: 80
32799     protocol: TCP
32800     targetPort: 8080
32801   - name: https
32802     port: 443
32803     protocol: TCP
32804     targetPort: 8080
32805   selector:
32806     app.kubernetes.io/name: argocd-server
32807   Type: LoadBalancer
32808 ---
```

If that didn't work you can edit the ArgoCD Service resource for the webUI after it's up and running. Here you don't have to "add" the key/value pair - merely change it to "LoadBalancer"
```
kubectl edit svc -n argocd argocd-server
```
To get a login token for the ARGO-CD WebUI:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
