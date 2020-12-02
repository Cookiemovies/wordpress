# WORDPRESS IN DOCKER WITH BACKUP AND RESTORE

## Used images

```
docker pull mysql:5.7
docker pull wordpress
docker pull alpine
```

Alpine image is only used for checking, whether the backup is in place.  

## Use docker-compose.yaml to run Wordpress and MySQL in Docker

```
docker-compose up -d
```

## Backup the volumes 

First stop the containers and then run the backup

```
docker-compose stop
docker run --rm --volumes-from wordpress_db_1 -v C:\Users\marti\Projects\wordpress:/backup alpine sh -c "cd /var/lib/mysql && tar cvf /backup/wordpress_db.tar ."
docker run --rm --volumes-from wordpress_wordpress_1 -v C:\Users\marti\Projects\wordpress:/backup alpine sh -c "cd /var/www/html && tar cvf /backup/wordpress_wp.tar ."
```

When you want to delete everything then  

```
docker-compose rm
docker volume rm wordpress_db
docker volume rm wordpress_wordpress
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

## Store TAR files in Github LFS

As the TAR files in this repository were too huge for Github, there are stored as LFS.  

Follow this, when you want to use it as well for your files:  
Install github-lfs  

```
git init
git lfs install
git lfs track "*.tar"
git add .gitattributes
git add .
git commit -m "First"
git lfs clean
git remote add origin https://www.github.com/cookiemovies/wordpress.git
git push --set-upstream origin master
```
