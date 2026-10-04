# Evaluación Parcial II

## Paso 1: Instalación de MetalLB | Definicion de IP MetalLB

* Instalación

```
> mkdir metallb && curl -L -o metallb/metallb-native.yaml \ https://raw.githubusercontent.com/metallb/metallb/v0.16.1/config/manifests/metallb-native.yaml

> k apply -f metallb/metallb-native.yaml
```

![](images/metallb-install.png)

* Definicion de IP

metallb/metallb-config.yaml
```
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: single-ip-pool
  namespace: metallb-system
spec:
  addresses:
    - 192.168.49.240/32
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: l2-adv
  namespace: metallb-system
spec:
  ipAddressPools:
    - single-ip-pool
```

### IP Definida: 192.168.49.240/32

Comprobacion y aplicacion:
![](images/ip-definition.png)

## Paso 2: Configuracion Traefik

* Configuracion de Traefik

traefik/traefik-values.yaml
```
service:
  type: LoadBalancer
  annotations:
    metallb.io/loadBalancerIPs: 192.168.49.240
ingressClass:
  enabled: true
  isDefaultClass: true
providers:
  kubernetesIngress:
    enabled: true
  kubernetesCRD:
    enabled: true
```

* Instalacion y comprobacion

![](images/traefik-install.png)

## Paso 3: Configuracion de namespace "parcial-ceva"

Servicios:
* nginx *(apps/app1-nginx.yaml)*
* apache *(apps/app2-apache.yaml)*
* whoami *(apps/app3-whoami.yaml)*
* http-echo *(apps/app4-http-echo.yaml)*

### Aplicar los servicios:

![](images/services-1.png)

![](images/services-2.png)


## Paso 4: Configuracion de Ingress

**Ruta Ingress:** apps/ingress.yaml

![](images/ingress-config.png)

* Escritura en /etc/hosts

![](images/hosts.png)

## Paso 5: Pruebas finales

### Verificación de dominios con port-forwarding

```
> kubectl port-forward -n traefik svc/traefik 8080:80 & sleep 2
```

* nginx
![](images/nginx.png)
* apache
![](images/apache.png)
* whoami
![](images/whoami.png)
* http-echo
![](images/echo.png)

Llamada a Nginx desde Ubuntu con "curl"

![](images/curl.png)
