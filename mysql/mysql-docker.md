 docker run --name mysql -p 3308:3306 -e MYSQL_ROOT_PASSWORD=passw0rd -d mysql:5.7

 mysql -h 10.0.0.195 -P 3308 -u root -p

import sql to dataase kid

mysql -u root -p kid < kid.sql