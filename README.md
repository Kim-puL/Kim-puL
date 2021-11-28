
install mariadb server
docker run --name mysql-server -t -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix_pwd" -e MYSQL_ROOT_PASSWORD="root_pwd" --network=zabbix-net -d mariadb --character-set-server=utf8 --collation-server=utf8_bin

install zabbix-java-gateway
docker run --name zabbix-java-gateway -t --network=zabbix-net --restart unless-stopped -d zabbix/zabbix-java-gateway

install zabbix-server-mysql
docker run --name zabbix-server-mysql -t -e DB_SERVER_HOST="mysql-server" -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix_pwd" -e MYSQL_ROOT_PASSWORD="root_pwd" -e ZBX_JAVAGATEWAY="zabbix-java-gateway" --network=zabbix-net -p 10050:10050 --restart unless-stopped -d zabbix/zabbix-server-mysql

install zabbix-web-nginx-mysql
docker run --name zabbix-web-nginx-mysql -t -e ZBX_SERVER_HOST="zabbix-server-mysql" -e DB_SERVER_HOST="mysql-server" -e MYSQL_DATABASE="zabbix" -e MYSQL_USER="zabbix" -e MYSQL_PASSWORD="zabbix" -e MYSQL_ROOT_PASSWORD="kim11112222pul" --network=zabbix-net -p 80:8080 --restart unless-stopped -d zabbix/zabbix-web-nginx-mysql

install zabbix-agent
docker run --name zabbix-agent -e ZBX_HOSTNAME="127.0.0.1,localhost" -e ZBX_SERVER_HOST="zabbix-server" -d zabbix/zabbix-agent
