# docker-dokuwiki
A doker image providing dokwiki with focus of backup in the container 

This project based on the work of https://github.com/istepanov/docker-dokuwiki
also look at http://fast.jumpingcrab.com/wp/blog/2017/02/06/docker-eine-praktische-einfuehrung-am-beispiel-dokuwiki/


with some improvements:
  * It will check the md5sum of the dokuwiki download.
  * It adds php7-openssl and his dependencies to ssl-download plugins within the image (https://github.com/Simon-L/docker-dokuwiki)
  * It creates a volume "lib/plugins" for easy add|remove|backup plugins
  * It creates a volume "var/dokuwiki-backup" witch contains a week of daily Backups per cron
  * Adding timezone support \
    Change the ENV TIMEZONE=America/New_York in the Dockerfile to your needs
  * Adding ntpd support via supervisor
  * ~~Upgrade to the latest (Bugfix) Release 2017-02-19a “Frusterick Manners”~~
  * ~~Upgrade to the latest (Bugfix) Release 2017-02-19b “Frusterick Manners”~~
  * Upgrade to the latest (Bugfix) Release 2017-02-19c “Frusterick Manners”
  * Upgrade Alpine Linux to latest stable version v3.5
  * (NEW) php upload limit set to 64MB \
    Change the value to your needs in the file upload.ini

Build the image

  * git clone https://github.com/migae21/docker-dokuwiki
  * docker build . -t dokuwiki
  * docker run -d -p 80:80 --name dokuwiki dokuwiki

Upgrade from old Versions
```
#validate and save the backups

mkdir dokuwiki-backup

    #if you have a Version without the cron-backup connect to the container and run 
    docker exec -ti dokuwiki /bin/sh
    /etc/periodic/daily/backup(.sh)   
    exit

cd dokuwiki-backup
docker cp dokuwiki:/var/dokuwiki-backup .
docker stop dokuwiki
docker rm dokuwiki

#checkout the new version from my github-repo and build it
git checkout https://github.com/migae21/docker-dokuwiki
cd docker-dokuwiki
docker build . -t dokuwiki

#launch the service
docker run -d -p 80:80 --name dokuwiki dokuwiki

#restore the backup replace $DAY with the weekday of the backup
docker cp $DAY.tgz dokuwiki:/var/dokuwiki-backup/

#exec a shell from the container
docker exec -ti dokuwiki /bin/sh
cd /
tar xzvf /var/dokuwiki-backup/Wednesday.tgz
#finish, launch and test dokuwiki

```

