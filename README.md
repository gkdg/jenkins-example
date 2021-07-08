## This example uses official jenkins docker image.

### Prerequisites
- docker
- Make

Run the command:
```make run```

Read the logs of the newly created container in order to find initial admin password by running below commands.

Find the container id:
```
docker ps
```

Check the logs of the container to find the initial password:
```
docker logs <container_id>
```

Connect to localhost on port 8080 in order to reach Jenkins UI and enter initial password.

There are three credentials scope. According to projects need you may define one of them or all of them for different use.

1. Global scope
2. System scope
> Only available on jenkins server not for jenkins job.
3. Project scope
> Only comes with multi-pipeline branch option and secrets are naturally has project scope)

If you wanna build only a particular branch go to on console 
```your-pipeline => branch sources => Behaviours => discover branches``` add java regular expression. 

For instance below regular expression will only accept branches main, dev or feature
```
^main|dev|feature.*$
```