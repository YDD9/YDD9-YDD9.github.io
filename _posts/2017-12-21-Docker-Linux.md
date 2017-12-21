# trouble shoot:

Below error happens when you run docker in a VM or AWS environment, you need to restart docker service
[docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.](https://forums.docker.com/t/cannot-connect-to-the-docker-daemon-at-unix-var-run-docker-sock-is-the-docker-daemon-running/32818/3)
```
sudo service docker stop
sudo service docker start
```

docker config
[image location change](https://forums.docker.com/t/how-do-i-change-the-docker-image-installation-directory/1169)
CAUTION: on CentOS 7, don't follow this.

You can change Docker's storage base directory (where container and images go) using the -g option when starting the Docker daemon.

Ubuntu/Debian: edit your /etc/default/docker file with the -g option: DOCKER_OPTS="-dns 8.8.8.8 -dns 8.8.4.4 -g /mnt"

Fedora/Centos: edit /etc/sysconfig/docker, and add the -g option in the other_args variable: ex. other_args="-g /var/lib/testdir". If there's more than one option, make sure you enclose them in " ". After a restart, (service docker restart) Docker should use the new directory.

Using a symlink is another method to change image storage.
