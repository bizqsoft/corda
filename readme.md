

<div align="center">
  <h3 align="center">Corda docker deploy on aws ec2 </h3>
</div>

<!-- GETTING STARTED -->
## Getting Started

This is an example of how you may give instructions on setting up your project locally.
To get a local copy up and running follow these simple example steps.


### Deploy corda by using docker compose on notary and partya node.

1. Clone deploy corda package repo
```sh
git clone https://github.com/bizqsoft/corda.git
```
2. Deploy Node : Notary and Party A ,config network nod by naviget to notary ,partya then edit file node.conf to change node name and node ip:
```sh
myLegalName="O=PartyA,L=London,C=GB"
p2pAddress="3.95.222.23:10005"
```
3. Copy deploy file to aws server Node A
```sh
scp -i "./corda-docker.pem" -r ./deploy/nodeA ubuntu@3.95.222.23:~/
```
```sh
scp -i "./corda-docker.pem" -r ./deploy/commands.sh ubuntu@3.95.222.23:~/
```
```sh
scp -i "./corda-docker.pem" -r ./deploy/nodeA/notary/corda.jar ubuntu@3.95.222.23:~/
```
4. Access ssh console aws NodeA
```sh
ssh -i "corda-docker.pem" ubuntu@ec2-3-95-222-23.compute-1.amazonaws.com
```
5. run set up server shell script file command.
```sh
cd ~
chmod a+x ./commands.sh
./commands.sh
```
6. run database migration script.
```sh
cd /nodeA/notary
java -jar corda.jar run-migration-scripts --core-schemas --app-schemas --allow-hibernate-to-manage-app-schema
```
```sh
cd /nodeA/partyA
java -jar corda.jar run-migration-scripts --core-schemas --app-schemas --allow-hibernate-to-manage-app-schema
```
Run corda server by docker
```sh
cd /nodeA
sudo chown $(whoami) -R ./*
docker-compose -f ./docker-compose-vm1.yaml up
```
Run corda server by manual

```sh
cd /nodeA/notary
java -jar corda.jar
cd /nodeA/partya
java -jar corda.jar
```

Run corda server by script

```sh
./build/node/runnodes
```

Run Spring Webserver
```sh
./gradlew runPartyAServer
```

### Deploy corda by using docker compose on PartyB node.


1. Clone deploy corda package repo
```sh
git clone https://github.com/bizqsoft/corda.git
```
2. Deploy Node : Party B ,config network nod by naviget to partyb then edit file node.conf to change node name and node ip:
```sh
myLegalName="O=PartyB,L=New York,C=US"
p2pAddress="3.93.6.36:10005"
```
3. Copy deploy file to aws server Node B
```sh
scp -i "./corda-docker.pem" -r ./deploy/nodeB ubuntu@3.93.6.36:~/
```
```sh
scp -i "./corda-docker.pem" -r ./deploy/commands.sh ubuntu@3.93.6.36:~/
```
```sh
scp -i "./corda-docker.pem" -r ./deploy/nodeB/notary/corda.jar ubuntu@3.93.6.36:~/
```
4. Access ssh console aws NodeB
```sh
ssh -i "corda-docker.pem" ubuntu@ec2-3-93-6-36.compute-1.amazonaws.com
```
5. run set up server shell script file command.
```sh
cd ~
chmod a+x ./commands.sh
./commands.sh
```
6. run database migration script.
```sh
cd /nodeB/partyb
java -jar corda.jar run-migration-scripts --core-schemas --app-schemas --allow-hibernate-to-manage-app-schema
```

Run corda server by docker
```sh
cd /nodeB
sudo chown $(whoami) -R ./*
docker-compose -f ./docker-compose-vm2.yaml up
```
Run corda server by manual

```sh
cd /nodeB/partyb
java -jar corda.jar
```

Run corda server by script

```sh
./build/node/runnodes
```

Run Spring Webserver
```sh
./gradlew runPartyBServer
```

<p align="right">(<a href="#top">back to top</a>)</p>

