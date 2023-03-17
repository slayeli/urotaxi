# urotaxi
To launch the urotaxi application we need to pass the below properties as environment varaibles pointing to the database server details
spring.datasource.url=jdbc:mysql://<dbhost>:<dbport>/urotaxidb
spring.datasource.username=<dbusername>
spring.datasource.password=<dbpassword>

docker network create urotaxinw
docker volume create urotaxidbvol

# launch the mysql server docker container
docker container run --name urotaximysqldb --network urotaxinw --mount type=volume,source=urotaxidbvol,target=/var/lib/mysql -e MYSQL_ROOT_PASSWORD=welcome1 -d mysql:8.0.28

#  create db schema on the mysql server instance
docker cp src/main/db/urotaxidb.sql urotaximysqldb:/
docker container exec -it urotaximysqldb bash

# verify in docker container whether urotaxidb has been created or not
mysql -uroot -pwelcome1
show databases;
use urotaxidb;
show tables;
exit;

# modify the application.yml file in the java project to populate database properties
src/main/resources/application.yml

# build the project
mvn clean verify 

# build the docker image
docker image build -t urotaxi:1.0 .

# run the docker container
open variables.env and populate database information appropriately
docker container run --name urotaxi --network=urotaxinetwork -p 8080:8080 -d urotaxi:1.0


















