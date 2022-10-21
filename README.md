# projet-test-azure-cloud

  chmod 400 UbuntuVM_key.pem  
  ssh -i UbuntuVM_key.pem azureuser@(ip)  
  ssh -p (port) -i UbuntuVM_key.pem azureuser@(ip) dans le dossier de la key.pm  
  git clone (lien git du projet)  
  se mettre sur le dossier du projet et faire : docker-compose up  
  Back : OK : http://20.19.191.70:5000/  
  FRONT : KO  

## Machine Virtuelle 

![image](vue-d-ensemble.png)

## Configuration IP

![image](config-ip.png)

## Networking Ports

![image](network.png)

## Déployer des containers 

  se connecter au registre de container : az acr login --name containerregistry8  
  modifier le docker-compose.yml pour que l'image puisse être push  
  Avant
  ```
  app:
    build: ./app
    image: roberto8/projet-docker-app
    links:
      - db
    ports:
      - "5000:5000"
    healthcheck:
      test: curl --fail -s http://localhost:5000/ || exit 1
      interval: 1m30s
      timeout: 10s
      retries: 3
    networks:
      - frontend
      - backend
  ```
  Après
  ```
  app:
    build: ./app
    image: containerregistry8.azurecr.io/projet-docker-app
    links:
      - db
    ports:
      - "5000:5000"
    networks:
      - frontend
      - backend
  ```
  Push les images avec docker compose : docker-compose push  
  Vérifier que l'image est stockée : sudo az acr repository show --name containerregistry8 --repository projet-docker-app  
  Créer un context : docker login azure, docker context create aci myacicontext, docker context ls  
  Utiliser un context : docker context use myacicontext  
  Lancer les containers : docker compose up (sans le tiret)


