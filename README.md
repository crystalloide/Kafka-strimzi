## Etapes de la démo :
 
 ### 1. La première étape consiste à déployer l'opérateur Strimzi :

     kubectl create -f 'https://strimzi.io/install/latest?namespace=default' -n default

 ### 2. Une fois que l'opérateur est "Ready", on va déployer la Custom Resource Définition de notre cluster Kafka nommée `kafkaTLS.yaml` : 

     kubectl create -f kafkaTls.yaml -n default

### 3. Une fois que le cluster Kafka est déployé, on applique les custom ressources définitions nommées `user` et `topic` :

*    Pour "user" :

      ```sh
       kubectl create -f user.yaml -n default
      ```

*    Pour "topic" :

      ```sh
       kubectl create -f topic.yaml -n default
      ```


### 4. Une fois ces étapes effectuées, on applique les fichiers de déploiement du consumer et du producer :
    
*    Pour le "consumer" :
    
     ```sh
      kubectl create -f consumer.yaml -n default
     ```

*    Pour le "Producer" : 

     ```sh
      kubectl create -f hello-world.yaml -n default
     ```

### 5. Regardons les pods :  

   ```sh
      kubectl get pods
   ```
     
Affichage des pods : 

    NAME                                          READY   STATUS    RESTARTS   AGE
    hello-world-consumer-8c94685d-jlb97           1/1     Running   0          8m56s
    hello-world-producer-5cb7d54cb6-z74hw         1/1     Running   0          8m41s
    my-cluster-entity-operator-7df45b6b78-qrwpx   2/2     Running   0          13m
    my-cluster-kafka-0                            1/1     Running   0          14m
    my-cluster-kafka-1                            1/1     Running   0          14m
    my-cluster-kafka-2                            1/1     Running   0          14m
    my-cluster-zookeeper-0                        1/1     Running   0          15m
    my-cluster-zookeeper-1                        1/1     Running   0          15m
    my-cluster-zookeeper-2                        1/1     Running   0          15m
    strimzi-cluster-operator-86df856f74-bcpfn     1/1     Running   0          19m


### 6. Regardons les services :  
   ```sh
      kubectl get services
   ```
     
Affichage des services : 

    NAME                          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                               AGE
    kubernetes                    ClusterIP   10.96.0.1       <none>        443/TCP                               34m
    my-cluster-kafka-bootstrap    ClusterIP   10.96.244.54    <none>        9091/TCP,9093/TCP                     16m
    my-cluster-kafka-brokers      ClusterIP   None            <none>        9090/TCP,9091/TCP,8443/TCP,9093/TCP   16m
    my-cluster-zookeeper-client   ClusterIP   10.96.191.190   <none>        2181/TCP                              17m
    my-cluster-zookeeper-nodes    ClusterIP   None            <none>        2181/TCP,2888/TCP,3888/TCP            17m


### 7. Regardons les logs du producer : 

   ```sh
kubectl logs hello-world-producer-5cb7d54cb6-z74hw
   ```
  A savoir : on peut ajouter "--follow" à la commande ci-dessus pour rester et suivre les logs

### 8. Et regardons ensuite dans les logs du consumer pour voir que les messages issus du producer sont bien consommés par le consumer ...

   ```sh
kubectl logs hello-world-consumer-8c94685d-jlb97 
   ```
 






