# Installing MySQL and MyPHPAdmin on Linux

For this setup, I will be using Docker to manage our servers inside containers (a type of virtual machine).
To learn more about containers and docker see:
* https://blog.jessfraz.com/post/containers-zones-jails-vms/
* https://www.youtube.com/watch?v=Q5POuMHxW-0

You won't need to know too much about Docker as I'll give you the commands to get things going.

The MySQL and MyPHPAdmin servers will run on two separate containers connect by a network.
If you are on an Ubuntu style distro you should be able to install Docker by running the following:

```shell
sudo apt-get update
sudo apt-get install -y docker
sudo systemctl start docker
sudo gpasswd -a $USER docker
newgrp docker
```

Docker should be running, so try `docker info` and you should see a lot of output.

## Creating A Virtual Network
We'll need to create a network for your two containers to communicate.

```shell
docker network create --attachable db_net
```

## Start MySQL
```shell
docker run --name db --rm -e MYSQL_ROOT_PASSWORD=root -v $PWD/datadir:/var/lib/mysql -d --network db_net mysql
```

To see the status of running containers run `docker ps`.

## Start MyPHPAdmin
```shell
docker run --name db_web --rm -d --network db_net  -e PMA_HOST=db -p 8080:80 -e MYSQL_ROOT_PASSWORD=root phpmyadmin/phpmyadmin
```

## Accessing MyPHPAdmin
Run a browser and navigate to http://localhost:8080
