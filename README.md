# store-products

```sh
oc new-app mongodb -l app=mongodb --name=productsdb \
  -e MONGODB_ADMIN_PASSWORD=password  -e MONGODB_USER=app_user \
  -e MONGODB_DATABASE=store  -e MONGODB_PASSWORD=password
  
oc new-app https://github.com/i63/store-products --name=products

oc env dc products MONGO_USER=app_user MONGO_PASSWORD=password MONGO_SERVER=productsdb MONGO_PORT=27017 MONGO_DB=store \
mongo_url='mongodb://app_user:password@productsdb/store'

```


## CI-CD
```
cat > config.json <<EOF
{
  "auths": {
     "https://index.docker.io/v1/": {
		"auth": "base64encoded_password_here" 
     }
  }
} 
EOF
```
```
kubectl create secret generic docker-reg --from-file=config.json
```

> Build pod needs to mount secret at /root/.docker/config.json
