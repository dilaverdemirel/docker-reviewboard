docker-reviewboard
==================

Dockerized reviewboard. This container follows Docker's best practices, and DOES NOT include sshd, supervisor, apache2, or any other services except the reviewboard itself which is run with ```uwsgi```.

The requirements are dockerized memcached and data volume. 

This container has two volume mount-points:

- ```/.ssh``` - The default path to where reviewboard stores it's ssh keys.
- ```/media``` - The default path to where reviewboard stores uploaded media.


The container require a MySQL server. 

- ```PGUSER``` - the mysql port. 
- ```PGPASSWORD``` - the mysql puser password. 
- ```PGDB``` - the mysql db name. 
- ```PGPORT``` - the mysql port. 
- ```PGHOST``` - the mysql server address. 


## Quickstart. Run dockerized reviewboard with all dockerized dependencies and MySQL Database parameters.

    # Install memcached
    docker run --name rb-memcached -d -p 11211 sylvainlasnier/memcached

    # Create a data container for reviewboard with ssh credentials and media.
    docker run -v /.ssh -v /media --name rb-data busybox true

    # Run reviewboard
    docker run -it \
    --link rb-memcached:memcached \
    --volumes-from rb-data \
    -p 8000:8000 \
    -e PGHOST="10.10.10.32" \
    -e PGPASSWORD=reviewboard \
    -e PGUSER=reviewboard \
    -e PGDB=reviewboard1 \
    -e PGPORT=3306 \
    ddemirel/docker-reviewboard-mysql

After that, go the url, e.g. ```http://docker_machine_ip_address:8000/```, login as ```admin:admin```, change the admin password, and change the location of your SMTP server so that the reviewboard can send emails. You are all set!
