# docs
configuration:
- https://dev.mysql.com/doc/refman/8.0/en/mysql-cluster-install-configuration.html
installation:
- https://github.com/mlucasdasilva/cluster-mysql/blob/master/docker-compose.yml
- https://dev.mysql.com/doc/refman/8.0/en/mysql-cluster-install-docker.html


# docker
docker pull container-registry.oracle.com/mysql/community-cluster:8.4
docker network create cluster --subnet=172.30.0.0/16
docker run -d -v "./conf/my.cnf:/etc/my.cnf" -v "./conf/mysql-cluster.cnf:/etc/mysql-cluster.cnf" --net=cluster --name=management1 --ip=172.30.0.10 container-registry.oracle.com/mysql/community-cluster:8.4 ndb_mgmd
docker run -d -v "./conf/my.cnf:/etc/my.cnf" -v "./conf/mysql-cluster.cnf:/etc/mysql-cluster.cnf" --net=cluster --name=ndb1 --ip=172.30.0.11 container-registry.oracle.com/mysql/community-cluster:8.4 ndbd
docker run -d -v "./conf/my.cnf:/etc/my.cnf" -v "./conf/mysql-cluster.cnf:/etc/mysql-cluster.cnf" --net=cluster --name=ndb2 --ip=172.30.0.12 container-registry.oracle.com/mysql/community-cluster:8.4 ndbd
docker run -d -v "./conf/my.cnf:/etc/my.cnf" -v "./conf/mysql-cluster.cnf:/etc/mysql-cluster.cnf" --net=cluster --name=mysql1 --ip=172.30.0.20 -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_ROOT_HOST: '%' container-registry.oracle.com/mysql/community-cluster:8.4 mysqld

docker exec -it mysql1 mysql -u root -p 123456


mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';

docker run -it -v "./conf/my.cnf:/etc/my.cnf" -v "./conf/mysql-cluster.cnf:/etc/mysql-cluster.cnf" --net=cluster container-registry.oracle.com/mysql/community-cluster:8.4 ndb_mgm

# docker-compose
docker exec mgm bash