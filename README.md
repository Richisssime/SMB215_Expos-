# Projet Smb215 2021-2022

----------------------------------------

Dans le cadre de l'UE SMB215, nous allons mettre en place mettre en place un réseau hyperledger Fabric, pour montrer le fonctionnement des blockchains privées.

J'ai utilisé une machine virtuelle Ubuntu 20.04.4

--------------------------------------------------------------------

### Augmenter les privilèges entant que root pour avoir tous les droits en écriture 
`sudo -i`

### Se déplacer sur la racine de linux  
`cd /`

--------------------------------------------------------------------
## Installer l'environnement de Hyperledger Fabric
---  

### Installer Git  
Installer la dernière version de Git si elle n'est pas déjà installé  
`apt-get install git`  

### Installer cURL  
Installer la dernière version de cURL si elle n'est pas déjà installé 
`apt-get install curl`   

### Installer Docker     
Installer la dernière version de Docker si elle n'est pas déjà installé 
`apt-get -y install docker-compose`    
  
Assurez-vous que le démon Docker est en cours d'exécution
`sudo systemctl start docker`  
  
Facultatif : Si vous souhaitez que le démon Docker démarre au démarrage du système, utilisez ce qui suit :
`sudo systemctl enable docker`

### Installer le module "express"
`npm install express body-parser --save`

### Installer nodejs
`apt install nodejs`

### Installer npm
`apt install npm`

--------------------------------------------------------------------
## Télécharger des exemples Fabric, des images Docker et des fichiers binaires
---
### Créé un dossier nommé "fabric"    
`mkdir fabric`

### Se déplacer dans le dossier "fabric"  
`cd fabric` 

### Télécharger le paquet Fabric-sample fournit par hyperledger 
`git clone https://github.com/hyperledger/fabric-samples.git`

--------------------------------------------------------------------
## Déploiement du réseau Fabcar
---
### se déplacer dans le dossier "fabcar" forunit par fabric
`cd fabric-samples/fabcar`

### Télécharger l'api sur Github
`git clone https://github.com/thomastibin/apiserver.git`

### Copie le dossier javascript dans le dossier apiserver
`cp -r javascript/ apiserver/`

### Se déplacer dans le dossier "apiserver"
`cd apiserver`

### Repartir dans le répertoire "fabcar"
`cd ..`
--------------------------------------------------------------------
### Déployer le réseau Fabric avec le script fournit par Hyperledger   
Avant de lancer le script du déploiement du réseau Fabcar, il faut se rassurer qu'aucun réseau est déjà lancé en exécutant le script 
`./networkDown.sh`

Puis éxécuter le script qui permet de déployer le réseau fabric sur notre ordinateur
`./startFabric.sh`

![image](https://user-images.githubusercontent.com/93784527/172252052-bca2e3b7-41a8-4b6b-a75c-e63a2728e573.png)


### Se déplacer dans le répertoire "apiserver"   
`cd apiserver/`

### Enroler l'utilisateur admin "admin" et importer dans le wallet  
`node enrollAdmin.js`

![image](https://user-images.githubusercontent.com/93784527/172252135-10740179-7945-4e5d-a952-9422743cf0d5.png)


### enregistrement et inscription de l'utilisateur admin "appUser" et importation dans le portefeuille  
`node registerUser.js`

![image](https://user-images.githubusercontent.com/93784527/172252154-69cf77e6-38cd-459c-83af-b63d4a072bee.png)


### lancer l'api serveur (fichier "apiserver.js") se trouvant dans le dossier "apiserver"   
`nodemon apiserver.js`

![image](https://user-images.githubusercontent.com/93784527/172252224-07684434-334d-4e9e-941c-a03227c56777.png)


### Ouvrir un autre terminal

### lancer l'application avec Curl en lançant une requète http  
Pour afficher toutes les voitures enregistrées sur le réseau  
`curl http://localhost:8080/api/queryallcars` 

![image](https://user-images.githubusercontent.com/93784527/172252277-b456865d-08fc-4b20-997a-306153c754f0.png)


### Pour afficher les information d'une voiture (ex: CAR3)
http://localhost:8080/api/query/CAR3


### lancer Postman pour envoyer les requête HTTP sur le serveur 

### Pour ajouter une voiture à la blockchain (ici la voiture 10)   
D'abord se mettre en methode http "POST"
http://localhost:8080/api/addcar

Dans le corp de la requètte
{
    "carid":"CAR10",
    "make":"Tesla",
    "model":"X Plaid",
    "colour":"Noire",
    "owner":"OSSEBI"
}

![image](https://user-images.githubusercontent.com/93784527/172251838-8d59114c-8583-4b2b-829e-d3f8e55b4e07.png)

![image](https://user-images.githubusercontent.com/93784527/172251910-8fd63ed1-a38d-4470-9f39-49c358ba8eb2.png)



## Erreurs 
--------------------------------------------------------------------
Ici nous allons lister quelques erreurs rencontrées 

### 1
internal/modules/cjs/loader.js:638
    throw err;
    ^
### Solution:
Mettre à jour npm
`npm update`

### 2
nodemon: command not found
### Solution:  
installer nodemon  
`npm install nodemon -g --save`  

### 3
Après un certain temps d'inactivité sur le réseau, il faut se réenroler il faut donc suprimer les fichier d'enrollement de la précédent connexion
`rm /fabric/fabric-samples/fabcar/apiserver/wallet/admin.id`  
`rm /fabric/fabric-samples/fabcar/apiserver/wallet/appUser.id` 
