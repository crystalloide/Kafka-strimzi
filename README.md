## Etapes de la démo :
 
 ### 1. La première étape consiste à déployer l'opérateur Strimzi :

     kubectl create -f 'https://strimzi.io/install/latest?namespace=myproject' -n myproject

 ### 2. Une fois que l'opérateur est "Ready", on va déployer la Custom Resource Définition de notre cluster Kafka nommée `kafkaTLS.yaml` : 

     kubectl create -f kafkaTls.yaml -n myproject

### 3. Une fois que le cluster Kafka est déployé, on applique les custom ressources définitions nommées `user` et `topic` :

*    Pour "user" :

      ```sh
       kubectl create -f user.yaml -n myproject
      ```

*    Pour "topic" :

      ```sh
       kubectl create -f topic.yaml -n myproject
      ```


### 4. Une fois ces étapes effectuées, on applique les fichiers de déploiement du consumer et du producer :
    
*    Pour le "consumer" :
    
     ```sh
      kubectl create -f consumer.yaml -n myproject
     ```

*    Pour le "Producer" : 

     ```sh
      kubectl create -f hello-world.yaml -n myproject
     ```

### 5. Regardons les pods :  

   ```sh
      kubectl get pods -n myproject
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
      kubectl get services -n myproject
   ```
     
Affichage des services : 

    NAME                          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                               AGE
    my-cluster-kafka-bootstrap    ClusterIP   10.96.244.54    <none>        9091/TCP,9093/TCP                     16m
    my-cluster-kafka-brokers      ClusterIP   None            <none>        9090/TCP,9091/TCP,8443/TCP,9093/TCP   16m
    my-cluster-zookeeper-client   ClusterIP   10.96.191.190   <none>        2181/TCP                              17m
    my-cluster-zookeeper-nodes    ClusterIP   None            <none>        2181/TCP,2888/TCP,3888/TCP            17m


### 7. Regardons les logs du producer : 

   ```sh
kubectl -n myproject logs hello-world-producer-5cb7d54cb6-z74hw
   ```
  A savoir : on peut ajouter "--follow" à la commande ci-dessus pour rester et suivre les logs

### 8. Et regardons ensuite dans les logs du consumer pour voir que les messages issus du producer sont bien consommés par le consumer ...

   ```sh
kubectl -n myproject logs hello-world-consumer-8c94685d-jlb97 
   ```
 
### 9. Une fois le test fini, suppression du cluster Apache Kafka

   ```sh
kubectl -n myproject delete $(kubectl get strimzi -o name -n myproject) 
   ```
Cela supprimera toutes les ressources personnalisées Strimzi, 

y compris le cluster Apache Kafka et toutes les ressources personnalisées KafkaTopic

mais laissera par contre l'opérateur de cluster Strimzi fonctionner 

afin qu'il puisse répondre aux nouvelles demandes de ressources personnalisées Kafka.

### 10. Suppression de l'opérateur de cluster Strimzi :

Pour supprimer complètement l'opérateur de cluster Strimzi et les définitions associées :

   ```sh
kubectl -n myproject delete -f 'https://strimzi.io/install/latest?namespace=myproject'
   ```

### Have Fun :-)

