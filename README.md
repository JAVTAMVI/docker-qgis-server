docker-qgis-server
==================

A simple docker container that runs QGIS MapServer.


**Note** this is a demonstrator project only and you should revise the security
etc of this implementation before using in a production environment.

To use the image, either pull the latest trusted build from 
https://registry.hub.docker.com/u/kartoza/docker-qgis-server/ by doing this:

```
docker pull kartoza/qgis-server
```

or build the image yourself like this:

```
docker build -t kartoza/qgis-server git://github.com/kartoza/docker-qgis-server
```

**Note:** The 'build it yourself' option above will build from the develop branch
wheras the trusted builds are against the master branch.


To run a container do:

```
docker run --name "qgis-server" -p 8081:80 -d -t kartoza/qgis-server
```

Apache environment variables
============================

Apache will make of the following environment variables:

```
	APACHE_SERVERADMIN=admin@localhost
	APACHE_SERVERNAME=localhost
	APACHE_SERVERALIAS=docker.localhost
	APACHE_DOCUMENTROOT=/var/www
	APACHE_RUN_USER=www-data
	APACHE_RUN_GROUP=www-data
	APACHE_LOG_DIR=/var/web/log/apache2
	APACHE_PID_FILE=/var/run/apache2.pid
	APACHE_RUN_DIR=/var/run/apache2
	APACHE_LOCK_DIR=/var/lock/apache2
```

Probably you will want to mount the /web folder with local volume
that contains some QGIS projects. 

```

./build.sh; docker kill server; docker rm server; 
docker run --name="server" \
    -v `pwd`/logs:/var/log/apache2 \
    -v `pwd`/web:/var/www \
    -e APACHE_DOCUMENTROOT="/web" \
    -e APACHE_LOG_DIR="/logs" \
    -e APACHE_SERVERNAME="demo.qgis.org" \
    -d -p 9999:80 \
    kartoza/qgis-server:latest
 docker logs server


```

Replace ``<path_to_local_qgis_project_folder>`` with an absolute path on your
filesystem. That folder should contain the .qgs project files you want to
publish and all the data should be relative to the project files and within the
mounted volume. See https://github.com/kartoza/maps.kartoza.com for an example
of a project layout that we use to power http://maps.kartoza.com




Also consider looking at https://github.com/kartoza/docker-qgis-orchestration
which provides a cloud infrastructure including QGIS Server.


Accessing the services:

http://192.168.99.101:9999/cgi-bin/qgis_mapserv.fcgi?map=/web/helloworld.qgs


-----------

Tim Sutton (tim@linfiniti.com)
May 2014
