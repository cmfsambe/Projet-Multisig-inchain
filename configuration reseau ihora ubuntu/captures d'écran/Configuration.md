#Installation et configuration de Iroha by Hyperledger


##Installation de docker
~~~
sudo apt-get install curl
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
apt-cache policy docker-ce
sudo apt-get install -y docker-ce
~~~

##Installation du réseau Docker pour Iroha 
~~~
sudo docker network create reseau-iroha
~~~

##Ajout de Postgres (port 5432)

~~~
sudo docker run --name postgres \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=mysecretpassword \
-p 5432:5432 \
--network=reseau-iroha \
-d postgres:9.5
~~~

##Création d'un volume de stockage persistant nommé "blockstore" pour storer les blocks sur notre blockchain.

~~~
sudo docker volume create blockstore
~~~


##Configurer Iroha sur le réseau. telecharger le code à partir de la branche develop du repo github Ihora open source project (installer git si nécessaire)

~~~
sudo apt-get install git
git clone -b develop https://github.com/hyperledger/iroha --depth=1
~~~

##Initialisation du Genesis block

~~~
sudo docker run -it --name iroha \
-p 50051:50051 \
-v $(pwd)/iroha/example:/opt/iroha_data \
-v blockstore:/tmp/block_store \
--network=reseau-ihora \
--entrypoint=/bin/bash \
hyperledger/iroha:x86_64-develop
~~~

Lancer le conteneur Iroha

~~~
irohad --config config.docker --genesis_block genesis.block --keypair_name node0
~~~
