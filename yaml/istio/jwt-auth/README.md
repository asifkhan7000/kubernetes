Before you start, run the following command to install python dependences.

```
pip install jwcrypto
```
Regenerate private key and JWKS (for developer use only)
 1)Regenerate private key using openssl
openssl genrsa -out key.pem 2048
2) Run gen-jwt.py with --jkws to create new public key set and demo JWT
    gen-jwt.py key.pem -jwks=./jwks.json --expire=3153600000 

### Verifying #####
1) kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/shopping-ns.yaml
2)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/jwt-auth/shopping-tls-enable.yaml
3)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/jwt-auth/shoppingportal-jwt-enable.yaml
4)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/jwt-auth/product-v1.yaml
5)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/product/product-service.yaml
6)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/jwt-auth/product-destination.yaml
7)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/jwt-auth/shoppingportal-gw.yaml
8)kubectl create -f https://raw.githubusercontent.com/meta-magic/kubernetes_workshop/master/yaml/istio/jwt-auth/shoppingportal-virtualservice-jwt.yaml


curl http://35.238.229.131//productms/check/live -s -o /dev/null -w "%{http_code}\n"
 TOKEN=$(TOKEN=$(./gen-jwt.py key.pem --expire=300 --iss "testing@secure.istio.io")


 curl --header "Authorization: Bearer $TOKEN" http://35.238.229.131/productms/check/live -s -o /dev/null -w "%{http_code}\n"

