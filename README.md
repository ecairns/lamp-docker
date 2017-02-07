# LAMP - Docker


This is a simple LAMP docker containers for personal development. Based on https://github.com/laradock/laradock

>Use Docker first and learn about it later.



<a name="Docker"></a>


<a name="List-current-running-Containers"></a>
### List current running Containers
```bash
docker ps
```
You can also use the following command if you want to see only this project containers:

```bash
docker-compose ps
```






<br>
<a name="Close-all-running-Containers"></a>
### Close all running Containers
```bash
docker-compose stop
```

To stop single container do:

```bash
docker-compose stop {container-name}
```






<br>
<a name="Delete-all-existing-Containers"></a>
### Delete all existing Containers
```bash
docker-compose down
```






<br>
<a name="Enter-Container"></a>
### Enter a Container (run commands in a running Container)

1 - First list the current running containers with `docker ps`

2 - Enter any container using:

```bash
docker-compose exec {container-name} bash
```

*Example: enter MySQL container*

```bash
docker-compose exec mysql bash
```

3 - To exit a container, type `exit`.




<br>
<a name="View-the-Log-files"></a>
### View the Log files
The Nginx Log file is stored in the `logs/nginx` directory.

However to view the logs of all the other containers (MySQL, PHP-FPM,...) you can run this:

```bash
docker logs {container-name}
```

<br>
<a name="Rebuild Docker Container"></a>
### Rebuild docker 
Re-build your PHP-FPM Container `docker-compose build php-fpm`.

<br/>
<a name="Change-the-timezone"></a>
### Change the timezone and other php.ini

EDIT php-fpm/php.ini


## Credits

- [Laradock](https://github.com/laradock) (laradock) 


## License

[MIT License](https://github.com/laradock/laradock/blob/master/LICENSE) (MIT)

