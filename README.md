# nginx-ingress-controller-example

Assumption:
- You are having ruuning kuebernetes cluster

Steps:

1. Install nginx-ingress-controller

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx

helm repo update

helm install my-ingress-controller ingress-nginx/ingress-nginx

    # k get all
    NAME                                                                  READY   STATUS    RESTARTS   AGE
    pod/my-ingress-controller-ingress-nginx-controller-555b7dc97d-vv54g   1/1     Running   0          32m
    
    NAME                                                               TYPE           CLUSTER-IP      EXTERNAL-IP                                                             PORT(S)                      AGE
    service/kubernetes                                                 ClusterIP      10.100.0.1      <none>                                                                  443/TCP                      85m
    service/my-ingress-controller-ingress-nginx-controller             LoadBalancer   10.100.193.20   ac6ec634f409f4fe798aa797475f64d8-19581587.us-east-1.elb.amazonaws.com   80:30704/TCP,443:30207/TCP   77m
    service/my-ingress-controller-ingress-nginx-controller-admission   ClusterIP      10.100.231.81   <none>                                                                  443/TCP                      77m
    
    NAME                                                             READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/my-ingress-controller-ingress-nginx-controller   1/1     1            1           77m
    
    NAME                                                                        DESIRED   CURRENT   READY   AGE
    replicaset.apps/my-ingress-controller-ingress-nginx-controller-555b7dc97d   1         1         1       32m

2. Install sample helloworld application and create ingress for it

    kubectl apply -f sample.yaml
    
    
    deployment.apps/echo-server created
    service/echo-server created
    ingress.networking.k8s.io/echoserver-ing created

3. Verify ingress

          kubectl get ingress
          NAME             CLASS   HOSTS   ADDRESS                                                                 PORTS   AGE
          echoserver-ing   nginx   *       ac6ec634f409f4fe798aa797475f64d8-19581587.us-east-1.elb.amazonaws.com   80      48s
          
          # curl http://ac6ec634f409f4fe798aa797475f64d8-19581587.us-east-1.elb.amazonaws.com
          Hello, World!

![image](https://github.com/tushardashpute/nginx-ingress-controller-example/assets/74225291/706784a3-8da2-40f1-af89-1175e7cacd89)
