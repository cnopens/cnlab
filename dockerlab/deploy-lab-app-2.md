# deploy a wordpress lab 
create image :pull msysql /nginx 

##  create mysql container
sudo docker run -d --name mysql -v mysql-data:/valr/lib/mysql -e MYSQL_ROOT_PASSWORD=root -e MYSQL_DATABSE=wordpress mysql

## Create wordpress container:
sudo run -d -e WORDPRESS_DB_HOST=mysql:3306 --link mysql -p 8080:80 wordpress
