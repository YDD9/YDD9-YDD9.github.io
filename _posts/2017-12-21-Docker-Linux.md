# trouble shoot:

Below error happens when you run docker in a VM or AWS environment, you need to restart docker service
[docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.](https://forums.docker.com/t/cannot-connect-to-the-docker-daemon-at-unix-var-run-docker-sock-is-the-docker-daemon-running/32818/3)
```
sudo service docker stop
sudo service docker start
```

