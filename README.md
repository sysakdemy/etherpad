# Etherpad

https://docs.etherpad.org/docker.html

## Kubernetes

* Créer préalablement le namespace **pad**

```bash
kubectl create ns pad
```

* Créer le secret **padusers** (remplacer adminpassword et userpassword par les mots de passe)

```bash
cat << EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: padusers
  labels:
    app: etherpad
  namespace: pad
data:
  adminPassword: $(echo "adminpassword" |base64)
  userPassword: $(echo "userpassword" |base64)
type: Opaque
EOF
```

* Déployer ensuite les manifest

``` bash
cd manifest
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
kubectl apply -f deployment.yaml
```

## Docker

``` bash
docker run -it --rm -p 80:9001 \
-e 'ADMIN_PASSWORD=secret' \
-e 'USER_PASSWORD=usersecret' \
-e 'REQUIRE_AUTHENTICATION=true' \
-e 'MINIFY=true' \
-e 'EXPOSE_VERSION=false' \
etherpad/etherpad:2.6.1
```