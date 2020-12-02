# WORDPRESS IN DOCKER

## Used images

```
docker pull mysql:5.7
docker pull wordpress
```

## Use docker-compose.yaml to run Wordpress and MySQL in Docker

```
docker-compose up -d
```

## Stop and Remove the containers

```
docker-compose stop
docker-compose rm
```

## Backup the volumes 

First stop the containers and then run the backup

```
docker run --rm --volumes-from wordpress_db_1 -v C:\Users\marti\Projects\wordpress:/backup alpine sh -c "cd /var/lib/mysql && tar cvf /backup/wordpress_db.tar ."
docker run --rm --volumes-from wordpress_wordpress_1 -v C:\Users\marti\Projects\wordpress:/backup alpine sh -c "cd /var/www/html && tar cvf /backup/wordpress_wp.tar ."
```

## Restore the volumes

```
docker run --rm -v wordpress_wordpress:/recover -v C:\Users\marti\Projects\wordpress:/backup alpine sh -c "cd /recover && tar xvf /backup/wordpress_db.tar"
docker run --rm -v wordpress_db:/recover -v C:\Users\marti\Projects\wordpress:/backup alpine sh -c "cd /recover && tar xvf /backup/wordpress_wp.tar"
```

## Check the volumes

```
docker run --rm -v wordpress_db:/var/lib/mysql alpine sh -c "cd /var/lib/mysql && ls"
docker run --rm -v wordpress_wordpress:/var/www/html alpine sh -c "cd /var/www/html && ls"
```
