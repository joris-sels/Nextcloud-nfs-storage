# Nextcloud-NFS-Storage
This is the docker-compose file that explains how to use Nextcloud with multiple storage pools including ones from an NFS share.



version: '2'

volumes:
  nextcloud:
  db:

services:
  db:
    image: mariadb
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW --innodb-file-per-table=1 --skip-innodb-read-only-compressed

    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=#change this
      - MYSQL_PASSWORD=#change this
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud
    restart: always
    ports:
      - 8888:80
    links:
      - db
    volumes:
      - nextcloud:/var/www/html
      - /mnt/name_of_nfs_share/cloud/nextcloud:/var/www/html/data   #path to NFS share + :/var/www/html/data 
      
    environment:
      - MYSQL_PASSWORD=#change this to match the mysql_password above
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
