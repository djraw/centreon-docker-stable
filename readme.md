Docker using stable repository
==============================

![docker-compose-diagram](doc/images/diagram-docker-compose.png)

Prerequisites:

- Docker-compose: <https://docs.docker.com/compose/>

Docker-compose configuration
----------------------------

![docker-compose-desc](doc/images/docker-compose-desc.png)

Persistent data
---------------

Some directories is necessary to run a minimum persistent data for Centreon

- `/etc/centreon`

This directory contains all configuration files used by Centreon, like as database access

- `/var/spool/centreon/.ssh`

This directory holds the SSH access keys, used for Centreon Central server communication with other remote servers. It is created at environment startup

- `/var/lib/mysql`

This directory contains the data from the Mysql database, managed by the MariaDB image.

Mount point location settings are defined in the docker-compose `Dockerfile` file. These are the minimum mount points to maintain the required data persistence.

How to use
----------

```bash
docker-compose up
```

To always force build of containers, use the command:

```bash
docker-compose up --build
```

For clean all environment:

```bash
docker-compose down -v --rmi local
```

Once the installation finished you can edit centreon user to relax host checking (because docker IP may change) and purge the superadmin:
```
Shell into container
# docker exec -it "centreon-centreon-db-1" sh
# mysql -p

DROP USER 'root'@'%';
USE mysql;
UPDATE user SET Host='%' WHERE User='centreon';
GRANT ALL PRIVILEGES ON centreon.* TO 'centreon'@'%';
GRANT ALL PRIVILEGES ON centreon_storage.* TO 'centreon'@'%';
FLUSH PRIVILEGES;
```

Screencast
----------

[![youtube](http://i.imgur.com/tDiuIm0.png)](https://youtu.be/5GtVYwrKAWA)
