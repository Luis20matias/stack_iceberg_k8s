Here is an example of setting up Minio service using Metallb in Minikube:

Enable Metallb Addon:

```bash
minikube addons enable metallb
```

Apply Metallb ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: default
      protocol: layer2
      addresses:
      - 192.168.49.240-192.168.49.250
```


```bash
kubectl apply -f metallb-config.yaml
```

Deploy Minio Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: minio
spec:
  selector:
    app: minio
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9000
  type: LoadBalancer
```

```bash
kubectl apply -f minio-service.yaml
```

Run Minikube Tunnel:

```bash
minikube tunnel
```

By following these steps, you should be able to access your Minio service without encountering the connection refused error.

Link describing how to enable ingress: https://dev.to/olgabraginskaya/unleash-your-pipeline-creativity-local-development-with-argo-workflows-and-minio-on-minikube-2764