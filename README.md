# prime-grpc-scala

## Deploy
1. Install minikube
```
$ ./deploy/scripts/setup-minikube-for-linux.sh
```

2. Publish docker images
```
$ sbt prime-generator/docker:publishLocal -Ddocker.username=iamsmkr -Ddocker.registry=index.docker.io
$ sbt prime-proxy-impl/docker:publishLocal -Ddocker.username=iamsmkr -Ddocker.registry=index.docker.io
```

3. Create application secret
```
$ kubectl create secret generic application-secret --from-literal=secret="$(openssl rand -base64 48)"
```

4. Apply k8s manifests
```
kubectl apply -f deploy/k8s/config-map.yml
kubectl apply -f deploy/k8s/prime-generator.yml
kubectl apply -f deploy/k8s/prime-proxy.yml
```

## Usage
```
curl --header 'Host: primeservice.com' $(sudo -E minikube ip)/prime/23
```
