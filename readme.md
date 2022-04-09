
#Install corda by Docker compose
to clone deploy corda package :

git clone https://github.com/bizqsoft/corda.git

Deploy Node : Notary and Party A

config network nod : partya/node.conf and change nodename and node ip:

myLegalName="O=PartyA,L=London,C=GB"

p2pAddress="3.95.222.23:10005"

config network nod : notary/node.conf and change nodename and node ip:

myLegalName="O=PartyA,L=London,C=GB"

p2pAddress="3.95.222.23:10005"

1.) Copy deploy file to aws server Node A

scp -i "./corda-docker.pem" -r ./deploy/nodeA ubuntu@3.95.222.23:~/

scp -i "./corda-docker.pem" -r ./deploy/commands.sh ubuntu@3.95.222.23:~/

scp -i "./corda-docker.pem" -r ./deploy/nodeA/notary/corda.jar ubuntu@3.95.222.23:~/

2) Access ssh console aws NodeA
ssh -i "corda-docker.pem" ubuntu@ec2-3-95-222-23.compute-1.amazonaws.com

3) run set up Server shell script file command.
cd ~
chmod a+x ./commands.sh
./commands.sh

4) run database migration script

cd /nodeA/notary

java -jar corda.jar run-migration-scripts --core-schemas --app-schemas --allow-hibernate-to-manage-app-schema

cd /nodeA/partyA

java -jar corda.jar run-migration-scripts --core-schemas --app-schemas --allow-hibernate-to-manage-app-schema

5) Run Server

docker start

cd /nodeA

sudo chown $(whoami) -R ./*

docker-compose -f ./docker-compose-vm1.yaml up

or

manual start

cd /nodeA/notary

java -jar corda.jar

cd /nodeA/partya

java -jar corda.jar

or

script start

Navigate to the root project 

./build/node/runnodes

6) Run Spring Webserver

./gradlew runPartyAServer


Deploy Node : Party B

config network nod : partyb/node.conf and change nodename and node ip:

myLegalName="O=PartyB,L=New York,C=US"

p2pAddress="3.93.6.36:10008"


1) Copy deploy file to aws server Node B

scp -i "./corda-docker.pem" -r ./deploy/nodeB ubuntu@3.93.6.36:~/

scp -i "./corda-docker.pem" -r ./deploy/commands.sh ubuntu@3.93.6.36:~/

2) Access ssh console aws NodeB

ssh -i "corda-docker.pem" ubuntu@ec2-3-93-6-36.compute-1.amazonaws.com

3) run set up Server shell script file command.

cd ~

chmod a+x ./commands.sh

./commands.sh

4) run database migration script

cd /nodeB/partyB

java -jar corda.jar run-migration-scripts --core-schemas --app-schemas --allow-hibernate-to-manage-app-schema

java -jar corda.jar


5) Run Server

User docker start.

cd /nodeB

sudo chown $(whoami) -R ./*

docker-compose -f ./docker-compose-vm2.yaml up

or 
User manual start.

cd /nodeA/notary

java -jar corda.jar

cd /nodeA/partya

java -jar corda.jar


or 

Use scripts to start.

Navigate to the root project

./build/node/runnodes

6) Run Spring Webserver

./gradlew runPartyBServer

Docker start and stop containner

docker ps -a

docker rm <Container id>

docker start <Container id>

docker stop <Container id>

Manual corda install on ubuntu.

1) sudo apt-get update

2) sudo apt-get install openjdk-8-jdk

3) export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64

4) git clone https://github.com/bizqsoft/corda.git

5) Navigate to the Basic\cordapp-example

6) ./gradlew build

7) ./gradlew deployNodes

8) ./build/node/runnodes

9) ./gradlew runPartyAServer