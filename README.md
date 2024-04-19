## Etapes de la démo :
 
 ### 1. La première étape consiste à déployer l'opérateur Strimzi :

     kubectl create -f 'https://strimzi.io/install/latest?namespace=default' -n default

 ### 2. Une fois que l'opérateur est "Ready", on va déployer le Kafka custom resource apprlé `kafkaTLS.yaml` : 

     kubectl create -f kafkaTls.yaml -n default

### 3. Une fois que le cluster Kafka est déployé, on applique les custom ressources nomées `user` et `topic` :

*    Pour "user" :

      ```sh
       kubectl create -f user.yaml -n default
      ```

*    Pour "topic" :

      ```sh
       kubectl create -f topic.yaml -n default
      ```


### 4. Une fois les étapes précédents effectuées, on applique les fichiers de déploiement du consumer et du producer :
    
*    Pour le "consumer" :
    
     ```sh
      kubectl create -f consumer.yaml -n default
     ```

*    Pour le "Producer" : 

     ```sh
      kubectl create -f hello-world.yaml -n default
     ```

Et on peut ensuite regarder dans les logs du consumer et vérifier que les messages issus du producer sont bien consommés par le consumer ...
